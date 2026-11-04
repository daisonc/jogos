# PRD — Raio do Murilo

- **Versão do documento**: 1.1 (v1.0 implementada em 2026-11-03; falta a sessão K1/K2 com o Murilo)
- **Escopo do produto**: v1.0 — três cenários, raio dos olhos, caixa carregada, sem vilão, placar desde o primeiro dia
- **Fonte de verdade de design**: [`TEMA.md`](./TEMA.md), [`GLOSSARY.md`](./GLOSSARY.md), [`docs/adr/`](./docs/adr/)
- **Jogos irmãos**: [`../PRD.md`](../PRD.md) (Teia), [`../hulk/PRD.md`](../hulk/PRD.md) (Força), [`../sonic/PRD.md`](../sonic/PRD.md) (Veloz)
- **Status**: v1.0 implementada e com smoke automatizado verde (230 nós, 9 linhas de placar, 3 cenários, festa). Pendente apenas a homologação K1/K2 com o Murilo.

> `TEMA.md` responde *por que*; este PRD responde *o que sai e como se mede*.

---

## 1. Sumário executivo

### Problema

O Murilo tem 3 anos e já tem a Teia (mover o mundo com a teia), o Força (levantar o impossível) e o Veloz (chegar rápido). Falta o olhar que alcança e guarda. Qualquer jogo de "pegar itens" convencional falha nos três eixos — mira, leitura, derrota.

### Solução

Arquivo único onde o mouse é varinha: ele aponta, o Super Homem vira, dois raios vermelhos saem dos olhos, o brinquedo voa em arco até a caixa que ele carrega. Zero texto (salvo o placar), zero derrota, zero cronômetro, zero vilão, ele nunca pousa.

### Critérios de sucesso

| # | Tipo | Critério | Como medir |
| --- | --- | --- | --- |
| K1 | Observacional | O Murilo ri ao menos uma vez por sessão | Observação, prancheta de 5 min |
| K2 | Observacional | Pede "de novo" ou volta a clicar sem ser convidado | Observação, prancheta de 5 min |
| K3 | Técnico | Primeiro frame em ≤ 2 s, offline | Cronômetro, 3 execuções |
| K4 | Técnico | Frame time p95 ≤ 16,7 ms durante raio e festa | DevTools › Performance, 60 s |
| K5 | Técnico | Zero rede e zero erros no console em 5 min | DevTools › Network + Console |

Anotação qualitativa (não é KPI): acha a caixa? o raio assusta?

---

## 2. Experiência e funcionalidade

### Personas

**Murilo — jogador primário (3 anos).** Pré-leitor. Já conhece a Teia, o Força e o Veloz. Motricidade fina incompleta. Atenção de 2 a 5 minutos.

**Adulto — operador (pai).** Abre o arquivo, regula o volume, tira o filho quando a hora acabou. Sem instalação, sem manual.

### Histórias de usuário

#### US1 — Apontar e causar o raio
*Como Murilo, quero apontar para o brinquedo e ver o raio sair dos olhos, para sentir que fui eu que causei aquilo.*

- Exatamente **um** brinquedo ativo por vez, sempre visível, mexendo devagar, com halo.
- Clique válido dispara `MIRAR` em ≤ 100 ms.
- Aim assist: círculo de **220 px** ao redor do brinquedo + **terço central** (30–70% × 25–75%).
- Clique fora não produz **nada**.
- Nenhum texto nem tutorial. O logo do peito é um losango, sem letra.

#### US2 — A coleta acontece sozinha
*Como Murilo, quero que o raio solte o brinquedo e o leve até a caixa, sem precisar fazer mais nada.*

- Sequência `MIRAR → LASER → COLETA → GUARDA` completa em **1,6 s a 2,0 s**.
- Cliques durante a sequência são ignorados.
- Ao fim da guarda, o próximo brinquedo entra após 0,4 s, **do outro lado**.
- O brinquedo voa em arco até a caixa (easing). Nunca linear. Nunca cai.
- A jaula não existe: o brinquedo está solto, parado no lugar, mexendo.

#### US3 — Ele voa o tempo todo; alterna de lado
*Como Murilo, quero que o Super Homem nunca pouse, para o voo ser o corpo dele.*

- Não existe pouso. O bob de voo é contínuo, inclusive em `ESPERANDO` (`ADR 0003`).
- Próximo brinquedo nasce do **outro lado**, distância do raio entre **600 e 900 px**.
- Alternância de lado por contador (`jogo.lado`), sem `Math.random`. **Não é inversão de tela.**
- A caixa acompanha o `transform` do herói.
- Posição contínua entre etapas.

#### US4 — Progresso visível, sem números (exceto o placar)
- Três estrelas na base; uma acende por brinquedo.
- No terceiro, festa: confete, **a caixa pulsa dourada**, jingle de 8 notas, ele gira no ar.
- Festa dura **3,0 s ± 0,2 s**, transição sem menu e sem texto.
- Nenhum número fora do placar (`US8`). A caixa não mostra dígitos.

#### US5 — Entender de onde vem o problema
- Sem vilão (`ADR 0001`). Os brinquedos ficaram espalhados.
- O brinquedo está sempre à vista, mexendo devagar, com halo.

#### US6 — A caixa agradece
- Na `GUARDA`, a tampa fecha e a caixa brilha, ≤ 350 ms.
- Som curto de guarda, não substitui o som do brinquedo.

#### US7 — O raio (o espetáculo)
*Como Murilo, quero ver dois raios vermelhos saírem dos olhos, para sentir que fui eu que olhei.*

- Durante `LASER`: **dois feixes finos vermelhos**, um de cada olho, que se juntam no brinquedo.
- Os feixes são nós pré-criados, só `transform`/`opacity`.
- `prefers-reduced-motion` mantém o raio (é o verbo do jogo) mas tira o brilho extra dos olhos.
- Nenhum flash branco. Nenhuma sirene. Zumbido agudo curto (`L9`).

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
- Primeiro toque no celular entra em tela cheia (`../PADRAES.md`).
- Nenhum `alert`/`confirm`.

#### US10 — Não quebra, não pede, não trava
- Zero rede. Delta do loop clamp 100 ms.
- Após 8 s ocioso, uma coleta de demonstração. Qualquer clique cancela.

#### US11 — Ele nunca pousa
- Bob de voo em todos os estados. Capa ondulando.
- Nenhuma poeira de chegada, nenhuma derrapagem, nenhum pé no chão.

#### US12 — O logo sem letra
- O peito leva um **losango amarelo/vermelho**, sem a letra S (L3).
- Reconhecível como Super Homem pela capa + azul + losango + botas, em 200 ms.

### Non-goals

- Toque como mecânica jogável (exceção: tela cheia no primeiro toque).
- Clark Kent, criptonita, cabine telefônica, jornal.
- Vilão, Lex, Zod, combate.
- Anéis, loop, spring, plataforma.
- Caixa como segundo placar / inventário que mostra os itens dentro.
- Quinto cenário, água, espaço.
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
murilo/super/raio-do-murilo.html
├── <svg id="palco">   céu · cenário · caixa+herói · brinquedo · raios · estrelas · poeira · confete · placar
├── Loop (rAF)         delta clamp 100 ms
├── FSM                ESPERANDO → MIRAR → LASER → COLETA → GUARDA → (FESTA)
├── Entrada            pointerdown → coords virtuais → resolveAlvo()
├── Cenários (dados)   3 configs
└── Áudio              Web Audio sintetizado
```

SVG no DOM, não Canvas. Só `transform` e `opacity` animam. Botão de voltar é HTML.

### Máquina de estados

| Estado | Duração | Saída |
| --- | --- | --- |
| `ESPERANDO` | indefinida | clique válido ou 8 s |
| `MIRAR` | 250 ms | automática |
| `LASER` | 450 ms | automática |
| `COLETA` | 400 ms | automática |
| `GUARDA` | 350 ms | automática |
| `FESTA` | 3000 ms | troca de cenário |

Cliques só em `ESPERANDO`.

### Sistema de coordenadas

Mundo virtual **1920×1080**, letterbox. Distância do raio 600–900 px. Aim assist 220 px + terço central.

### Dados dos cenários

| id | Nome | Céu | Brinquedos (por rodada) |
| --- | --- | --- | --- |
| `metropolis` | Metropolis | `#5FB4E4 → #E4F5FB` | Bola → Ursinho → Balão |
| `campo` | Campo | `#7FC7E8 → #EAF8E4` | Pipa → Carrinho → Patinho |
| `por-do-sol` | Pôr do sol | `#E0685F → #FFE4B5` | Gatinho → Bola de praia → Sorvete |

Ordem: `metropolis` → `campo` → `por-do-sol` → festa → `metropolis`…

Amigos/brinquedos: `amigos[jogo.guardados % amigos.length]` — o contador de guardados decide o brinquedo, não a visita ao cenário. Assim os três brinquedos do cenário aparecem em sequência, sem repetir. Objetos da Teia reutilizam desenhos e sons. Ursinho e carrinho são novos.

A arte de cada brinquedo e de cada cenário vive uma única vez no documento (`<defs id="biblioteca">` para os brinquedos, `<template>` para os cenários) e é instanciada por `<use>` / clone. Isso mantém o placar de 9 linhas barato em nós.

### Caixa

Grupo filho do herói (acompanha o `transform`). Tampa é nó separado (`g.tampa`) que gira na `COLETA` e fecha na `GUARDA`. A caixa fica **sempre à esquerda do herói** (`CAIXA_X = -68`), inclusive quando ele olha para a esquerda: o herói **inclina** (`rotate` até 9°) em vez de espelhar, então a caixa nunca troca de braço e nunca cobre o brinquedo.

### Raios

Dois `<path>` vermelhos em arco (curva quadrática), um por olho, que se abrem no meio do caminho e convergem no brinquedo — o `d` é recalculado a cada frame a partir dos olhos e do alvo. Um halo rosa mais grosso atrás de cada feixe dá volume. Aparecem só em `LASER` (e nos primeiros 90 ms de `COLETA`, desvanecendo). Espessura 5,5 px, `stroke-linecap: round`. Impacto: um círculo vermelho pulsante + núcleo claro no brinquedo.

### Placar

Mesmo modelo da casa. `registrarGuarda()` em `entrar(S.GUARDA)`: primeiro guardado cria a linha (ícone do brinquedo + número 1); os seguintes só incrementam o número. Uma linha por brinquedo distinto, máximo 9, fundo escuro cresce com as linhas. NF4 = **240**.

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

Pico medido: **230 nós** com as 9 linhas de placar e a festa cheia (defs 65, herói 39, placar 28, confete 14, cenário 15, estrelas 6, raios 6, poeira 5, brinquedo 1). Teto 240, folga de 10.

### Segurança e privacidade

Zero rede, zero dado, zero supply chain. LGPD: não há dado a tratar.

---

## 5. Riscos e roadmap

| # | Risco | Prob. | Impacto | Mitigação | Plano B |
| --- | --- | --- | --- | --- | --- |
| R1 | Raio assusta | Baixa | Alto | Fino, 0,45 s, zumbido curto, sem sirene; K1/K2 | Encurtar para 0,3 s / baixar ganho |
| R2 | Caixa cobre o brinquedo | Média | Alto | Caixa no braço esquerdo, brinquedo do outro lado; observar | Flutuar a caixa mais abaixo |
| R3 | Acha que é tiro/arma | Baixa | Médio | Dois raios finos, cor vermelha de visão de calor, sem "pew-pew" de arma | Zumbido mais suave |
| R4 | Autoplay bloqueia áudio | Média | Baixo | Destravar no pointerdown | Ícone piscando |
| R5 | Notebook antigo < 60 fps | Baixa | Médio | Só transform/opacity; DPR ≤ 2 | Menos confete |
| R6 | Escopo cresce (Lex, criptonita, logo com S) | Alta | Médio | Non-goals; ADRs 0001/0002 | Disciplina |
| R7 | Nós estouram 240 | Média | Médio | Placar estático; raios são 2 linhas | Cortar deco |
| R8 | Brinquedo some atrás da caixa | Média | Alto | Brinquedo do outro lado; caixa do lado do herói; z-order amigo > herói quando no ar | Ajustar âncoras |
| R9 | Voo engasga | Baixa | Médio | Bob senoidal simples; capa em 2 nós | Reduzir amplitude |
| R10 | Confusão com os outros jogos | Baixa | Baixo | Vitrine com figura do Super Homem | Nada |

### Rollout

| Fase | Entrega | Critério |
| --- | --- | --- |
| **F1 — Raio** | FSM, 1 cenário, arte provisória, sem áudio, sem placar | Clicar produz raio e coleta; herói nunca pousa — **feito** |
| **F2 — Voz e HUD** | 3 cenários, áudio, placar, caixa com tampa, voltar, attract, `M`/`F` | Zero erros; NF4 ≤ 240 — **feito** |
| **F3 — v1.0** | Regressão NF1–NF12, sessão com o Murilo | Smoke verde + 4 telas revisadas; **K1/K2 pendentes** |

### Definição de pronto

Arquivo único abre offline, Murilo aponta e uma coleta acontece em menos de 2,0 s, o raio não assusta, a caixa acompanha, placar mostra os brinquedos, NF1–NF12 verificados, nenhuma lei violada.

**Situação:** implementado. Smoke automatizado em jsdom percorre ~90 s de jogo: 230 nós de pico, 32 coletas, 9 linhas de placar, alternância de lado, laser, tampa e festa — zero erros de console. Telas dos três cenários, do raio, da coleta, da guarda e da festa conferidas em Chrome headless. Falta a sessão real com o Murilo (K1/K2).

---

## 6. Rastreabilidade

| Requisito | Origem |
| --- | --- |
| Zero derrota, zero cronômetro | Teia `ADR 0001` |
| Mouse varinha | Teia `ADR 0002` |
| Sem vilão | `docs/adr/0001-sem-vilao-brinquedos-pelo-vento.md` |
| Sempre Super Homem | `docs/adr/0002-sempre-super-homem-sem-clark.md` |
| Nunca pousa; alterna de lado | `docs/adr/0003-heroi-voa-nunca-pousa.md` |
| Placar desde v1.0 | `docs/adr/0004-placar-desde-a-v1.md` + `../PADRAES.md` |
| Tela cheia primeiro toque | `docs/adr/0005-tela-cheia-no-primeiro-toque.md` + `../PADRAES.md` |
| Laser dos dois olhos; caixa no braço | `docs/adr/0006-laser-dois-olhos-caixa-no-braco.md` |
| Vocabulário | `GLOSSARY.md` |
| Logo sem letra | `TEMA.md` L3 |
