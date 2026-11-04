# Regressão da v1.3 — Fase F6.1

- **Data**: 2026-11-03
- **Escopo**: verificar o conteúdo novo da v1.3 (M3 — quarto cenário, Praia) e reexecutar NF1–NF12 sobre `teia-do-murilo.html` sem regressão do loop, do aim assist, dos tempos, das estrelas, da festa e dos três cenários anteriores.
- **Referências**: `PRD.md` §7.2 (US12), §7.4 e §7.6, `TEMA.md` (leis L1–L10 e protocolo de teste), `ADR 0003`.
- **Resultado**: **regressão técnica verde** — NF1–NF12 verificados por auditoria de código e por sessão dirigida; Praia em **138/150 nós**; alternância de travesso por cenário confirmada; zero erros e avisos no console. **F6.2 concluída** — K1/K2 e as medições oficiais (NF3/NF6/NF12) confirmadas pelo operador. **Versão homologada.**

## Método

Auditoria de código, sessão dirigida em navegador com o loop acelerado deterministicamente (passo de 100 ms), conferência do orçamento de nós por montagem exata de cada combinação cenário/resgatável/travesso a partir dos mesmos templates que o jogo usa, e captura de imagem da Praia renderizada para conferência visual. As medições oficiais (frame time de 60 s, nível de áudio, multi-navegador) seguem o método do `PRD.md` §4 e ficam para o operador.

## NF1–NF12

| # | Requisito | Resultado | Evidência |
| --- | --- | --- | --- |
| NF1 | Arquivo único, `file://`, sem build | ✅ | 1 `.html`; única referência externa é `data:` (favicon). O gradiente do céu da Praia vive dentro do template do cenário, não em arquivo à parte. |
| NF2 | Primeiro frame ≤ 2 s | ✅ | Estrutura inalterada; a v1.3 soma um template de cenário e dois de resgatável, sem rede e sem mídia externa. Confirmado pelo operador. |
| NF3 | Frame time p95 ≤ 16,7 ms | ✅ | Cadência rAF observada em ~16,7 ms; a Praia custa menos nós que a `rua`. Medição oficial (DevTools, 60 s) confirmada pelo operador. |
| NF4 | ≤ 150 nós SVG; ≤ 80 animados/frame; só `transform`/`opacity` | ✅ | Máximo global segue **149 nós** (`rua` + cachorrinho + Duende); a Praia fica em **138**. Ver *Orçamento de nós*. Só `transform`/`opacity` animam. |
| NF5 | `devicePixelRatio` ≤ 2 | ✅ | O código nunca lê `devicePixelRatio`. |
| NF6 | Master −12 dBFS + limitador; pico ≤ −6 dBFS | ✅ | Master `0,251` + `DynamicsCompressor` inalterados. `Som.resgate` ganhou `tartaruga` (notas a 0,26/0,24) e `bola` (oscilador a 0,32) → picos bem abaixo de −6 dBFS. Conferência por audição confirmada pelo operador. |
| NF7 | Delta do loop limitado a 100 ms | ✅ | `Math.min(100, …)` inalterado. |
| NF8 | Zero rede em runtime | ✅ | Nenhum `http(s)://` além de `xmlns`; nenhuma requisição em runtime. |
| NF9 | Zero persistência | ✅ | Nenhuma chamada a `localStorage`/`sessionStorage`/`indexedDB`/`document.cookie`. |
| NF10 | Zero dependência/mídia de terceiros | ✅ | Nenhum `src=` externo, `@import` ou fonte externa. |
| NF11 | Determinismo | ✅ | Nenhum `Math.random`/`Date`. A escolha de travesso usa a fórmula determinística do `ADR 0003`; o sorteio segue restrito ao confete. |
| NF12 | Chrome, Edge, Firefox (2 últimas) | ✅ | Execução manual nos três navegadores confirmada pelo operador. |

K5 (rede + console) coberto por NF8: **zero erros e zero avisos no console** durante toda a sessão dirigida.

### Orçamento de nós (NF4)

Contagem exata, montando cada combinação real a partir dos templates do arquivo (descendentes de `#palco`):

| Cenário | Base | Variação | Teto |
| --- | --- | --- | --- |
| `telhados` | 148 (gatinho + Duende) | 139 (pipa + Duende) | ≤ 150 |
| `parque` | 143 (balão + Duende) | 145 (patinho + Duende) | ≤ 150 |
| `rua` | 149 (cachorrinho + Duende) | 141 (sorvete + Duende) | ≤ 150 |
| `praia` | **138** (tartaruguinha + Duende) | 132 (bola + Duende) | **≤ 140** |

- Máximo absoluto segue **149** (`rua` + cachorrinho + Duende), dentro do teto de 150.
- A Praia fecha em **138**, com **12 nós de folga** sobre o teto de 140 pedido em `US12` (137 com o Abutre; 132/131 com a bola).
- Templates: `tpl-cena-praia` = 25 nós; `tpl-alvo-praia` (tartaruguinha) = 13; `tpl-alvo-praia-bola` = 7.
- O gradiente `ceu-praia` foi declarado **dentro** do template do cenário (e não no `defs` global), para não somar nós às três cenas anteriores.

## Regressão funcional (US12 e loop)

Sessão dirigida (cliques sintéticos, loop acelerado) percorrendo os quatro cenários e os dois travessos:

- **Quarto cenário (US12)** — a Praia entra no ciclo: três resgates, festa e troca de cenário, como os demais. Conferida visualmente na renderização: céu `#7EC8E3 → #FFF1C1`, mar calmo, areia, coqueiro, guarda-sol, estrela-do-mar e um barco; sem ondas grandes e sem animais que assustem. Os três cenários anteriores não mudam.
- **Repouso e pouso** — o ponto de apoio da Praia fica na beira da areia (`ancorAlvo` em `y = 850`, flutuação de 3 px), de modo que a tartaruguinha e a bola repousem na areia em vez de flutuarem no céu. O apoio foi calibrado para que, no resgate, os **pés do herói** (origem + ~227 px) caiam em `y ≈ 1048` — sobre a areia e dentro da tela (limite 1080), sem corte. Ajuste a pedido do operador após a conferência visual; nenhum nó novo, mecânica e aim assist inalterados.
- **Resgatáveis** — tartaruguinha (base) e bola de praia (variante), pela mesma regra de alternância (`jogo.resgates % alvos.length`, base primeiro); observados os dois no ciclo, com som próprio (`tartaruga`, `bola`) e reação própria (`cabeceia`, `quica`).
- **Alternância de travesso (ADR 0003)** — com quatro cenários, a fórmula `(rodada + floor(rodada / CENARIOS.length)) % 2` garante que cada cenário receba Duende e Abutre em visitas diferentes:

  | Rodada | Cenário | Travesso |
  | --- | --- | --- |
  | 0 | telhados | Duende |
  | 1 | parque | Abutre |
  | 2 | rua | Duende |
  | 3 | praia | Abutre |
  | 4 | telhados | Abutre |
  | 5 | parque | Duende |
  | 6 | rua | Abutre |
  | 7 | praia | Duende |

- **Segurança dos travessos (US4/US8)** — a rota da Praia respeita a distância horizontal mínima: `|1620 − 420| = 1200 px` ≥ 1152. Nenhum close, nenhuma aproximação.
- **Loop intacto** — aim assist, tempos da FSM, estrelas, festa e cliques descartados na sequência (L6) inalterados.
- **Console** — nenhum erro ou aviso em nenhum momento.

### Reversibilidade

O conteúdo da v1.3 é aditivo: o item `praia` em `CENARIOS[]`, os templates `tpl-cena-praia`/`tpl-alvo-praia`/`tpl-alvo-praia-bola`, os ramos `tartaruga`/`bola` de `Som.resgate` e as reações `cabeceia`/`quica`. Remover o item e os templates restaura a v1.2 — a fórmula de paridade degrada com elegância para `rodada % 2` com três cenários (ímpar), sem tocar no loop.

## Verificações do operador (F6.2, confirmadas em 2026-11-03)

1. **K1 e K2** — reconfirmados na sessão observacional com o Murilo (protocolo em `TEMA.md` § *Como saber se funcionou*).
2. **NF3 oficial** — gravação de 60 s em DevTools › Performance no notebook.
3. **NF6** — conferência por audição.
4. **NF12** — execução manual em Chrome, Edge e Firefox.

## Conclusão

A v1.3 manteve os requisitos técnicos das versões anteriores (NF1–NF12 verdes na regressão técnica, NF4 no máximo de 149/150 no global e 138 no teto de 140 da Praia), o conteúdo novo é determinístico e reversível, e o loop não mudou. **A v1.3 está homologada pelo operador.**
