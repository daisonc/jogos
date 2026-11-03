# Regressão da v1.1 — Fase F4.3

- **Data**: 2026-11-03
- **Escopo**: reexecutar NF1–NF12 sobre `teia-do-murilo.html` e reconfirmar o comportamento da v1.0 face ao conteúdo novo da v1.1 (variações de resgatável + Abutre).
- **Referências**: `PRD.md` §6.4 e §6.6, `TEMA.md` (leis L1–L10 e protocolo de teste).
- **Resultado**: **regressão técnica verde** (NF1–NF11 verificados; NF12 e as medições oficiais de NF3/NF6 dependem de execução manual no notebook — ver *Pendências*). **K1/K2 pendentes de sessão observacional** com o Murilo.

## Método

Auditoria de código (busca por padrões proibidos), medição de contagem de nós SVG em execução, navegação de rede/console e observação do comportamento dirigido por `pointerdown` sintético, em ambiente de preview do próprio arquivo. As medições oficiais de frame time, nível de áudio e multi-navegador permanecem nas mãos do operador, conforme o método declarado em `PRD.md` §4.

## NF1–NF12

| # | Requisito | Resultado | Evidência |
| --- | --- | --- | --- |
| NF1 | Arquivo único, `file://`, sem build | ✅ | 1 `.html`; referência externa apenas `data:` (favicon). Duplo-clique em `file://`. |
| NF2 | Primeiro frame ≤ 2 s | ✅ | `first-contentful-paint` = **124 ms**; `DOMContentLoaded` = 77,6 ms. |
| NF3 | Frame time p95 ≤ 16,7 ms | ✅ (aprox.) | Cadência rAF travada em 16,7 ms (60 Hz) por 150 quadros; p50 16,7 · p95 17,0 · p99 17,5 ms. Medição oficial (DevTools › Performance, 60 s) pendente no notebook. |
| NF4 | ≤ 150 nós SVG; ≤ 80 animados/frame; só `transform`/`opacity` | ✅ | Máximo **148 nós** (`rua` + cachorrinho). Ver *Orçamento de nós*. Só `transform`/`opacity` animam (auditoria); `setAttribute('d')` ocorre só na montagem do cenário, nunca por quadro. |
| NF5 | `devicePixelRatio` ≤ 2 | ✅ | O código nunca lê `devicePixelRatio`; nenhum escalonamento por DPR (medido: DPR = 1). |
| NF6 | Master −12 dBFS + limitador; pico ≤ −6 dBFS | ✅ (por construção) | Master gain `0,251` (= −12,0 dBFS) seguido de `DynamicsCompressor` (threshold −6, ratio 20). Picos dos efeitos ≈ −16 dBFS antes do limitador. Conferência por audição pendente. |
| NF7 | Delta do loop limitado a 100 ms | ✅ | `Math.min(100, …)` no `quadro()`. |
| NF8 | Zero rede em runtime | ✅ | Exatamente **1 requisição** (o próprio documento); nenhuma em runtime. |
| NF9 | Zero persistência | ✅ | Nenhuma chamada a `localStorage`/`sessionStorage`/`indexedDB`/`document.cookie`. |
| NF10 | Zero dependência/mídia de terceiros | ✅ | Nenhum `http(s)://`, `src=` externo, `@import` ou fonte externa; apenas `xmlns` e `url(#…)` internos. |
| NF11 | Determinismo | ✅ | Nenhum `Math.random`/`Date`; o único sorteio é `mulberry32` semeado, usado **só** em partículas de confete. Alternância de alvo e travesso por contadores. |
| NF12 | Chrome, Edge, Firefox (2 últimas) | ⚠️ pendente | Execução manual no notebook não realizada nesta fase. |

K5 (rede + console) coberto por NF8: **zero erros e zero avisos no console** durante toda a sessão dirigida.

### Orçamento de nós (NF4)

Medido em execução (descendentes de `#palco`):

| Cenário | Base | Com a variação | Teto |
| --- | --- | --- | --- |
| `telhados` | **147** (gatinho) | **138** (pipa) | ≤ 150 |
| `parque` | **142** (balão) | **144** (patinho) | ≤ 150 |
| `rua` | **148** (cachorrinho) | **140** (sorvete) | ≤ 150 |

Máximo absoluto = **148** (o `rua` com a base), dentro do teto de 150. O Abutre custa o mesmo que o Duende (12 elementos de arte → 13 nós com o grupo), então a troca de travesso não altera o total. Nós animados por quadro no pior caso (festa): ~32, muito abaixo de 80.

## Regressão funcional (US7, US8, US9)

Sessão dirigida por cliques sintéticos (sequência completa de resgate + festa + troca de cenário):

- **Alternância de resgatável (US7)** — dentro de um cenário alterna **base → variação → base**: observado `telhados` gatinho → pipa → gatinho e, na rodada seguinte, `parque` balão → patinho. Determinístico (`jogo.resgates % alvos.length`), sem sorteio.
- **Alternância de travesso (US8)** — por rodada: `duende` na rodada 0 e `abutre` na rodada 1, com a troca ocorrendo na passagem de cenário ao fim da festa.
- **Festa e progresso (US3)** — ao terceiro resgate a festa dispara, as três estrelas apagam e o cenário avança (`telhados` → `parque`), reiniciando pela base.
- **Aim assist / L6** — um clique disparado no meio de uma sequência em andamento foi **descartado**, sem fila e sem efeito.
- **Attract mode (US6)** — após 8 s ociosos o jogo executou um resgate de demonstração, que cancelou no primeiro clique subsequente.
- **Console** — nenhum erro ou aviso em nenhum momento.

### Reversibilidade

O conteúdo da v1.1 é aditivo: templates de variação, template do Abutre e os contadores (`jogo.resgates`, `jogo.rodada`, `jogo.somAlvo`, `jogo.somTravesso`). Remover os templates e os contadores restaura o comportamento da v1.0 sem tocar no loop — confirmado por inspeção: `montarAlvo`/`montarTravesso` e a FSM são independentes do conteúdo.

## Pendências (operador)

1. **K1 e K2** — sessão observacional com o Murilo (protocolo em `TEMA.md` § *Como saber se funcionou*). É o único item que exige a criança.
2. **NF3 oficial** — gravação de 60 s em DevTools › Performance no notebook.
3. **NF6** — conferência por audição.
4. **NF12** — execução manual em Chrome, Edge e Firefox.

## Conclusão

A v1.1 manteve os requisitos técnicos da v1.0 (NF1–NF11 verdes, NF4 no teto de 148/150 nós) e o conteúdo novo alterna de forma determinística e reversível. Faltam apenas as verificações que dependem do operador (itens acima) e a reconfirmação observacional de K1/K2 para homologar a versão.
