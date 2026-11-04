# Regressão da v1.2 — Fases F5.1, F5.2 e F5.3

- **Data**: 2026-11-03
- **Escopo**: reexecutar NF1–NF12 sobre `teia-do-murilo.html` e verificar o conteúdo novo da v1.2 (M1 — gratidão do resgatado; M2 — travesso trapalhão) sem regressão do loop, do aim assist, dos tempos, das estrelas e da festa.
- **Referências**: `PRD.md` §7.2 (US10/US11), §7.4 e §7.6, `TEMA.md` (leis L1–L10 e protocolo de teste).
- **Resultado**: **regressão técnica verde** — NF1–NF12 verificados por auditoria de código e por sessão dirigida; orçamento de nós em **149/150**; zero erros e avisos no console. **F5.3 concluída** — K1/K2 e as medições oficiais (NF3/NF6/NF12) confirmadas pelo operador. **Versão homologada.**

## Método

Auditoria de código (busca por padrões proibidos e por animação fora de `transform`/`opacity`), sessão dirigida em navegador com o loop acelerado deterministicamente (via substituição de `requestAnimationFrame`, passo de 100 ms) cobrindo as três cenas e os dois travessos, e conferência do orçamento de nós por montagem exata de cada combinação cenário/resgatável/travesso a partir dos mesmos templates que o jogo usa. As medições oficiais (frame time de 60 s, nível de áudio, multi-navegador) seguem o método do `PRD.md` §4 e ficam para o operador.

## NF1–NF12

| # | Requisito | Resultado | Evidência |
| --- | --- | --- | --- |
| NF1 | Arquivo único, `file://`, sem build | ✅ | 1 `.html`; única referência externa é `data:` (favicon). Duplo-clique em `file://`. |
| NF2 | Primeiro frame ≤ 2 s | ✅ | Estrutura inalterada; a v1.2 soma um template de chapéu e poucos ramos de JS, sem rede e sem mídia externa. Confirmado pelo operador (v1.1 = 124 ms). |
| NF3 | Frame time p95 ≤ 16,7 ms | ✅ | Cadência rAF observada em 16,7 ms na sessão dirigida; o custo por quadro não aumenta (nenhum nó novo animado por quadro além dos já existentes). Medição oficial (DevTools, 60 s) confirmada pelo operador. |
| NF4 | ≤ 150 nós SVG; ≤ 80 animados/frame; só `transform`/`opacity` | ✅ | Máximo **149 nós** (`rua` + cachorrinho + Duende com chapéu). Só `transform`/`opacity` animam (auditoria); `setAttribute('d')` só na montagem da cena. Ver *Orçamento de nós*. |
| NF5 | `devicePixelRatio` ≤ 2 | ✅ | O código nunca lê `devicePixelRatio` (auditoria por busca). |
| NF6 | Master −12 dBFS + limitador; pico ≤ −6 dBFS | ✅ | Master `0,251` (−12 dBFS) + `DynamicsCompressor` (threshold −6, ratio 20) inalterados. `Som.gratidao` usa `nota` a 0,34 e `Som.trapalhada` a 0,34 (≈ −21 dBFS antes do limitador) → picos bem abaixo de −6 dBFS. Conferência por audição confirmada pelo operador. |
| NF7 | Delta do loop limitado a 100 ms | ✅ | `Math.min(100, …)` no `quadro()` inalterado. |
| NF8 | Zero rede em runtime | ✅ | Nenhum `http(s)://` além de `xmlns`; nenhuma requisição em runtime. |
| NF9 | Zero persistência | ✅ | Nenhuma chamada a `localStorage`/`sessionStorage`/`indexedDB`/`document.cookie`. |
| NF10 | Zero dependência/mídia de terceiros | ✅ | Nenhum `src=` externo, `@import` ou fonte externa; apenas `xmlns` e `url(#…)` internos. |
| NF11 | Determinismo | ✅ | Nenhum `Math.random`/`Date`. Reação escolhida pelo índice do resgatável (`alvos[v].reacao`) e trapalhada pelo dado do travesso (`TRAVESSOS[i].gag`); o único sorteio (`mulberry32` semeado) segue restrito ao confete. |
| NF12 | Chrome, Edge, Firefox (2 últimas) | ✅ | Execução manual nos três navegadores confirmada pelo operador. |

K5 (rede + console) coberto por NF8: **zero erros e zero avisos no console** durante toda a sessão dirigida.

### Orçamento de nós (NF4)

Contagem exata, montando cada combinação real a partir dos templates do arquivo (descendentes de `#palco`):

| Cenário | Base (v1.2) | Variação (v1.2) | Teto |
| --- | --- | --- | --- |
| `telhados` | **148** (gatinho + Duende) | 139 (pipa + Duende) | ≤ 150 |
| `parque` | 143 (balão + Duende) | **145** (patinho + Duende) | ≤ 150 |
| `rua` | **149** (cachorrinho + Duende) | 141 (sorvete + Duende) | ≤ 150 |

- Máximo absoluto = **149** (`rua` + cachorrinho + Duende), dentro do teto de 150 e do critério "≤ 149" de F5.2. O Duende custa 14 nós e o Abutre 13, então com o Abutre o total cai 1.
- O chapéu do Duende como nó separado soma **+1** em cada cenário, exatamente como previsto em §7.4 (147/142/148 → 148/143/149).
- Nós animados por quadro no pior caso (festa): ~32, muito abaixo de 80.

**Correção de um resíduo da v1.1.** A regressão da v1.1 media o cenário assentado, mas entre o fim da `VOLTA` e o próximo `nascerAlvo` (400 ms) o resgatável anterior permanecia no DOM — invisível (opacidade 0) — e o cenário mais largo somava além do teto (na v1.1 chegava a 153; com o chapéu, a 154). A v1.2 introduz `limparAlvo()`, chamada em `concluir()`, em `cancelarDemo()` e na troca de cenário, que remove o nó assim que o resgatável deixa de estar ativo. Com isso, **nenhum quadro** excede 149.

## Regressão funcional (US10, US11 e loop)

Sessão dirigida (cliques sintéticos, loop acelerado), cobrindo `telhados`, `parque`, `rua` e os travessos Duende e Abutre:

- **Gratidão (US10 / M1)** — a reação dispara na entrada de `VOLTA` e cabe na etapa. Além do gesto próprio de cada resgatável, o objeto ganha **destaque** para o gesto não se perder no balanço de volta: sobe ~90 px à frente do herói e cresce de `0,86` para `1,4` (escala externa), enquanto o gesto roda com retenção no pico (`envoltoria`: entra em 22%, segura até 72%, sai) — observados a espreguiça do gatinho (corpo a `scale(1 1,22)` dentro da escala 1,4 externa, segurado ~300 ms — reduzido a pedido do operador, que achou o alongamento anterior desproporcional) e o giro de 360° da pipa (`rotate(0 → 360)`). Duração total ≤ 600 ms, sem alongar a `VOLTA`. Nenhum nó novo (a reação usa o grupo de arte já existente e o `transform` do herói); o som curto de gratidão toca sem substituir o som do resgate.
- **Trapalhão (US11 / M2)** — o Duende perde o chapéu, que cai girando: `transform` do `.gag-chapeu` de `translate(0 0) rotate(0)` a `translate(29,4 114,5) rotate(313,7)`, com fade; o Abutre dá a cambalhota (rotação do travesso no plano, observada chegando a ~360°). O gesto acontece a meio da fuga, no ar, sem queda, impacto ou dor, e a distância horizontal ao herói não diminui.
- **Loop intacto (US9)** — aim assist (círculo de 220 px + terço central), tempos da FSM (teia 250 ms, resgate 500 ms, volta 600 ms, festa 3000 ms), estrelas e troca de cenário inalterados; cliques durante a sequência continuam descartados (L6).
- **Console** — nenhum erro ou aviso em nenhum momento.

### Reversibilidade

O conteúdo da v1.2 é aditivo: os campos `reacao`/`tomGrato` em `alvos[]`, o campo `gag` em `TRAVESSOS[]`, os templates do chapéu, e os sons `Som.gratidao`/`Som.trapalhada`. Remover `reacao`, `gag` e o nó do chapéu restaura a v1.1 sem tocar no loop — `montarAlvo`/`montarTravesso` e a FSM são independentes do conteúdo.

## Verificações do operador (F5.3, confirmadas em 2026-11-03)

1. **K1 e K2** — reconfirmados na sessão observacional com o Murilo (protocolo em `TEMA.md` § *Como saber se funcionou*).
2. **NF3 oficial** — gravação de 60 s em DevTools › Performance no notebook.
3. **NF6** — conferência por audição.
4. **NF12** — execução manual em Chrome, Edge e Firefox.

## Conclusão

A v1.2 manteve os requisitos técnicos das versões anteriores (NF1–NF12 verdes na regressão técnica, NF4 em 149/150 nós em todo quadro), o conteúdo novo é determinístico e reversível, e o loop não mudou. **A v1.2 está homologada pelo operador.**
