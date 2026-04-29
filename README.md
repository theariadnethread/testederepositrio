# CacheKit

Biblioteca Swift de cache HTTP em memória + disco com política LRU e invalidação por ETag.

## Por que existe

Apps mobile fazem dezenas de requests redundantes ao reabrir telas. `URLSession` traz `URLCache` no padrão, mas o controle é grosseiro: política única por sessão, invalidação opaca, e nenhuma visibilidade de hit/miss em produção. **CacheKit** resolve isso com uma camada fina por cima do `URLSession` que dá controle por endpoint, métricas e modos de evicção previsíveis.

## Componentes

- **`CacheStore`** — protocolo. Implementações:
  - `MemoryCacheStore`: dicionário thread-safe com LRU sobre soft limit em bytes.
  - `DiskCacheStore`: escreve em `Caches/CacheKit/<sha256>` (sandbox-friendly), índice em SQLite.
  - `TieredCacheStore`: memória na frente, disco atrás. Promove em hit do disco.
- **`CachePolicy`** — política por request: `.bypass`, `.preferCache(maxAge:)`, `.preferNetwork(staleAfter:)`, `.exclusiveCache`.
- **`CacheInterceptor`** — adapter que pluga em qualquer pipeline (URLSession, Alamofire, async/await direto).

## Política de invalidação

3 caminhos suportados, em ordem de precedência:

1. **ETag** — server manda `ETag: "abc"`, próxima request vai com `If-None-Match`. 304 = serve do cache.
2. **`Cache-Control: max-age`** — respeita o TTL do server. Sobrescreve apenas se `CachePolicy` permitir staleness.
3. **TTL local** — opcional via `CachePolicy.preferCache(maxAge:)`. Útil para endpoints sem cache headers honestos.

## Métricas

`CacheMetrics` expõe contadores Combine que o app pode publicar em telemetria:

- `hitsByEndpoint: [Endpoint: Int]`
- `missesByEndpoint: [Endpoint: Int]`
- `evictionsByReason: [EvictionReason: Int]` (`.lru`, `.ttl`, `.manual`, `.diskPressure`)
- `bytesOnDisk`, `bytesInMemory`

## Exemplo

```swift
let cache = TieredCacheStore(
    memoryLimit: 8.megabytes,
    diskLimit: 100.megabytes
)
let session = URLSession(configuration: .default)
    .with(CacheInterceptor(store: cache, defaultPolicy: .preferCache(maxAge: 300)))

let response: UserProfile = try await session.fetch(
    UserProfile.endpoint(id: userId),
    cachePolicy: .preferNetwork(staleAfter: 60)
)
```

## Concorrência

Tudo `Sendable`. `MemoryCacheStore` usa `OSAllocatedUnfairLock`. `DiskCacheStore` serializa writes via `actor`. Reads em paralelo são lock-free no `MemoryCacheStore` (snapshot CoW do dicionário).

## Limites conhecidos

- Não cacheia respostas com `Set-Cookie` (evita vazamento entre sessões).
- `DiskCacheStore` **não** é multi-process safe — abrir o app duas vezes ao mesmo tempo (Catalyst) corrompe o índice. Tracked em [#42](https://example.com/issues/42).
- LRU de memória usa contador monotônico, não timestamps reais. Empate de hit count desempata por ordem de inserção.

## Requisitos

- iOS 16+ / macOS 13+
- Swift 5.9+
- SPM (Cocoapods removido na v3)

## Licença

MIT.
