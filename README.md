# CacheKit

Biblioteca Swift de cache HTTP em memória + disco, com política LRU multi-nível, invalidação por ETag/Cache-Control, observabilidade nativa, e suporte a cache offline-first com revalidação em background. Plugável em `URLSession`, `Alamofire`, ou pipeline custom — sem assumir transport.

> **Status**: estável (`v3.4`), produção em **Vivaldi**, **Hatch** e **PocketScale** (~3M MAU agregado).
> **Última auditoria de segurança**: março/2026 (17 findings — todas endereçadas, ver `SECURITY.md`).

---

## Por que existe

`URLCache` do Foundation tem 3 problemas que doem em produção:

1. **Política única por sessão** — não dá pra ter `preferCache` em endpoints de catálogo e `preferNetwork` em endpoints de saldo no mesmo `URLSession`.
2. **Invalidação opaca** — `URLCache.removeAllCachedResponses()` é tudo-ou-nada; não há "invalida só `/users/me`".
3. **Zero visibilidade** — nenhuma métrica de hit rate, byte usage, evictions. Em produção isso é a diferença entre 80ms p50 e 800ms p50, e você não tem como saber.

CacheKit resolve com uma camada fina (~2k LOC, zero deps) por cima de qualquer transport.

---

## Conceitos

### Endpoint key

Toda entrada de cache é endereçada por `EndpointKey`:

```swift
public struct EndpointKey: Hashable, Sendable {
    let method: HTTPMethod
    let url: URL
    let varyingHeaders: [String: String]   // headers em `Vary:` do response
    let bodyHash: SHA256Digest?            // só pra POST/PUT idempotentes
}
```

`varyingHeaders` é populado a partir do `Vary:` do response anterior — segue RFC 7234. `bodyHash` é opt-in via `CachePolicy.cachableBody`; default é `nil` (cache só GET).

### CacheStore

Protocolo. 3 implementações stock:

| Store | Backend | Thread-safe | Multi-process | Tamanho típico |
|---|---|---|---|---|
| `MemoryCacheStore` | `Dictionary` + `OSAllocatedUnfairLock` | ✅ | n/a | 2-32 MB |
| `DiskCacheStore` | filesystem + SQLite index | ✅ via `actor` | ❌ (ver Limitações) | 50-500 MB |
| `TieredCacheStore` | mem na frente + disk atrás | ✅ | ❌ | combinado |

`TieredCacheStore` promove entradas em hit do disco (move pra memória) e demota em pressão (evict da memória → disco se ainda dentro do TTL).

### CachePolicy

Decisão por request. Não global.

```swift
public enum CachePolicy: Sendable {
    case bypass                              // sempre rede, nunca grava
    case preferCache(maxAge: TimeInterval)   // serve cache se fresh, senão rede
    case preferNetwork(staleAfter: TimeInterval) // tenta rede, fallback cache se < staleAfter
    case exclusiveCache                       // nunca rede, throws .offline se miss
    case revalidate                          // sempre rede com If-None-Match/If-Modified-Since
}
```

`preferNetwork(staleAfter:)` é o modo recomendado pra dados sensíveis a frescura (saldo, notificações). Tenta rede primeiro; se rede falhar e a cópia cached tiver < `staleAfter` segundos, serve a cached marcada com `.staleResponse`. Fail mode explícito > silent fallback.

### CacheInterceptor

Adapter que pluga em pipelines:

- **URLSession adapter** — extension `URLSession.with(_ interceptor:)` retorna `Session` que intercepta `dataTask`/`data(for:)`.
- **Alamofire adapter** — `Session.RequestInterceptor` impl em `CacheKitAlamofire` (target separado, dep opcional).
- **Custom** — protocolo `CacheableRequest` aceita qualquer transport via `func sendBypassingCache() async throws -> CachedResponse`.

---

## Política de invalidação — precedência

Quando uma request chega, o pipeline avalia em ordem:

1. **`CachePolicy.bypass`** → vai direto pra rede, nunca consulta store.
2. **ETag conditional** — se a entrada existe e tem `ETag`, manda `If-None-Match`. Server respondendo `304` serve do cache (com headers atualizados se vier).
3. **`Last-Modified` conditional** — fallback se não tem ETag. Manda `If-Modified-Since`.
4. **`Cache-Control: max-age`** — se entrada existe e ainda dentro do max-age do server, serve direto sem rede.
5. **TTL local** — `CachePolicy.preferCache(maxAge:)` aplica TTL definido pelo cliente (sobrepõe server).
6. **Stale-while-revalidate** — se config global `staleWhileRevalidate=true`, entradas expiradas servem imediatamente E disparam revalidação em background.

`Cache-Control: no-store` do server **sempre** ganha sobre qualquer policy do cliente — RFC 9111 §5.2.2.5.

### Negative caching

Por default, respostas `4xx`/`5xx` **não são cacheadas**. Pode habilitar via `CachePolicy.cacheNegativeResponses(maxAge: 30)` em endpoints onde 404 é informação válida (ex: `/users/{id}/exists`).

---

## Observabilidade

`CacheMetrics` é um `actor` que expõe contadores como `AsyncStream`:

```swift
actor CacheMetrics {
    var hitsByEndpoint: [EndpointKey: Int]
    var missesByEndpoint: [EndpointKey: Int]
    var evictionsByReason: [EvictionReason: Int]   // .lru, .ttl, .manual, .diskPressure, .schemaVersion
    var bytesOnDisk: Int64
    var bytesInMemory: Int64
    var revalidationsByOutcome: [RevalidationOutcome: Int]   // .notModified, .modified, .failed
}
```

3 caminhos pra consumir:

- **Real-time stream** — `metrics.events` é `AsyncStream<CacheEvent>`. Plugavel em logging/telemetry.
- **Snapshot periódico** — `metrics.snapshot()` retorna `CacheMetricsSnapshot` (struct `Sendable`). Bom pra mandar em payload de telemetry a cada 60s.
- **OSLog signpost** — built-in via `OSSignposter`. Aparece no Instruments → "Points of Interest" sem setup.

### Hit rate

Calculado on-demand: `metrics.hitRate(for: endpoint, window: .lastHour)`. Janela suportada: `.allTime`, `.lastHour`, `.last24h`. Não há ring buffer infinito — fora dessas janelas, métricas são agregadas via OSLog signpost storage.

---

## Persistência

### DiskCacheStore — layout

```
Caches/CacheKit/
├── index.sqlite           ← schema versão 4 (ver Migrations)
├── index.sqlite-wal
├── index.sqlite-shm
└── blobs/
    ├── ab/cd/ef/...        ← <sha256[0:2]>/<sha256[2:4]>/<sha256[4:6]>/<sha256[6:]>.blob
    └── ...
```

Sharding em 3 níveis evita listings lentos quando o cache cresce (>10k entries em 1 dir trava o Finder). Schema do índice:

```sql
CREATE TABLE entries (
    key_hash       TEXT PRIMARY KEY,         -- SHA256 do EndpointKey.canonicalString
    endpoint_url   TEXT NOT NULL,
    method         TEXT NOT NULL,
    blob_path      TEXT NOT NULL,
    bytes          INTEGER NOT NULL,
    created_at     INTEGER NOT NULL,         -- unix epoch
    last_hit_at    INTEGER NOT NULL,
    hit_count      INTEGER NOT NULL DEFAULT 0,
    etag           TEXT,
    last_modified  TEXT,
    expires_at     INTEGER,                  -- max-age + created_at, nullable
    schema_version INTEGER NOT NULL          -- pra invalidar em deploy de breaking change
);
CREATE INDEX idx_entries_lru ON entries(last_hit_at);
CREATE INDEX idx_entries_ttl ON entries(expires_at) WHERE expires_at IS NOT NULL;
```

### Migrations

Schema bumps invalidam todo o cache no boot — uma migration de schema é traduzida como "esses dados não são mais válidos". Não há tentativa de upgrade-in-place. Disco vazia, mem zerada, métricas resetadas.

Versão atual: `4` (introduziu `hit_count` e `schema_version` em v3.0).

---

## Concorrência

Tudo `Sendable`. Stack atual:

- `MemoryCacheStore` — `OSAllocatedUnfairLock<State>` (Swift 5.9+). Reads são lock-free via snapshot CoW do dicionário; writes pegam o lock.
- `DiskCacheStore` — `actor` serializa I/O. Reads usam read-only DB connections via pool (até 4 simultâneas).
- `CacheInterceptor` — stateless, thread-safe trivialmente.
- `CacheMetrics` — `actor`, eventual consistency com snapshot.

Reentrância: nenhum lock é mantido durante I/O de disco ou rede. Locks são pra mutação de estado in-memory, sempre `<1µs`.

### Cancellation

Toda API `async throws` propaga cancellation:

```swift
func fetch(_ endpoint: Endpoint) async throws -> CachedResponse {
    try Task.checkCancellation()
    if let cached = try await store.read(key) { return cached }
    try Task.checkCancellation()
    let response = try await transport.send(request)
    try await store.write(key, response)
    return response
}
```

Cancelamento durante `store.write` é "best-effort" — write pode completar mesmo após cancel, mas a entrada nunca fica em estado parcial (write é atômico via temp file + rename).

---

## Segurança

### Auditoria março/2026

17 findings, todas endereçadas em v3.3.x:

- **#03** (high) — `DiskCacheStore` aceitava `URL` com `..` no path key, escapava o `Caches/CacheKit/` sandbox. Fix: `EndpointKey.canonicalString` usa SHA256 (nunca path-derived).
- **#07** (medium) — `Set-Cookie` em response cacheada vazava entre user sessions após logout. Fix: `Set-Cookie` é critério de exclusão hard (não opt-in).
- **#11** (medium) — TLS pinning não respeitado em revalidações background (rodavam em URLSession default). Fix: revalidações usam o mesmo `URLSession` do interceptor.
- **#14** (low) — schema do SQLite tinha CHECK constraints só em DEBUG. Fix: constraints sempre on.

`SECURITY.md` tem o changelog completo.

### Threat model

CacheKit não criptografa dados em disco. **Premissa**: o sandbox do iOS/macOS já isola por app. Se você cacheia dados que precisam de criptografia em repouso (PII, healthcare), use `DataProtection.Class.completeUntilFirstUserAuthentication` no `Caches/` directory ou implemente um `CacheStore` próprio com `CryptoKit`.

---

## Performance

Benchmarks em iPhone 15 Pro (Swift 5.9, release config), 10k entries no `TieredCacheStore`:

| Operação | p50 | p99 |
|---|---|---|
| `read` (memory hit) | 1.2 µs | 8.4 µs |
| `read` (disk hit, cold) | 380 µs | 1.2 ms |
| `read` (disk hit, warm) | 12 µs | 47 µs |
| `write` (memory) | 2.8 µs | 18 µs |
| `write` (disk, 50KB blob) | 1.4 ms | 6.2 ms |
| `evict` (LRU sweep, 100 entries) | 8.1 ms | 22 ms |

`disk hit warm` reflete cache de página do filesystem; primeiro acesso (cold) é dominado por seek + I/O de page do SQLite. Evictions são amortizadas — rodam em `.background` Task quando `bytesOnDisk > diskLimit * 1.1`.

---

## Limitações

- **Não multi-process**. `DiskCacheStore` corrompe o índice se app abrir 2x ao mesmo tempo (ex: Catalyst + iOS-on-Mac via Designed for iPad). Tracked em [#42](https://example.com/issues/42), planejado pra v4.0 com SQLite WAL2 + retry-on-busy.
- **Não fragmenta blobs**. Response de 50MB ocupa 1 arquivo de 50MB. Para downloads grandes use `URLSessionDownloadTask` direto.
- **Não suporta partial content** (`Range:` requests). Response com `206 Partial Content` é bypass.
- **`Vary: *`** desabilita cache pra esse endpoint. Não há heurística de "talvez varie só por `Authorization`".

---

## Roadmap

- **v4.0** (Q3/2026) — multi-process safety, partial content support, predictive prefetch via `CacheHints`.
- **v4.1** — encrypted-at-rest opt-in com chave derivada de `LAContext` biometria.
- **v5.0** (TBD) — sync cross-device via iCloud Drive (read-only mirror).

---

## Migração

### De v2 → v3

`v3` removeu `CocoaPods`. `Package.swift` only. Se ainda em CocoaPods, o último release suportado é `v2.7.4`.

`CacheStore` ganhou `Sendable` requirement (Swift 6). Custom impls precisam adicionar `@unchecked Sendable` ou modelar com `actor`.

### De v3.0 → v3.4

Sem breaking changes. Schema do SQLite foi de v2 → v4 (auto-invalidação no primeiro boot pós-update — usuário perde cache uma vez).

---

## Requisitos

- iOS 16+ / macOS 13+ / tvOS 16+ / watchOS 9+
- Swift 5.9+
- SPM (CocoaPods removido na v3.0; Carthage nunca foi suportado)

---

## Licença

MIT. `LICENSE` na raiz.
