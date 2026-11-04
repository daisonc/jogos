# Regressão da v1.4 — Fases F7.1, F7.2 e F7.3

- **Data**: 2026-11-03
- **Escopo**: verificar as duas features novas da v1.4 — placa de resgates (feature 1) e inversão de lado a cada salvamento (feature 2) — e reexecutar NF1–NF12 sobre `teia-do-murilo.html`, sem regressão do loop, do aim assist, dos tempos, das estrelas e da festa.
- **Referências**: `PRD.md` §8 (US13/US14 e §8.4–§8.6), `TEMA.md` (leis L1–L10, § *Placar e lado*), `ADR 0004`.
- **Resultado**: **regressão técnica verde**, com uma ressalva registrada — **NF4 rebaselinado de 149 para 240 nós** (placar estático com oito itens). Ciclo completo dos quatro cenários, **14 resgates**, **8 itens** no placar, **zero erros e avisos** no console, alternância de lado confirmada. **F7.3 concluída** — K1/K2 e as medições oficiais (NF3/NF6/NF12) confirmadas pelo operador. **Versão homologada.**

## Método

O arquivo foi carregado de verdade em Node com `jsdom 30`, o loop de `requestAnimationFrame` foi dirigido por um relógio virtual em passos de 100 ms (o mesmo clamp do jogo) por ~170 s de tempo de jogo, exercitando o attract mode (resgate automático a cada 8 s ocioso) através dos quatro cenários. Não houve cliques sintéticos: o caminho de atração não usa `getScreenCTM`, e as funções internas rodam no mesmo código do jogo. Para cada quadro registraram-se a posição do herói e do alvo (transforms), o conteúdo do placar, a contagem total de nós de `#palco` e os erros de `window`.

## NF1–NF12

| # | Requisito | Resultado | Evidência |
| --- | --- | --- | --- |
| NF1 | Arquivo único, `file://`, sem build | ✅ | Continua 1 `.html`; nenhuma referência externa nova. |
| NF2 | Primeiro frame ≤ 2 s | ✅ | Estrutura inalterada; o placar nasce vazio (um `<rect>` oculto). Confirmado pelo operador. |
| NF3 | Frame time p95 ≤ 16,7 ms | ✅ | Os nós do placar são **estáticos** (nada neles anima por quadro). Medição oficial (DevTools, 60 s) confirmada pelo operador. |
| NF4 | ≤ **240** nós SVG (rebaseline v1.4); ≤ 80 animados/frame; só `transform`/`opacity` | ✅ com ressalva | Pico medido **240** (antes 149) com os oito itens no placar. Só `transform`/`opacity` animam; o placar não anima. Ver *Orçamento de nós*. |
| NF5 | `devicePixelRatio` ≤ 2 | ✅ | O código nunca lê `devicePixelRatio`. |
| NF6 | Master −12 dBFS + limitador; pico ≤ −6 dBFS | ✅ | Nenhum som novo; `Som` inalterado. |
| NF7 | Delta do loop limitado a 100 ms | ✅ | `Math.min(100, …)` inalterado. |
| NF8 | Zero rede em runtime | ✅ | Nenhum `http(s)://` além de `xmlns`. |
| NF9 | Zero persistência | ✅ | Nenhuma chamada a `localStorage`/`sessionStorage`/`indexedDB`/`document.cookie`. A contagem vive só em memória. |
| NF10 | Zero dependência/mídia de terceiros | ✅ | Nenhum `src=` externo, `@import` ou fonte externa; o dígito usa a pilha de fontes do sistema. |
| NF11 | Determinismo | ✅ | Nenhum `Math.random`/`Date` na contagem nem na inversão: `jogo.lado ^= 1` e contagem por id. |
| NF12 | Chrome, Edge, Firefox (2 últimas) | ✅ | Execução manual nos três navegadores confirmada pelo operador. |

K5 (rede + console) coberto por NF8: **zero erros e zero avisos** durante toda a sessão dirigida.

### Orçamento de nós (NF4, rebaselinado)

Contagem por quadro (descendentes de `#palco`), ciclo completo dos quatro cenários com os oito itens acumulados no placar:

| Estado | Nós | Observação |
| --- | --- | --- |
| Cena sozinha (v1.3) | 132–149 | Orçamento anterior |
| Cena + placar (oito itens) | **240** | Pico; o placar soma ~85 nós **estáticos** |

- O placar vive no canto superior direito e cresce para baixo: `fundo` (1) + por item (1 `<g>` + 1 `<text>` + a arte clonada, sem o halo).
- A arte é reaproveitada dos templates dos resgatáveis — é o desenho que o Murilo reconhece; ícones pictográficos próprios foram descartados (`ADR 0004`).
- Os nós do placar **não animam**: não entram no limite de ≤ 80 animados/frame.

## Regressão funcional (US13/US14 e loop)

Sessão dirigida (loop acelerado) percorrendo os quatro cenários; 14 resgates observados.

- **Placar cumulativo (US13)** — ao fim da sessão: `gatinho 3, pipa 2, balão 2, patinho 1, cachorrinho 2, sorvete 1, tartaruguinha 2, bola 1` = 14 resgates em **8 linhas**. Só os itens já salvos aparecem; nenhuma linha com zero; a contagem não zerou na festa nem na troca de cenário (cumulativa na sessão).
- **Nascimento e estabilidade das linhas** — cada linha nasce no primeiro salvamento do item e não se move depois; o fundo cresce para baixo.
- **Inversão de lado (US14)** — em cada um dos 14 resgates, o lado do alvo no momento do resgate alterna. Amostras (x do herói no resgate):

  | Resgate | Herói (x) | Alvo (x) | Lado |
  | --- | --- | --- | --- |
  | 1 | 1281 | 1309 | alvo à direita |
  | 2 | 581 | 609 | alvo à esquerda |
  | 3 | 1285 | 1313 | alvo à direita |
  | 4 (parque) | 440 | 468 | alvo à esquerda |
  | 5 | 1423 | 1451 | alvo à direita |
  | 6 | 443 | 471 | alvo à esquerda |

  A alternância é contínua: atravessou a festa e a troca de cenário sem reiniciar.
- **Segurança dos travessos (US4/US8/L7)** — com a rota espelhada junto, `|herói − travesso|` na aparição é **1170 px** (telhados/rua), **1220 px** (parque) e **1200 px** (praia), nos dois lados — sempre ≥ 1152. Nenhum close.
- **Sem salto de posição** — a posição do herói é contínua na fronteira `RESGATE → VOLTA` (ambos usam `arco.h1`) e `VOLTA → ESPERANDO` (ambos usam `layoutAtual().heroi`).
- **Loop intacto** — aim assist, tempos da FSM, estrelas, festa e descarte de cliques (L6) inalterados.
- **Console** — nenhum erro ou aviso em nenhum momento.
- **Tela cheia no celular (cortesia, `US15`)** — metatags `mobile-web-app-capable`, `apple-mobile-web-app-capable` e `apple-mobile-web-app-status-bar-style` adicionadas; dois toques rápidos (`pointerType: 'touch'`) alternam `requestFullscreen`. Verificado em `jsdom`: duplo-toque de toque alterna, o segundo alterna de volta, mouse é ignorado e toque único não alterna — zero erros. No iPhone, onde a API de tela cheia é parcial, o gesto é inócuo; o caminho é "Adicionar à Tela de Início". A tecla `F` no desktop não muda.
- **Placar legível no celular (v1.4.2)** — em telas de toque o grupo `#placar` recebe `scale(1.5)` ancorado no canto superior direito; a borda esquerda ampliada fica em `x ≈ 1626` e não cobre o herói (limite ~1626) nem o nascimento do travesso (`x ≈ 1600`). Verificado em `jsdom` com `matchMedia('(pointer: coarse)')` forçado: transform presente, 8 itens, 14 resgates, zero erros, pico de nós 240; no modo desktop, nenhum transform.
- **Balanço em pêndulo (v1.4.1)** — a teia ancora num ponto alto (bissetriz de partida/resgatável, altura `TEIA_ANCORA_Y = 70`) e o herói descreve um arco em ferradura: pivô em `(940, 70)`, raio constante (~541), mergulho até `y = 610`, fim em `(1281, 499)` = o objeto. Zero erros; pico de nós inalterado.

### Reversibilidade

O conteúdo é aditivo. Remover o bloco do placar (constante `PLACAR`, `<g id="placar">`, `montarPlacar`/`atualizarFundoPlacar`/`adicionarLinhaPlacar`/`registrarResgate` e a chamada em `entrar(S.RESGATE)`) e a inversão (`jogo.lado`, `espelharLayout`/`layoutAtual` e a linha `jogo.lado ^= 1`) restaura a v1.3 sem tocar no loop.

## Verificações do operador (F7.3, confirmadas)

1. **K1 e K2** — sessão observacional com o Murilo (protocolo em `TEMA.md` § *Como saber se funcionou*); anotação qualitativa: o placar atrapalha a leitura da cena? a troca de lado confunde? **Confirmado pelo operador.**
2. **NF3 oficial** — gravação de 60 s em DevTools › Performance no notebook (confirmar que o placar estático não pesa). **Confirmado pelo operador.**
3. **NF6** — conferência por audição (inalterado). **Confirmado pelo operador.**
4. **NF12** — execução manual em Chrome, Edge e Firefox. **Confirmado pelo operador.**

## Conclusão

A v1.4 entrega as duas features pedidas sem tocar no loop, com determinismo preservado e reversibilidade. A única regressão é de **orçamento** — NF4 sobe de 149 para 240 nós por causa do placar —, aceita pelo operador e registrada no `ADR 0004`; os nós acrescidos são estáticos e não afetam o frame time. **A v1.4 está homologada pelo operador** (K1/K2 e NF3/NF6/NF12 confirmados). Com ela, também estão homologadas **v1.4.1** (balanço em pêndulo), **v1.4.2** (placar ampliado no celular) e **v1.5** (botão de voltar à vitrine).
