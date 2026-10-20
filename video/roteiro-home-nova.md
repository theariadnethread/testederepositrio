# Home nova: onde estamos e pra onde vamos

Roteiro do vídeo de estudo, com as falas por capítulo. Estado contado em **20/10/2026**.

O que é **HIPÓTESE** ou **PROPOSTA** está dito assim no vídeo e marcado aqui.

## Parte 1 · 11:03

- `00:00` A pergunta
- `00:43` As nove peças
- `02:33` HS01 a HS04
- `04:09` O problema
- `06:11` HS05
- `08:20` O mapper
- `08:56` As 5 regras

### 00:00 · A PERGUNTA

- `00:00` Se o back mudar o nome de um campo amanhã, quantas peças do app quebram?
- `00:04` Hoje, a resposta é cinco, no mínimo.
- `00:07` Neste vídeo, eu explico por que é cinco, e como a gente leva pra uma.
- `00:11` Hoje é 20/10/2026.
- `00:14` Vou contar onde estamos, o que já fizemos e pra onde vamos.
- `00:17` O fio é a Maria, uma cliente usando o app.
- `00:20` Cada peça técnica vira algo que acontece com ela.
- `00:23` O projeto: uma Home nova num app grande de banco e seguros.
- `00:27` No iOS e no Android.
- `00:29` O back diz o que aparece na Home. O app desenha.
- `00:32` Em dezembro, a Maria vê o cartão, com a fatura e o limite.
- `00:36` Vê a conta com o saldo, os seguros e o consórcio.
- `00:39` O mercado chama isso de tela guiada pelo servidor.

### 00:43 · AS NOVE PEÇAS

- `00:43` No app, a Home é feita de nove peças.
- `00:46` Elas formam uma fila, da esquerda pra direita.
- `00:49` Esse desenho vai voltar várias vezes.
- `00:51` N1, a rede: busca a Home no back.
- `00:54` N2, o conferente: confere a resposta.
- `00:57` Ele olha a identidade, a data do conteúdo e se veio degradada.
- `01:01` Degradada é incompleta: faltou algum pedaço.
- `01:04` N3, a guarda: guarda no celular a última Home.
- `01:07` Criptografada e sem dinheiro.
- `01:09` N4, o montador: monta o estado da tela.
- `01:13` Ele decide o que cada bloco e cada item desenha.
- `01:16` N5, os desenhistas: desenham cada componente.
- `01:19` O card de produto, o grupo, a oferta, os atalhos e o topo.
- `01:23` N6, as ações: executam o toque da Maria.
- `01:26` Abrem uma tela do app, o deeplink, ou um endereço, a url.
- `01:31` N7, o olho e os valores: mostram e escondem os valores.
- `01:35` E buscam o saldo na hora.
- `01:37` N8, o maestro: orquestra tudo.
- `01:39` A abertura e os pedidos da Maria.
- `01:42` N9, o recibo: anota os marcos de cada abertura.
- `01:45` E também os crashes e os diagnósticos.
- `01:48` Agora, a Maria abre o app.
- `01:50` O maestro começa a abertura.
- `01:52` Tem Home guardada? Ela aparece na hora, sem esqueleto.
- `01:56` Não tem? Aparece o esqueleto, o desenho de carregamento.
- `01:59` Ao mesmo tempo, o maestro sempre busca a rede.
- `02:02` A resposta pode ser de quatro jeitos.
- `02:05` 304: nada mudou.
- `02:07` 200: uma Home nova.
- `02:09` Degradada: veio incompleta.
- `02:11` Mais velha que a guardada: é descartada.
- `02:13` Aí o montador monta, e os desenhistas desenham.
- `02:16` O maestro manda gravar a cópia nova.
- `02:19` E o recibo anota tudo.
- `02:20` Agora, o dinheiro.
- `02:22` A fatura e o limite vêm dentro da resposta do back.
- `02:25` Vêm como valor protegido.
- `02:27` O saldo, o app busca na hora.
- `02:29` E a cópia no celular nunca guarda dinheiro.

### 02:33 · HS01 A HS04

- `02:33` Agora, o que já fizemos.
- `02:35` São quatro histórias: da HS01 à HS04.
- `02:39` É a fundação.
- `02:40` Lembra, a HS01 fez as tomadas.
- `02:43` Cada uma das nove peças ganhou nome e interface no código.
- `02:47` Igual no iOS e no Android.
- `02:49` Ainda sem nada dentro.
- `02:50` Pra quê? Cada história seguinte liga a sua peça no lugar certo.
- `02:54` Sem inventar formato.
- `02:56` A HS02 fez a primeira ligação de ponta a ponta.
- `03:00` A Home abre, lê a cópia guardada e desenha um bloco.
- `03:03` Esse bloco ainda é um card cinza provisório.
- `03:06` Onde faltava peça de verdade, entrou um dublê.
- `03:09` A HS03 deu ao maestro a abertura inteira.
- `03:12` Ele decide sozinho.
- `03:14` Mostra o esqueleto, lê a cópia primeiro e busca a rede sempre.
- `03:18` Trata cada resposta, grava, descarta o que chegou velho e encerra.
- `03:22` Tudo testado por uma tabela de transições.
- `03:25` Um teste por linha.
- `03:26` A HS04 faz o maestro atender a Maria.
- `03:30` O puxar pra atualizar.
- `03:31` O Tentar novamente de um card que falhou.
- `03:34` A bandeira atualizar: outra parte do app avisa que os dados dela mudaram.
- `03:38` Por exemplo, depois de uma contratação.
- `03:40` E a carona.
- `03:41` Se a abertura ainda está buscando, o pedido dela aproveita essa busca.
- `03:46` Em vez de fazer outra.
- `03:47` Onde isso está em 20/10?
- `03:49` HS01 e HS02: código nas duas plataformas.
- `03:54` Estão entre revisão e merge.
- `03:56` HS03: código no iOS, em revisão.
- `03:59` No Android, ainda estamos conferindo quanto foi feito.
- `04:03` HS04: escrita e publicada.
- `04:06` Mas o código ainda não foi feito.

### 04:09 · O PROBLEMA

- `04:09` Agora, o problema que apareceu.
- `04:11` As quatro histórias falam a língua do back.
- `04:13` O JSON que chega da rede é o mesmo objeto que passa por tudo.
- `04:17` Pela rede, pelo conferente, pela guarda.
- `04:19` Pelo montador e pelo maestro.
- `04:21` Agora, o back muda o nome de um campo.
- `04:24` Essas cinco peças quebram.
- `04:26` É a resposta da pergunta do começo: cinco, no mínimo.
- `04:29` Em 20/10, a gente decidiu três coisas.
- `04:32` Primeiro: o back continua no formato dele.
- `04:35` É o contrato 1.6.
- `04:37` A evolução da 1.4 e da 1.5, com os nomes de hoje.
- `04:41` Segundo: o app passa a ter o modelo dele, a ficha.
- `04:44` É um molde por componente da tela.
- `04:46` Um pro card de produto, um pro grupo, um pra oferta, e assim por diante.
- `04:51` A ficha vem do desenho do Figma.
- `04:53` E só existe dentro do app.
- `04:55` Terceiro: no meio fica o adaptador, o mapper.
- `04:58` Ele pega o JSON da 1.6 e devolve a ficha.
- `05:01` É a única peça que fala a língua do back.
- `05:04` E a tradução de cada produto? Ela mora no back.
- `05:07` Cartão, conta, seguro e consórcio viram componentes genéricos lá.
- `05:11` Entrou um seguro novo?
- `05:12` Ele cabe no card de produto. Então só o back muda.
- `05:16` E o app nem fica sabendo.
- `05:17` Agora, o contrato 1.6, só o essencial.
- `05:21` A rota é GET /home/v1/surface, sem corpo.
- `05:25` Vão cinco cabeçalhos: a autorização, a conta, o id do pedido, a versão e a plataforma.
- `05:31` A resposta é um envelope mais as sections.
- `05:34` O envelope tem a revisão, a data de geração e o contexto.
- `05:38` Cada seção tem id, type, status, props e items.
- `05:42` Em dezembro, são cinco tipos.
- `05:44` Os atalhos, o grupo de produtos, o card de produto, a oferta e o aviso do produto.
- `05:49` O card de produto tem título, até duas linhas, uma ação e até dois botões.
- `05:54` O valor pode ser de três jeitos.
- `05:56` text: texto. sensitive: protegido, como a fatura.
- `06:00` live: buscado na hora, como o saldo.
- `06:02` A ação é um deeplink ou uma url.
- `06:05` E se um produto falhar? Ele vem no lugar dele, com status: unavailable.

### 06:11 · HS05

- `06:11` Agora, o centro do vídeo: a HS05.
- `06:14` Ela faz as tomadas novas.
- `06:16` São quatro partes.
- `06:17` Parte um: as fichas viram tipos de verdade no código.
- `06:21` Nas duas plataformas.
- `06:22` O card de produto, o grupo, a oferta e o aviso.
- `06:26` A falha total, o fullError: o card de erro com Tentar novamente.
- `06:30` Os atalhos, o lugar do insight, o valor e a etiqueta.
- `06:33` A ação, e a marca de item quebrado, o discarded.
- `06:36` Entram também os fios entre as peças.
- `06:39` O estado do olho, o estado da tela, os pedidos da Maria e os diagnósticos.
- `06:43` Os pedidos são três: puxar, olho e tentar de novo.
- `06:47` Parte dois: tirar a língua do back das peças que já existem.
- `06:50` A rede, o conferente, a guarda, o montador e o maestro.
- `06:54` Sai o JSON, entra a ficha.
- `06:56` O comportamento não muda.
- `06:58` A prova? Os testes da HS01 à HS04 continuam passando.
- `07:03` Parte três: o catálogo.
- `07:05` Pra cada tipo de item, quem lê, quem traduz e quem desenha.
- `07:09` Amanhã chega um card novo.
- `07:10` É uma peça nova mais uma linha no catálogo.
- `07:13` Sem mexer no resto.
- `07:15` Parte quatro: os exemplos e os dublês.
- `07:17` Entra tal JSON da 1.6, sai tal ficha.
- `07:20` O iOS e o Android usam os mesmos exemplos.
- `07:23` Cada dev começa no dia seguinte, sem esperar o back.
- `07:27` E o que a HS05 não faz?
- `07:29` Não faz o leitor nem o adaptador de verdade. Isso é da HS06.
- `07:34` Não desenha nada novo.
- `07:36` E não muda comportamento.
- `07:37` Por que ela vem antes de tudo?
- `07:39` Sem ela, quatro devs em paralelo inventam quatro cards.
- `07:43` Na hora de juntar, nada encaixa.
- `07:45` É ela que torna verdade o depois eu só mudo o mapper.
- `07:48` Com a língua do back espalhada, mudar o JSON quebra cinco peças.
- `07:52` E os exemplos comuns garantem o mesmo resultado no iOS e no Android.
- `07:56` Agora, uma hipótese em estudo. **[HIPÓTESE]**
- `07:58` A HS05 do plano é grande. **[HIPÓTESE]**
- `08:01` Ela junta criar as tomadas com migrar as peças velhas. **[HIPÓTESE]**
- `08:04` Mas o paralelo só precisa da primeira parte. **[HIPÓTESE]**
- `08:07` Estamos testando se a HS05 pode travar só as tomadas e os exemplos. **[HIPÓTESE]**
- `08:12` E o dono de cada peça migra a sua. **[HIPÓTESE]**
- `08:14` Se der, o paralelo começa antes. **[HIPÓTESE]**
- `08:17` Repito: por enquanto, é hipótese. **[HIPÓTESE]**

### 08:20 · O MAPPER

- `08:20` Agora: o que o mapper resolve e o que não resolve.
- `08:23` Resolve três casos.
- `08:25` O back mudou um campo.
- `08:26` Entrou um produto novo que cabe num componente que já existe.
- `08:30` A forma do JSON mudou.
- `08:31` Nos três, só o adaptador muda.
- `08:34` Agora, o que ele não resolve.
- `08:36` Mudança no desenho.
- `08:37` O designer muda a cara de um componente no Figma.
- `08:40` Muda a ficha.
- `08:41` E com ela, os tipos da HS05 e o desenhista.
- `08:45` E comportamento.
- `08:46` O olho, a cópia, o tentar de novo e a ordem da abertura.
- `08:50` Nada disso passa pelo mapper.
- `08:52` Por isso as fichas passaram por cinco rodadas de revisão antes.

### 08:56 · AS 5 REGRAS

- `08:56` Agora, cinco regras novas que mudam os contratos. **[PROPOSTA]**
- `09:00` Atenção: são uma proposta. **[PROPOSTA]**
- `09:02` Ainda não aprovada. **[PROPOSTA]**
- `09:03` Vou mostrar cada uma como uma cena da Maria. **[PROPOSTA]**
- `09:06` Regra um: um produto falhou. **[PROPOSTA]**
- `09:08` O serviço do cartão caiu. **[PROPOSTA]**
- `09:10` Mas ontem, a Maria viu o cartão. **[PROPOSTA]**
- `09:12` E o celular guardou uma cópia, sem valores. **[PROPOSTA]**
- `09:15` Ela abre o app. **[PROPOSTA]**
- `09:16` Vê a cópia de ontem, sem aviso. **[PROPOSTA]**
- `09:19` Agora ela puxa pra atualizar. **[PROPOSTA]**
- `09:21` Vê o card de erro, com Tentar novamente. **[PROPOSTA]**
- `09:23` Por quê? Ela pediu dado novo. **[PROPOSTA]**
- `09:25` Mostrar o velho seria mentir. **[PROPOSTA]**
- `09:27` E sem cópia? É sempre o card de erro. **[PROPOSTA]**
- `09:30` Regra dois: um item veio quebrado. **[PROPOSTA]**
- `09:32` O back mandou o cartão 2 com um campo errado. **[PROPOSTA]**
- `09:35` O adaptador não some com ele. **[PROPOSTA]**
- `09:37` Ele deixa a marca discarded no lugar. **[PROPOSTA]**
- `09:39` E manda um diagnóstico pro recibo. **[PROPOSTA]**
- `09:41` Assim, o app sabe a diferença. **[PROPOSTA]**
- `09:44` O back tirou o item de propósito? O item sai. **[PROPOSTA]**
- `09:46` O item veio quebrado? Fica a versão anterior, sem aviso. **[PROPOSTA]**
- `09:50` Sem a marca, o app acharia que o cartão foi cancelado. **[PROPOSTA]**
- `09:53` E no puxar, vale o card de erro. **[PROPOSTA]**
- `09:55` Regra três: o que é fechar o app. **[PROPOSTA]**
- `09:58` Fechar é o processo morrer. **[PROPOSTA]**
- `10:00` Ela tira o app da lista, o sistema mata ou o app cai. **[PROPOSTA]**
- `10:03` Ir pro WhatsApp e voltar não é fechar. **[PROPOSTA]**
- `10:06` O olho e os valores ficam como estavam. **[PROPOSTA]**
- `10:08` A miniatura na lista de apps abertos sai coberta. **[PROPOSTA]**
- `10:11` Se o app morreu, na próxima vez o olho começa fechado. **[PROPOSTA]**
- `10:15` Regra quatro: a falha que chega tarde. **[PROPOSTA]**
- `10:18` A Maria tocou no olho. **[PROPOSTA]**
- `10:19` A rede demorou. **[PROPOSTA]**
- `10:20` E ela fechou o olho de novo. **[PROPOSTA]**
- `10:22` Aí, chega a falha. **[PROPOSTA]**
- `10:24` Essa falha é jogada fora. **[PROPOSTA]**
- `10:26` No próximo toque, o app busca de novo. **[PROPOSTA]**
- `10:28` E se a falha chegou com o olho aberto? **[PROPOSTA]**
- `10:31` A mensagem fica até ela tocar no olho de novo. **[PROPOSTA]**
- `10:33` Regra cinco: a abertura anterior foi crash? **[PROPOSTA]**
- `10:36` Antes de abrir a Home, o app confere. **[PROPOSTA]**
- `10:39` A última abertura terminou em crash? **[PROPOSTA]**
- `10:41` Se terminou, ele não usa a cópia. **[PROPOSTA]**
- `10:43` Se nem consegue ler essa anotação, também não usa. **[PROPOSTA]**
- `10:46` A Home abre esperando a rede. **[PROPOSTA]**
- `10:48` Um pouco mais lenta, uma vez. **[PROPOSTA]**
- `10:51` Em troca, não entra num loop de crash. **[PROPOSTA]**
- `10:53` É a única das cinco que tem custo pra Maria. **[PROPOSTA]**
- `10:56` Na parte dois: o resto do plano, as junções, o back e as datas.

## Parte 2 · 07:48

- `00:00` Onde paramos
- `00:19` HS06 a HS11
- `01:55` HS12 a HS17
- `03:34` As junções
- `05:15` O back
- `06:20` As datas

### 00:00 · ONDE PARAMOS

- `00:00` Parte dois. Lembra da fila das nove peças?
- `00:03` A HS01 à HS04 fizeram a fundação.
- `00:08` A HS05 troca a língua do back pela ficha.
- `00:11` Agora, o resto do plano: da HS06 à HS17.
- `00:16` Cada história acende a sua peça.

### 00:19 · HS06 A HS11

- `00:19` HS06: o conferente e o adaptador.
- `00:22` É o caminho real.
- `00:23` O leitor da 1.6, o conferente de verdade e o adaptador puro.
- `00:27` O adaptador puro é o de-para da 1.6 pra ficha.
- `00:31` E o que fazer com um tipo que o app não conhece.
- `00:33` Mais os diagnósticos.
- `00:35` E os cenários de um servidor de mentira, o WireMock, com a 1.6.
- `00:40` HS07: a guarda.
- `00:42` A cópia tipada e versionada.
- `00:44` Gravar, ler, apagar e criptografia.
- `00:47` O backup, o tamanho máximo e o logout.
- `00:50` E só grava se for mais nova.
- `00:51` HS08: o montador.
- `00:54` Primeiro, as regras das fichas.
- `00:56` Um aviso por vez.
- `00:58` Quem ganha quando o olho e um aviso disputam o lugar.
- `01:01` Os cards de erro e o Tentar novamente.
- `01:03` A origem do pedido e a carona.
- `01:06` Depois, as regras de aplicação.
- `01:08` Trocar sem pular a tela quando o dedo dela está na tela.
- `01:11` Aplicar com a Home fora da tela.
- `01:13` E avisar o maestro que pintou.
- `01:15` Por fim, o cálculo do que pode ser gravado.
- `01:18` Sem dinheiro.
- `01:19` HS09: o card de produto desenhado.
- `01:22` Os campos e a cara de cada estado do valor.
- `01:25` Os avisos, a setinha e o tamanho.
- `01:28` O texto cortado e o leitor de tela.
- `01:30` HS10: o grupo.
- `01:32` Um card só, ou um carrossel.
- `01:34` Os botões que trocam depois de assentar.
- `01:36` O pedaço do próximo card aparecendo.
- `01:39` Altura igual, e menos movimento pra quem pede.
- `01:42` HS11: a oferta, o aviso e o erro.
- `01:45` A oferta é o sellCard.
- `01:47` O aviso do produto é o statusCard.
- `01:49` E o card de erro é o fullError.
- `01:52` Ele manda o Tentar novamente daquele item.

### 01:55 · HS12 A HS17

- `01:55` HS12: o olho.
- `01:57` Mostrar e esconder, a máscara e a animação de roleta.
- `02:00` O Carregando.
- `02:02` O saldo buscado na hora pelo módulo da conta.
- `02:04` A memória e a limpeza no fim.
- `02:06` E a tampa da tela antes da miniatura.
- `02:09` HS13: as ações.
- `02:11` Executar o toque.
- `02:12` As telas conhecidas do app e a lista de endereços permitidos.
- `02:16` E o que fazer com o desconhecido.
- `02:18` HS14: o recibo.
- `02:21` Os marcos da abertura e o recibo gravado.
- `02:23` A recuperação depois que o app morre.
- `02:26` O envio, e a conferência: a última abertura foi crash?
- `02:29` E os diagnósticos, sem dado da cliente.
- `02:32` HS15: a Home inteira.
- `02:34` Os blocos na ordem e o esqueleto completo.
- `02:37` O aviso de sem Home.
- `02:39` O indicador do puxar e a tela de erro do puxar.
- `02:42` A rolagem.
- `02:43` E a retirada do card cinza. Lembra? Da HS02.
- `02:47` HS16: a rede de verdade.
- `02:50` O pedido com os cabeçalhos e os códigos de resposta.
- `02:53` O tempo máximo.
- `02:54` E o descarte de um pedido que ficou velho.
- `02:57` Ela testa contra o servidor de mentira até o back ficar pronto.
- `03:00` HS17: o topo e os atalhos.
- `03:03` A prateleira de atalhos.
- `03:05` O topo: o nome dela, os pontos, o botão de emergência e a bolinha de aviso.
- `03:10` E o lugar do insight.
- `03:12` Os dados do topo chegam por portas.
- `03:14` E cada porta tem um dono.
- `03:16` Cinco regras valem pra todas.
- `03:18` Cada história gera o trabalho do iOS e o do Android.
- `03:21` No máximo quatro critérios de aceite, com um teste por regra.
- `03:25` Tudo o que o dev precisa vai escrito na própria história.
- `03:28` Nenhum tipo do back passa do adaptador.
- `03:30` E o mesmo resultado nas duas plataformas.

### 03:34 · AS JUNÇÕES

- `03:34` Agora: como elas começam juntas e se encontram.
- `03:37` Tem três níveis de pronto.
- `03:39` Um: a peça aceita sozinha, com dublês.
- `03:42` Dois: a junção aceita.
- `03:44` As peças de verdade juntas, com os exemplos comuns.
- `03:47` Três: pronto pra produção.
- `03:49` As junções mais as condições de fora.
- `03:51` O design system, o desenho final, a segurança e os testes de ponta a ponta.
- `03:56` São sete junções.
- `03:57` J1: a cópia sem dinheiro, de ponta a ponta.
- `04:01` Junta a HS06, a HS07, a HS08 e a HS16, com o maestro e o card.
- `04:08` J2: o estado da tela e o catálogo.
- `04:11` O montador com todos os desenhistas.
- `04:14` Aqui se prova: tipo novo é peça nova mais uma linha no catálogo.
- `04:18` J3: o olho.
- `04:19` Montador, card, grupo e olho.
- `04:21` J4: o Tentar novamente.
- `04:24` Montador, erro, Home inteira e maestro.
- `04:27` J5: o recibo. Recibo e guarda.
- `04:30` J6: a rede e o contrato.
- `04:32` A rede e o adaptador contra o back de verdade, em homologação.
- `04:36` J7: o item quebrado.
- `04:38` Duas respostas, com os mesmos itens bons.
- `04:41` E telas diferentes.
- `04:43` Numa, o back tirou o item de propósito.
- `04:45` Na outra, o item veio quebrado.
- `04:48` E a ordem por plataforma. **[PROPOSTA]**
- `04:49` De novo, é proposta. **[PROPOSTA]**
- `04:51` No iOS: a HS03 entra como está. **[PROPOSTA]**
- `04:54` Depois a HS05, depois a HS04, depois a HS06 e as de tela. **[PROPOSTA]**
- `05:00` No Android: a HS05 primeiro. **[PROPOSTA]**
- `05:03` Depois a HS03 e a HS04, depois a HS06 e as de tela. **[PROPOSTA]**
- `05:09` Por quê? Fazer a HS04 depois da HS05 evita migrar duas vezes. **[PROPOSTA]**

### 05:15 · O BACK

- `05:15` Agora, o back, rapidinho.
- `05:17` Hoje existem dois BFFs.
- `05:20` O BFF é o servidor que monta a Home pro app.
- `05:23` Um pra pessoa física, o PF. Outro pra pessoa jurídica, o PJ.
- `05:28` O de PF é Java 21 com Spring Boot.
- `05:31` Ele monta a Home de hoje numa rota antiga.
- `05:33` Busca umas 15 fontes em paralelo.
- `05:35` Cartões, contas, seguros e outras.
- `05:38` O de PJ monta quatro blocos.
- `05:40` Atalhos, conta PJ, cartões PJ e campanhas.
- `05:44` O trabalho do back: criar a rota nova.
- `05:47` GET /home/v1/surface, respondendo no contrato 1.6.
- `05:52` Pra isso, ele traduz cada produto no componente genérico.
- `05:56` Reaproveitando as fontes de hoje.
- `05:58` E tem uma regra única de falha.
- `06:00` O back não sabe se a Maria tem o produto? O grupo inteiro vem em falha.
- `06:04` Sabe que ela tem, mas não consegue montar? Vem o card de falha.
- `06:08` Consegue montar? Vem o card com o que tem.
- `06:10` E nada da resposta vai pro log.
- `06:12` Porque tem dado da cliente.
- `06:14` Amanhã, 21/10, é o dia inteiro do back com os devs.
- `06:18` Pra começar a rodar.

### 06:20 · AS DATAS

- `06:20` Pra fechar: as datas e a verdade crua.
- `06:23` O plano usa 7/12 pra tudo estar integrado.
- `06:26` O corte de código do app: 11/12.
- `06:29` O último deploy do back: 17/12.
- `06:32` A versão do app: 21/12.
- `06:34` E o congelamento da loja: de 20/12 a 8/01.
- `06:38` O back em homologação? As fontes divergem.
- `06:41` Uma diz 21/11. A outra, de 02–04/12.
- `06:45` Agora, o tamanho. **[HIPÓTESE]**
- `06:47` É hipótese, até os devs estimarem. **[HIPÓTESE]**
- `06:49` São 14 entregas no iOS e 15 no Android. **[HIPÓTESE]**
- `06:53` Com um dev por plataforma e umas duas entregas por semana… **[HIPÓTESE]**
- `06:56` …dá sete semanas e meia. **[HIPÓTESE]**
- `06:58` Começando em 27/10, termina em meados de dezembro. **[HIPÓTESE]**
- `07:02` Não cabe antes de 7/12. **[HIPÓTESE]**
- `07:04` As saídas na mesa são quatro.
- `07:06` Mais gente nas peças soltas: ações, recibo e rede.
- `07:10` Elas não dependem do resto.
- `07:12` A estimativa real dos devs.
- `07:14` Um escopo mínimo definido agora.
- `07:16` Pra saber o que sai se apertar.
- `07:18` E reservar as semanas das junções.
- `07:21` E o próximo passo, na ordem.
- `07:23` Um: aprovar a HS05 e as cinco regras.
- `07:26` Dois: escrever as 13 histórias.
- `07:29` Cada uma revisada até consenso.
- `07:31` Três: os devs pegam várias em paralelo, com os dublês.
- `07:35` Quatro: o back começa em 21/10.
- `07:37` E a pergunta do começo?
- `07:39` Depois da HS05, se o back mudar um nome de campo…
- `07:43` …muda uma peça só: o adaptador.

