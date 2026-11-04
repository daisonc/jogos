# PRD — Veloz do Murilo

- **Versão do documento**: 1.0 (v1.0 em design; nada implementado)
- **Escopo do produto**: v1.0 — três cenários, disparo horizontal, Eggman de longe, placar desde o primeiro dia, anéis como enfeite
- **Fonte de verdade de design**: [`TEMA.md`](./TEMA.md), [`GLOSSARY.md`](./GLOSSARY.md), [`docs/adr/`](./docs/adr/)
- **Jogos irmãos**: [`../PRD.md`](../PRD.md) (Teia), [`../hulk/PRD.md`](../hulk/PRD.md) (Força)
- **Status**: design aprovado pelo operador em 2026-11-03. Implementação não começou.

> `TEMA.md` responde *por que*; este PRD responde *o que sai e como se mede*.

---

## 1. Sumário executivo

### Problema

O Murilo tem 3 anos e já tem a Teia (mover o mundo com a teia) e o Força (levantar o impossível). Falta a distância: chegar rápido onde ninguém chegou. Qualquer jogo de corrida convencional falha nos três eixos — mira, leitura, derrota.

### Solução

Arquivo único onde o mouse é varinha: ele aponta, o Sonic vira bola, dispara, o mundo vira um borrão, e o amigo aparece. Zero texto (salvo o placar), zero derrota, zero cronômetro, Eggman de longe, anéis que não contam.

### Critérios de sucesso

| # | Tipo | Critério | Como medir |
| --- | --- | --- | --- |
| K1 | Observacional | O Murilo ri ao menos uma vez por sessão | Observação, prancheta de 5 min |
| K2 | Observacional | Pede "de novo" ou volta a clicar sem ser convidado | Observação, prancheta de 5 min |
| K3 | Técnico | Primeiro frame em ≤ 2 s, offline | Cronômetro, 3 execuções |
| K4 | Técnico | Frame time p95 ≤ 16,7 ms durante disparo e festa | DevTools › Performance, 60 s |
| K5 | Técnico | Zero rede e zero erros no console em 5 min | DevTools › Network + Console |

Anotação qualitativa (não é KPI): ri do Eggman ou desvia o olhar? o borrão assusta?

---

## 2. Experiência e funcionalidade

### Personas

**Murilo — jogador primário (3 anos).** Pré-leitor. Já conhece a Teia e o Força. Motricidade fina incompleta. Atenção de 2 a 5 minutos.

**Adulto — operador (pai).** Abre o arquivo, regula o volume, tira o filho quando a hora acabou. Sem instalação, sem manual.

### Histórias de usuário

#### US1 — Apontar e causar o disparo
*Como Murilo, quero apontar para o amigo e ver o Sonic disparar, para sentir que fui eu que causei aquilo.*

- Exatamente **um** amigo ativo por vez, sempre visível, preso na jaula, mexendo devagar.
- Clique válido dispara `GIRAR` em ≤ 100 ms.
- Aim assist: círculo de **220 px** ao redor do amigo + **terço central** (30–70% × 25–75%). Mirando no amigo/jaula.
- Clique fora não produz **nada**.
- Nenhum texto nem tutorial.

#### US2 — O socorro acontece sozinho
*Como Murilo, quero que o Sonic dispare, chegue e abra a jaula, sem precisar fazer mais nada.*

- Sequência `GIRAR → DISPARO → CHEGADA → ABERTURA → GRATIDÃO` completa em **2,4 s a 2,8 s**.
- Cliques durante a sequência são ignorados.
- Ao fim da gratidão, o amigo sai da cena e o próximo entra após 0,4 s, **do outro lado**.
- O disparo é reta horizontal com easing de arrancada e chegada. Nunca linear. Nunca sobe no céu.
- A jaula se abre em **duas metades que saem para fora da tela**. Sem explosão de fogo.

#### US3 — O herói fica onde para
*Como Murilo, quero que o Sonic fique onde parou, para o próximo disparo sair dali.*

- Não existe `VOLTA`. Após a abertura, o herói permanece no ponto de chegada (`ADR 0003`).
- Próximo amigo nasce do **outro lado**, distância horizontal de disparo entre **600 e 900 px**.
- Alternância de lado por contador (`jogo.lado`), sem `Math.random`. **Não é inversão de tela.**
- Clamp na borda. Posição contínua entre etapas.

#### US4 — Progresso visível, sem números (exceto o placar)
- Três estrelas na base; uma acende por amigo.
- No terceiro, festa: confete **+ anéis dourados girando**, jingle de 8 notas, Sonic pulando.
- Festa dura **3,0 s ± 0,2 s**, transição sem menu e sem texto.
- Nenhum número fora do placar (`US8`). Anéis não têm contador.

#### US5 — Entender de onde vem o problema
- Eggman aparece no início de cada resgate, no Egg Mobile, **fugindo**, distância horizontal ≥ **60% da largura** (≥ 1152 px), altura ≤ 40% da tela, nunca de frente, some em ≤ 2 s, nunca se aproxima (`L7`).
- Risada `ohoho` aguda e curta.
- O amigo está sempre à vista, preso na jaula.

#### US6 — Receber um "obrigado"
- Reação do amigo e do herói em `GRATIDÃO`, ≤ 600 ms.
- Cada amigo da lista tem reação própria.
- Som curto de gratidão ≤ 150 ms, não substitui o som do amigo.

#### US7 — O borrão (o espetáculo)
*Como Murilo, quero ver o mundo virar listra, para sentir a velocidade sem perder o amigo do olho.*

- Durante `DISPARO`: **linhas de velocidade** (nós pré-criados, só `transform`/`opacity`) e **estica do cenário** (`scaleX` em torno do centro, ≤ 1,08).
- O amigo continua visível o tempo todo (não some no borrão).
- `prefers-reduced-motion` desliga o estica e as linhas; o disparo continua.
- Nenhum flash branco. Nenhum slow motion.

#### US8 — Ver o que eu já salvei
- Placar no canto superior direito, igual aos irmãos. Uma linha por amigo já salvo.
- Cumulativo na sessão. Linha nasce no primeiro salvamento e não se move.
- No celular, 1,5×.
- Máximo **nove linhas** (3 cenários × 3 amigos).
- Anéis **não** entram no placar.

#### US9 — Abrir, calar e sair, sem manual
- Arquivo único, `file://`, sem servidor.
- `M` mudo ≤ 100 ms, ícone no topo-centro. `F` tela cheia no desktop.
- Botão de voltar à vitrine: HTML, canto superior esquerdo, ≥ 56 px, `../../index.html`.
- Primeiro toque no celular entra em tela cheia (`PADRAES.md`).
- Nenhum `alert`/`confirm`.

#### US10 — Não quebra, não pede, não trava
- Zero rede. Delta do loop clamp 100 ms.
- Após 8 s ocioso, um socorro de demonstração. Qualquer clique cancela.

#### US11 — Eggman nunca encara
- Um travesso por vez. Aparece com o amigo, some em ≤ 2 s.
- Rota garante `|herói − Eggman| ≥ 1152 px` nos dois lados.
- Gag opcional futuro; v1.0 só a fuga.

#### US12 — Anéis que não contam
- Anéis estáticos no cenário (2–3, decorativos, não são alvo).
- Na festa, anéis caem girando como o confete (nós pré-criados).
- Nenhum contador de anéis. Nenhum ícone de anel no placar.

### Non-goals

- Toque como mecânica jogável (exceção: tela cheia no primeiro toque).
- Super Sonic, transformação, esmeraldas.
- Corrida contra Eggman, chefe, combate.
- Loop jogável, rampas, springs, proxies de plataforma.
- Anéis como missão ou como segunda contagem.
- Quinto cenário, água, Chemical Plant.
- Dois jogadores, o jogo do Pedro.
- Narração, analytics, persistência, dependência.

---

## 3. Requisitos de sistema de IA

**Não aplicável.** FSM determinística de 7 estados.

---

## 4. Especificação técnica

### Visão de arquitetura

Arquivo único, sem build. Mesma arquitetura dos irmãos.

```
murilo/sonic/veloz-do-murilo.html
├── <svg id="palco">   céu · cenário · anéis deco · eggman · jaula · amigo · herói · linhas · estrelas · poeira · confete · anéis de festa · placar
├── Loop (rAF)         delta clamp 100 ms
├── FSM                ESPERANDO → GIRAR → DISPARO → CHEGADA → ABERTURA → GRATIDÃO → (FESTA)
├── Entrada            pointerdown → coords virtuais → resolveAlvo()
├── Cenários (dados)   3 configs
├── Eggman             template + rota (L7)
└── Áudio              Web Audio sintetizado
```

SVG no DOM, não Canvas. Só `transform` e `opacity` animam. Botão de voltar é HTML.

### Máquina de estados

| Estado | Duração | Saída |
| --- | --- | --- |
| `ESPERANDO` | indefinida | clique válido ou 8 s |
| `GIRAR` | 250 ms | automática |
| `DISPARO` | 900 ms | automática |
| `CHEGADA` | 250 ms | automática |
| `ABERTURA` | 500 ms | automática |
| `GRATIDÃO` | 600 ms | automática; amigo sai; próximo em 400 ms |
| `FESTA` | 3000 ms | troca de cenário |

### Sistema de coordenadas

Mundo virtual **1920×1080**, letterbox. Distância de disparo 600–900 px. Aim assist 220 px + terço central.

### Dados dos cenários

| id | Nome | Céu | Amigos (por rodada) | Jaula |
| --- | --- | --- | --- | --- |
| `greenhill` | Green Hill | `#87CEEB → #C9E9F6` | Coelhinho → Gatinho → Balão | Arbusto fechado |
| `canyon` | Canyon | `#F8B26A → #FFE0B2` | Cervinho → Pipa → Cachorrinho | Pedra |
| `cidade` | Cidade | `#A8DADC → #FFF3D6` | Patinho → Bola → Sorvete | Jaula de Eggman |

Ordem: `greenhill` → `canyon` → `cidade` → festa → `greenhill`…

Amigos: `amigos[(visitas-1) % amigos.length]`. Objetos da Teia reutilizam desenhos e sons.

### Eggman

`EGGMAN = { tpl, som: 'ohoho', rota }`. Aparece em `nascerAmigo()`, some em ≤ 2 s. Rota garante ≥ 1152 px do herói nos dois lados. Um só renderizado.

### Placar

Mesmo modelo da casa. `registrarSocorro()` em `entrar(S.ABERTURA)`. NF4 = **240**.

### Anéis

- Deco: 2–3 `<use>` de um template de anel no cenário, estáticos.
- Festa: 8 anéis pré-criados, caem girando (mesma física do confete, outro desenho).
- Nenhum contador.

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

Pico estimado: cena ~140 + placar até 9 linhas ~50 + eggman ~10 + anéis de festa 8 = **~210**. Teto 240.

### Segurança e privacidade

Zero rede, zero dado, zero supply chain. LGPD: não há dado a tratar.

---

## 5. Riscos e roadmap

| # | Risco | Prob. | Impacto | Mitigação | Plano B |
| --- | --- | --- | --- | --- | --- |
| R1 | Eggman assusta | Média | Alto | L7 integral; K1/K2; risada aguda curta | Remover Eggman |
| R2 | Borrão assusta | Baixa | Alto | Estica ≤ 1,08; `prefers-reduced-motion`; amigo sempre visível | Tirar o estica, manter o disparo |
| R3 | Disparo longo demais | Baixa | Médio | 900 ms; total 2,4–2,8 s | Encurtar para 750 ms |
| R4 | Autoplay bloqueia áudio | Média | Baixo | Destravar no pointerdown | Ícone piscando |
| R5 | Notebook antigo < 60 fps | Baixa | Médio | Só transform/opacity; DPR ≤ 2 | Menos confete/anéis |
| R6 | Escopo cresce (esmeralda, loop, spring) | Alta | Médio | Non-goals | Disciplina |
| R7 | Nós estouram 240 | Média | Médio | Anéis estáticos; placar estático | Cortar deco |
| R8 | Amigo some no borrão | Média | Alto | Amigo sempre no lugar, opacidade 1 | Não esticar o amigo |
| R9 | Sonic cobre o amigo ao chegar | Média | Médio | Chegada ao lado da jaula, nunca em cima | Ajustar âncora |
| R10 | Confusão com os outros jogos | Baixa | Baixo | Vitrine com figura do Sonic | Nada |

### Rollout

| Fase | Entrega | Critério |
| --- | --- | --- |
| **F1 — Disparo** | FSM, 1 cenário, arte provisória, sem áudio, sem placar | Clicar produz disparo e abertura; herói fica |
| **F2 — Voz e HUD** | 3 cenários, áudio, Eggman, placar, anéis, voltar, attract, `M`/`F` | Zero erros; NF4 ≤ 240 |
| **F3 — v1.0** | Regressão NF1–NF12, sessão com o Murilo | Pendente |

### Definição de pronto

Arquivo único abre offline, Murilo aponta e um socorro acontece em menos de 2,8 s, o borrão não assusta, Eggman não chega perto, placar mostra os amigos, NF1–NF12 verificados, nenhuma lei violada.

**Situação:** pendente. Nada implementado.

---

## 6. Rastreabilidade

| Requisito | Origem |
| --- | --- |
| Zero derrota, zero cronômetro | Teia `ADR 0001` |
| Mouse varinha | Teia `ADR 0002` |
| Eggman de longe | `docs/adr/0001-eggman-de-longe.md` + Teia L7 |
| Sempre Sonic | `docs/adr/0002-sempre-sonic-sem-super-sonic.md` |
| Herói fica | `docs/adr/0003-heroi-fica-onde-para.md` |
| Placar desde v1.0 | `docs/adr/0004-placar-desde-a-v1.md` + `PADRAES.md` |
| Tela cheia primeiro toque | `docs/adr/0005-tela-cheia-no-primeiro-toque.md` + `PADRAES.md` |
| Anéis não contam | `TEMA.md` L8 |
| Vocabulário | `GLOSSARY.md` |
