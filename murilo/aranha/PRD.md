# PRD — Aranha do Murilo

- **Versão do documento**: 1.0 (v1.0 implementada em 2026-11-03; falta a sessão K1/K2)
- **Escopo do produto**: v1.0 — três cenários, teia no brinquedo, caixa no chão, sem vilão, placar desde o primeiro dia
- **Fonte de verdade de design**: [`TEMA.md`](./TEMA.md), [`GLOSSARY.md`](./GLOSSARY.md), [`docs/adr/`](./docs/adr/)
- **Jogos irmãos**: [`../PRD.md`](../PRD.md) (Teia), [`../hulk/PRD.md`](../hulk/PRD.md) (Força), [`../sonic/PRD.md`](../sonic/PRD.md) (Veloz), [`../super/PRD.md`](../super/PRD.md) (Raio)
- **Status**: design derivado do Raio do Murilo a pedido do operador em 2026-11-03. **v1.0 implementada em 2026-11-03** (F1 e F2 concluídas; smoke automatizado em jsdom: 231 nós de pico, 32 coletas, 9 linhas de placar, zero erros). F3 pendente: medições oficiais de NF3/NF6/NF12 e sessão observacional (K1/K2) pelo operador.

> `TEMA.md` responde *por que*; este PRD responde *o que sai e como se mede*.

---

## 1. Sumário executivo

### Problema

O Murilo adorou o Raio do Murilo e pediu "o mesmo, mas com o Homem-Aranha". O Homem-Aranha **não voa**; logo o verbo não pode ser o olhar de longe. Precisa de uma mecânica que mantenha o prazer central do irmão (apontar → o brinquedo vem → entra na caixa) sem copiar o voo.

### Solução

Arquivo único onde o mouse é varinha: ele aponta, o Homem-Aranha se agacha, **atira a teia** do punho, o nó se cola no brinquedo, e a teia **puxa o brinquedo** até a caixa que espera no chão. Zero texto (salvo o placar), zero derrota, zero cronômetro, zero vilão, ele nunca sai do chão.

É o par do Raio do Murilo: mesmos três cenários, mesmos nove brinquedos, mesma FSM, mesma caixa, mesmo placar. Muda o herói e o alcance.

### Critérios de sucesso

| # | Tipo | Critério | Como medir |
| --- | --- | --- | --- |
| K1 | Observacional | O Murilo ri ao menos uma vez por sessão | Observação, prancheta de 5 min |
| K2 | Observacional | Pede "de novo" ou volta a clicar sem ser convidado | Observação, prancheta de 5 min |
| K3 | Técnico | Primeiro frame em ≤ 2 s, offline | Cronômetro, 3 execuções |
| K4 | Técnico | Frame time p95 ≤ 16,7 ms durante teia e festa | DevTools › Performance, 60 s |
| K5 | Técnico | Zero rede e zero erros no console em 5 min | DevTools › Network + Console |

Anotação qualitativa (não é KPI): acha a caixa? a teia assusta?

---

## 2. Experiência e funcionalidade

### Personas

**Murilo — jogador primário (3 anos).** Pré-leitor. Já conhece a Teia, o Força, o Veloz e o Raio. Motricidade fina incompleta. Atenção de 2 a 5 minutos.

**Adulto — operador (pai).** Abre o arquivo, regula o volume, tira o filho quando a hora acabou. Sem instalação, sem manual.

### Histórias de usuário

#### US1 — Apontar e causar a teia
*Como Murilo, quero apontar para o brinquedo e ver a teia sair do punho, para sentir que fui eu que causei aquilo.*

- Exatamente **um** brinquedo ativo por vez, sempre visível, mexendo devagar, com halo.
- Clique válido dispara `MIRAR` em ≤ 100 ms.
- Aim assist: círculo de **240 px** ao redor do brinquedo + **terço central** (28–74% × 16–76%).
- Clique fora não produz **nada**.
- Nenhum texto nem tutorial. O peito leva uma aranha preta, sem letra.

#### US2 — A coleta acontece sozinha
*Como Murilo, quero que a teia prenda o brinquedo e o traga até a caixa, sem precisar fazer mais nada.*

- Sequência `MIRAR → TEIA → COLETA → GUARDA` completa em **1,6 s a 2,0 s**.
- Cliques durante a sequência são ignorados.
- Ao fim da guarda, o próximo brinquedo entra após 0,4 s.
- A teia estica do punho até o brinquedo; o brinquedo é puxado em arco até a caixa. Nunca linear. Nunca cai fora.

#### US3 — Ele fica no chão
*Como Murilo, quero que o Homem-Aranha fique de pé, como eu, e alcance o brinquedo com a teia.*
- Não existe voo, não existe balanço. O herói fica de pé, com uma sombra no chão e uma respiração leve (`ADR 0003`).
- Posição fixa por cenário; ele **não** teleporta de lado.
- A caixa fica no chão ao lado dele e não se move.

#### US4 — Progresso visível, sem números (exceto o placar)
- Três estrelas na base; uma acende por brinquedo.
- No terceiro, festa: confete, **a caixa pulsa dourada**, jingle de 8 notas, ele dá pulinhos.
- Festa dura **3,0 s ± 0,2 s**, transição sem menu e sem texto.
- Nenhum número fora do placar (`US8`). A caixa não mostra dígitos.

#### US5 — Entender de onde vem o problema
- Sem vilão (`ADR 0001`). Os brinquedos ficaram espalhados.
- O brinquedo está sempre à vista, mexendo devagar, com halo.

#### US6 — A caixa agradece
- Na `GUARDA`, a tampa fecha e a caixa brilha, ≤ 350 ms.
- Som curto de guarda, não substitui o som do brinquedo.

#### US7 — A teia (o espetáculo)
*Como Murilo, quero ver a teia sair do punho e grudar no brinquedo, para sentir que fui eu que alcancei.*
- Durante `TEIA`: um fio branco cresce do punho até o brinquedo; ao chegar, um **nó de teia** (estrela de fios) se cola nele.
- O fio é um `<path>`; o nó é um grupo pré-criado — só `transform`/`opacity`.
- `prefers-reduced-motion` mantém a teia (é o verbo do jogo) mas tira a respiração extra.
- Nenhum flash branco. Nenhuma sirene. Estalo curto (`L9`).

#### US8 — Ver o que eu já guardei
- Placar no canto superior direito, igual aos irmãos. Uma linha por brinquedo já guardado.
- Cumulativo na sessão. Linha nasce no primeiro salvamento e não se move.
- No celular, 1,5×.
- Máximo **nove linhas**.
- A caixa **não** é um segundo placar e **não** mostra dígitos.

#### US9 — Abrir, calar e sair, sem manual
- Arquivo único, `file://`, sem servidor.
- `M` mudo ≤ 100 ms, ícone no topo-centro. `F` tela cheia no desktop.
- Botão de voltar à vitrine: HTML, canto superior esquerdo, ≥ 56 px, `../../index.html`.
- Primeiro toque no celular entra em tela cheia (`../../PADRAES.md`).
- Nenhum `alert`/`confirm`.

#### US10 — Não quebra, não pede, não trava
- Zero rede. Delta do loop clamp 100 ms.
- Após 8 s ocioso, uma coleta de demonstração. Qualquer clique cancela.

#### US11 — Sempre Homem-Aranha
- Máscara vermelha com teias, olhos grandes brancos, aranha preta no peito, azul nos membros, botas e luvas vermelhas.
- Nenhum Peter Parker, nenhuma troca de roupa (`ADR 0002`).

#### US12 — O peito sem letra
- O peito leva a **aranha preta** (símbolo, não letra) — conforme L3.
- Reconhecível como Homem-Aranha pela máscara + teias + azul, em 200 ms.

### Non-goals

- Toque como mecânica jogável (exceção: tela cheia no primeiro toque).
- Peter Parker, Daily Bugle, tio Ben, Mary Jane.
- Vilão, Duende, Venom, combate.
- Teia-balanço (é o Teia do Murilo), loop, spring, plataforma.
- Caixa como segundo placar / inventário que mostra os itens dentro.
- Quarto cenário, água, espaço.
- Dois jogadores, o jogo do Pedro.
- Narração, analytics, persistência, dependência.

---

## 3. Requisitos de sistema de IA

**Não aplicável.** FSM determinística de 6 estados + festa.

---

## 4. Especificação técnica

### Visão de arquitetura

Arquivo único, sem build. Mesma arquitetura dos irmãos.

```
murilo/aranha/aranha-do-murilo.html
├── <svg id="palco">   céu · cenário · brinquedo · teia · caixa · herói · estrelas · poeira · confete · placar
├── Loop (rAF)         delta clamp 100 ms
├── FSM                ESPERANDO → MIRAR → TEIA → COLETA → GUARDA → (FESTA)
├── Entrada            pointerdown → coords virtuais → resolveAlvo()
├── Cenários (dados)   3 configs
└── Áudio              Web Audio sintetizado
```

SVG no DOM, não Canvas. Só `transform` e `opacity` animam. Botão de voltar é HTML.

### Máquina de estados

| Estado | Duração | Saída |
| --- | --- | --- |
| `ESPERANDO` | indefinida | clique válido ou 8 s |
| `MIRAR` | 260 ms | automática |
| `TEIA` | 380 ms | automática |
| `COLETA` | 420 ms | automática |
| `GUARDA` | 350 ms | automática |
| `FESTA` | 3000 ms | troca de cenário |

Cliques só em `ESPERANDO`.

### Sistema de coordenadas

Mundo virtual **1920×1080**, letterbox. O herói fica de pé no chão (y ≈ 892–902 conforme o cenário). Aim assist 240 px + terço central.

### Dados dos cenários

| id | Nome | Céu | Chão (y) | Herói (x) | Brinquedos (por rodada) |
| --- | --- | --- | --- | --- | --- |
| `metropolis` | Metropolis | `#5FB4E4 → #E4F5FB` | 900 (rua) | 560 | Bola → Ursinho → Balão |
| `campo` | Campo | `#7FC7E8 → #EAF8E4` | 898 (morro) | 600 | Pipa → Carrinho → Patinho |
| `pordosol` | Pôr do sol | `#E0685F → #FFE4B5` | 892 (morro) | 600 | Gatinho → Bola de praia → Sorvete |

Ordem: `metropolis` → `campo` → `pordosol` → festa → `metropolis`…

Cada cenário tem **três pontos** onde o brinquedo pode aparecer. O da rodada é `amigos[jogo.guardados % amigos.length]`, e o ponto correspondente também sai do contador — os três do cenário aparecem em sequência, sem repetir.

A arte de cada brinquedo e de cada cenário vive uma única vez no documento (`<defs id="biblioteca">` para os brinquedos, `<template>` para os cenários) e é instanciada por `<use>` / clone.

### Caixa

Grupo **irmão** do herói (não acompanha o `transform`), parado no chão ao lado dele. Tampa é nó separado (`g.tampa`) que gira na `COLETA` (abre) e volta na `GUARDA` (fecha). Não mostra dígitos.

### Teia

Um `<path>` branco do punho direito (ponto local `(90, -42)` transformado com o herói) até o brinquedo. Na `TEIA` a ponta viaja do punho até o alvo (cresce) e um nó de teia se cola nele; na `COLETA` a ponta segue o brinquedo puxado e some ao fim. Um halo branco mais grosso atrás do fio dá volume.

### Placar

Mesmo modelo da casa. `registrarGuarda()` em `entrar(S.GUARDA)`. NF4 = **240**.

### Requisitos não-funcionais

| # | Requisito | Limite |
| --- | --- | --- |
| NF1 | Arquivo único, `file://` | 1 `.html` |
| NF2 | Primeiro frame | ≤ 2 s |
| NF3 | Frame time p95 | ≤ 16,7 ms |
| NF4 | Nós SVG | ≤ **240**; ≤ 80 animados/frame |
| NF5 | DPR | ≤ 2 |
| NF6 | Áudio | master −12 dBFS; pico ≤ −6 dBFS |
| NF7 | Delta | clamp 100 ms |
| NF8 | Rede | zero |
| NF9 | Persistência | zero |
| NF10 | Terceiros | zero |
| NF11 | Determinismo | sorteio só em partículas |
| NF12 | Navegadores | Chrome, Edge, Firefox |

Pico medido: **231 nós** com as 9 linhas de placar e a festa cheia (defs 65, herói 29, placar 28, confete 14, cenário 14, estrelas 6, poeira 5, brinquedo 1). Teto 240.

### Segurança e privacidade

Zero rede, zero dado, zero supply chain. LGPD: não há dado a tratar.

---

## 5. Riscos e roadmap

| # | Risco | Prob. | Impacto | Mitigação | Plano B |
| --- | --- | --- | --- | --- | --- |
| R1 | Teia assusta | Baixa | Alto | Fio fino, estalo curto, 0,38 s; K1/K2 | Encurtar / baixar ganho |
| R2 | Confundir com o Teia do Murilo | Média | Médio | Vitrine com figuras distintas (um balança, outro de pé) e subtítulos diferentes | Renomear |
| R3 | Caixa cobre o brinquedo | Baixa | Médio | Caixa no chão à esquerda, brinquedo sempre à direita | Afastar mais |
| R4 | Autoplay bloqueia áudio | Média | Baixo | Destravar no pointerdown | Ícone piscando |
| R5 | Notebook antigo < 60 fps | Baixa | Médio | Só transform/opacity; DPR ≤ 2 | Menos confete |
| R6 | Escopo cresce (Peter, vilão, teia-balanço) | Média | Médio | Non-goals; ADRs 0001/0002/0003 | Disciplina |
| R7 | Nós estouram 240 | Média | Médio | Placar estático; teia é 1 fio | Cortar deco |
| R8 | Brinquedo some atrás do herói | Baixa | Alto | Brinquedo sempre à direita, herói no chão à esquerda | Ajustar pontos |
| R9 | Herói parece flutuar sobre o morro | Média | Baixo | `chao` por cenário + sombra que encolhe no pulo | Reancorar o `y` |
| R10 | Confusão com o Raio (mesmos cenários/brinquedos) | Média | Baixo | É intencional; o herói e o verbo distinguem | Nada |

### Rollout

| Fase | Entrega | Critério |
| --- | --- | --- |
| **F1 — Teia** | FSM, 1 cenário, arte provisória, sem áudio, sem placar | Clicar produz teia e coleta; herói no chão |
| **F2 — Voz e HUD** | 3 cenários, áudio, placar, caixa com tampa, voltar, attract, `M`/`F` | Zero erros; NF4 ≤ 240 |
| **F3 — v1.0** | Regressão NF1–NF12, sessão com o Murilo | **Pendente** — técnicos oficiais (NF3/NF6/NF12) e K1/K2 pelo operador |

### Definição de pronto

Arquivo único abre offline, Murilo aponta e uma coleta acontece em menos de 2,0 s, a teia não assusta, a caixa acompanha, placar mostra os brinquedos, NF1–NF12 verificados, nenhuma lei violada.

**Situação:** F1 e F2 implementadas. Smoke automatizado em jsdom percorre ~90 s de jogo: 231 nós de pico, 32 coletas, 9 linhas de placar, teia esticando e colando, tampa e festa — zero erros de console. Pendente F3 (K1/K2 e medições oficiais).

---

## 6. Rastreabilidade

| Requisito | Origem |
| --- | --- |
| Mesma missão, cenários e brinquedos do Raio | pedido do operador (2026-11-03) |
| Herói no chão; teia até o brinquedo | `docs/adr/0003-heroi-fica-no-chao.md` |
| Sem vilão | `docs/adr/0001-sem-vilao-brinquedos-pelo-vento.md` |
| Sempre Homem-Aranha, sem Peter Parker | `docs/adr/0002-sempre-homem-aranha.md` |
| Placar desde v1.0 | `docs/adr/0004-placar-desde-a-v1.md` + `../../PADRAES.md` |
| Tela cheia primeiro toque | `docs/adr/0005-tela-cheia-no-primeiro-toque.md` + `../../PADRAES.md` |
| Teia do punho; caixa no chão | `docs/adr/0006-teia-no-punho-caixa-no-chao.md` |
| Vocabulário | `GLOSSARY.md` |
| Peito sem letra | `TEMA.md` L3 |
