# PRD — Força do Murilo

- **Versão do documento**: 1.0 (v1.0 em design; nada implementado)
- **Escopo do produto**: v1.0 — três cenários, loop completo sem volta, placar desde o primeiro dia, botão de voltar à vitrine
- **Fonte de verdade de design**: [`TEMA.md`](./TEMA.md), [`GLOSSARY.md`](./GLOSSARY.md), [`docs/adr/`](./docs/adr/)
- **Jogo irmão**: [`../PRD.md`](../PRD.md) (Teia do Murilo) — herda leis, arquitetura e lições; **não herda** vocabulário nem features que aqui não existem (travesso, inversão de tela, Bruce)
- **Status**: design aprovado pelo operador em 2026-11-03. **v1.0 concluída e homologada em 2026-11-03** (F1–F3 concluídas). **v1.1 concluída e homologada em 2026-11-03** (amigos alternam por rodada, com objetos da Teia); K1/K2 reconfirmados e NF1–NF12 verdes.

> Este documento **não repete** a doutrina de design. Ele a referencia. `TEMA.md` responde *por que o jogo é assim*; este PRD responde *o que sai e como se mede*. Se os dois divergirem, o `TEMA.md` ganha na intenção e este PRD ganha na entrega até a divergência ser resolvida.

---

## 1. Sumário executivo

### Problema

O Murilo tem 3 anos, joga em notebook com mouse, e não consegue mirar, ler, nem perder. Ele já tem a Teia — apontar e o mundo balançar até o resgate. Este jogo é **outro poder para o mesmo dedo**: a força que socorre. Não é uma skin do Homem-Aranha verde. É a fantasia de levantar o impossível e ver o amigo aparecer.

### Solução

Um jogo de navegador em arquivo único onde o mouse é varinha, não punho: ele aponta para o obstáculo, o Hulk pula, a pedra se parte, e o amigo sai. Zero texto (salvo o placar), zero derrota, zero cronômetro, zero vilão, três cenários de natureza, e um estrondo que é visual, não um estouro de som.

### Critérios de sucesso

Dois observacionais (sessão real com o Murilo, protocolo em `TEMA.md` § *Como saber se funcionou*) e três técnicos (medição direta no notebook).

| # | Tipo | Critério | Como medir |
| --- | --- | --- | --- |
| K1 | Observacional | O Murilo ri ao menos uma vez por sessão | Observação, prancheta de 5 min |
| K2 | Observacional | Ao fim da festa, pede "de novo" ou volta a clicar sem ser convidado | Observação, prancheta de 5 min |
| K3 | Técnico | Duplo-clique no `.html` renderiza o primeiro frame em ≤ 2 s, offline | Cronômetro manual, 3 execuções |
| K4 | Técnico | Frame time p95 ≤ 16,7 ms (60 fps) durante salto e festa | DevTools › Performance, 60 s de gravação |
| K5 | Técnico | Zero requisições de rede e zero erros no console em 5 min de sessão | DevTools › Network + Console |

**Fora dos KPIs, por decisão herdada da Teia:** tempo de sessão com alvo numérico. K1 e K2 medem a coisa certa. Anotação qualitativa (não é KPI): ele ri do estrondo ou se assusta com o tremor? acha o amigo na tela?

---

## 2. Experiência e funcionalidade

### Personas

**Murilo — jogador primário (3 anos).** Pré-leitor. Motricidade fina incompleta: não mira, não arrasta, não combina botões. Atenção de 2 a 5 minutos. Aprende por repetição e por causa-efeito, não por instrução. **Já conhece a Teia**: sabe apontar, conhece o placar, conhece a festa. Este jogo não pode ser um passo atrás em generosidade.

**Adulto — operador (pai).** Abre o arquivo, regula o volume, tira o filho do jogo quando a hora acabou. Não quer instalar nada e não quer ler manual. É a interface de configuração do jogo, porque o jogo não tem configuração.

### Histórias de usuário

#### US1 — Apontar e causar

*Como Murilo, quero apontar para o obstáculo e ver o Hulk pular, para sentir que fui eu que causei aquilo.*

**Critérios de aceite**

- Exatamente **um** obstáculo ativo por vez, sempre visível, com o amigo preso nele **sempre à vista** (cabeça e pata para fora, mexendo devagar). Nunca escondido debaixo do obstáculo.
- Um clique válido dispara o `AGACHAR` em ≤ 100 ms.
- Aim assist em duas camadas, em coordenadas virtuais 1920×1080, **idêntico ao da Teia**, mirando no **obstáculo** (o que está grande na tela), não no amigo (que é pequeno):
  - **(a)** círculo de acerto com raio de **220 px** ao redor do obstáculo;
  - **(b)** se houver exatamente um obstáculo ativo, qualquer clique dentro do **terço central** (30%–70% da largura, 25%–75% da altura) também conta.
- Clique fora de (a) e (b) não produz **nada**: sem som, sem animação, sem mudança de estado, sem cursor de erro.
- Nenhum texto nem tutorial é exibido em momento algum. Ícones só existem como feedback de tecla do adulto (`US9`) e no botão de voltar.

#### US2 — O socorro acontece sozinho

*Como Murilo, quero que o Hulk pule, parta a pedra e traga o amigo, para ver o socorro sem precisar fazer mais nada.*

**Critérios de aceite**

- Após o clique válido, a sequência `AGACHAR → SALTO → IMPACTO → ROMPIMENTO → GRATIDÃO` completa em **2,4 s a 2,8 s** sem nenhuma entrada adicional.
- Cliques durante a sequência são **ignorados**, não enfileirados (`TEMA.md` L6).
- Ao fim da gratidão, o amigo sai da cena num pulinho para fora da tela e o próximo obstáculo entra após 0,4 s, **do outro lado** do herói.
- O salto é um arco com aceleração e desaceleração (easing), nunca linear. Pico do arco entre 25% e 35% da altura da tela; o herói nunca some no céu.
- O rompimento parte o obstáculo em **duas metades que saem para fora da tela**; não há explosão, fumaça de bomba nem fogo.

#### US3 — O herói mora no cenário

*Como Murilo, quero que o Hulk fique onde pousou, para o próximo salto sair dali e não de um teleporte.*

**Critérios de aceite**

- Não existe estado `VOLTA`. Após o rompimento, o herói permanece no ponto de pouso (`ADR 0003`).
- O próximo obstáculo nasce do **outro lado** do herói, a uma distância de salto (horizontal entre pouso e próximo obstáculo entre **600 e 900 px** virtuais).
- A alternância de lado é por contador (`jogo.lado`), determinística, sem `Math.random`. **Não é inversão de tela**: o cenário não espelha; só herói e obstáculo trocam de hemisfério.
- Se o pouso chegar perto da borda, o próximo obstáculo nasce para o centro (clamp). Nenhum alvo fora de alcance (`TEMA.md` L1).
- A posição do herói é contínua entre etapas: sem salto de posição entre o fim de uma etapa e o início da seguinte.

#### US4 — Progresso visível, sem números (exceto o placar)

*Como Murilo, quero ver uma festa a cada três amigos, para saber que avancei sem saber ler.*

**Critérios de aceite**

- Três estrelas na base da tela; uma acende por amigo liberto.
- No terceiro socorro, a festa começa automaticamente: confete, jingle de 8 notas, herói pulando.
- A festa dura **3,0 s ± 0,2 s** e transiciona para o próximo cenário sem menu, sem tela intermediária e sem texto.
- Ao reiniciar o ciclo, as três estrelas apagam.
- Nenhum número, dígito ou contador é exibido **fora do placar** (`US8`). Nenhuma letra ou palavra aparece em nenhum estado.

#### US5 — Entender de onde vem o problema

*Como Murilo, quero ver o amigo preso e o obstáculo fechando o caminho, para entender o que fazer sem ninguém explicar.*

**Critérios de aceite**

- Não há travesso, não há vilão, não há risada de antagonista (`ADR 0001`). A causa é o obstáculo: pedra, tronco ou rocha.
- O amigo preso está sempre visível e mexendo devagar — é o convite ao clique, junto do halo sutil do obstáculo.
- Nenhum personagem além do herói e do amigo aparece na cena.
- O rosto do herói **nunca** é de raiva fechada: cara de esforço. Sem rugido, sem "grrr" (`TEMA.md` L7 relida; `ADR 0002`).

#### US6 — Receber um "obrigado"

*Como Murilo, quero ver o amigo feliz depois de solto, para sentir que o que eu fiz deu certo.*

**Critérios de aceite**

- Em todo socorro, uma reação do amigo e do herói começa no início de `GRATIDÃO` e termina em **≤ 600 ms**. Nenhuma etapa da FSM é alongada (a faixa de 2,4 s a 2,8 s de `US2` se mantém).
- Cada amigo da lista do cenário tem reação própria, reconhecível sem texto (espreguiça, giro, pendulo, quiques, abana, orelha, quica).
- Um som curto de gratidão (**≤ 150 ms**) toca no início da reação. Ele **não substitui** o som do amigo no rompimento.
- Só `transform` e `opacity` animam, em nós existentes.
- Cliques durante `GRATIDÃO` continuam ignorados (`TEMA.md` L6).

#### US7 — A cena lembra

*Como Murilo, quero ver a rachadura no chão onde a pedra se partiu, para saber que aquele socorro aconteceu.*

**Critérios de aceite**

- Após cada rompimento, fica uma **rachadura** no chão (um `<path>`), até a festa.
- As metades do obstáculo saem para fora da tela e **não ficam** como escombro.
- Um cenário nunca passa de **3 rachaduras + 1 obstáculo ativo**.
- A rachadura é estática: não anima, não entra no limite de ≤ 80 animados/frame.
- Remover as rachaduras restaura o jogo sem elas, sem tocar na FSM.

#### US8 — Ver o que eu já salvei

*Como Murilo, quero ver o desenho de cada amigo que eu salvei e quantos eu já salvei, para sentir a coleção crescer sem precisar ler.*

**Critérios de aceite**

- Placar no **canto superior direito** (coordenadas virtuais), sem cobrir o herói nem o obstáculo ativo. Igual ao da Teia (`ADR 0004` da Teia; `ADR 0004` deste jogo).
- Uma linha por amigo **já salvo**: o desenho do item à esquerda e o número de salvamentos à direita.
- Só aparecem itens já salvos; nenhum ícone com zero.
- A contagem é **cumulativa na sessão** — não zera na festa nem na troca de cenário.
- A linha nasce no primeiro salvamento do item e **não se reposiciona** depois (o placar só cresce para baixo).
- Determinístico e sem `Math.random`; nenhum estado persistido.
- Só `transform` e `opacity` animam; o placar é **estático** (não soma animados/frame).
- É a **única** exceção a L3 e L8; nenhum outro texto, letra ou número aparece no jogo.
- No celular, o placar é ampliado **1,5×** (lição da Teia v1.4.2), ancorado no próprio canto; no desktop, nenhum transform.
- Com três amigos por cenário × três cenários, o placar tem no máximo **nove linhas** (três por rodada de cada cenário).

#### US9 — Abrir, calar e sair, sem manual

*Como adulto, quero abrir o jogo com dois cliques, mutar com uma tecla, pôr em tela cheia e voltar à vitrine, para não precisar de instruções.*

**Critérios de aceite**

- Um único arquivo `.html`, aberto por duplo-clique (`file://`), sem servidor, sem instalação, sem internet.
- A tecla `M` alterna mudo em ≤ 100 ms, com feedback visual discreto de 1 s (ícone de som riscado no topo-centro).
- A tecla `F` alterna tela cheia.
- Botão de voltar à vitrine: elemento **HTML** (`<a class="voltar">`), canto superior esquerdo, alvo ≥ 56 px, sem texto, só seta; aponta para `../../index.html`; fora do `<svg id="palco">`, não soma nós; respeita `env(safe-area-inset-*)` e `prefers-reduced-motion`.
- Indicador de mudo no **topo-centro**, para não coincidir com o botão de voltar.
- Nenhum `alert`, `confirm`, prompt de permissão ou diálogo do navegador em nenhum fluxo.

#### US10 — Não quebra, não pede, não trava

*Como adulto, quero poder deixar ele jogando sozinho, para não virar eu o controle do jogo.*

**Critérios de aceite**

- Nenhuma requisição de rede durante toda a execução.
- Trocar de aba e voltar não faz a animação saltar: o delta do loop é **limitado a 100 ms**.
- Após 8 s sem nenhuma entrada, o herói executa **um** socorro de demonstração automático (attract mode), que ensina o gesto a quem está olhando. Qualquer clique cancela a demonstração imediatamente.
- A demonstração não altera a contagem de estrelas do jogador além do socorro que ela própria realiza.

#### US11 — O estrondo que não assusta

*Como Murilo, quero sentir a força no pouso, para achar graça sem levar susto.*

**Critérios de aceite**

- Tremor de **2–3 px**, duração **≤ 150 ms**, uma vez, só no `IMPACTO`. Transform no palco, não em cada nó.
- `prefers-reduced-motion` **desliga** o tremor. A poeira grande permanece.
- Nenhum tremor vertical que leia como queda. Nenhum flash branco. Nenhum slow motion.
- Som de impacto: `thud` seco, passa-baixa, **80–120 ms**, pico ≤ −6 dBFS. Sem rugido, sem "grrr", nada grave e longo (`TEMA.md` L9).
- A força se lê nos olhos: poeira + pedra se partindo + amigo aparecendo.

#### US12 — Tela cheia no celular (cortesia)

*Como adulto, quero o jogo em tela cheia no celular sem gesto especial, para aproveitar sem teclado e sem duplo-toque.*

**Critérios de aceite** *(padrão da casa `PADRAES.md`)*

- A tecla `F` continua valendo no desktop, inalterada. Mouse não alterna tela cheia.
- No carregamento, se o ponteiro primário é grosso (`(pointer: coarse)`), o jogo **tenta** entrar em tela cheia.
- No **primeiro toque** (`pointerdown` de `pointerType: 'touch'`), se ainda não estiver em tela cheia, entra. O mesmo toque continua sendo socorro.
- **Duplo-toque sai.**
- Metatags `mobile-web-app-capable`, `apple-mobile-web-app-capable`, `apple-mobile-web-app-status-bar-style` e `viewport-fit=cover`.
- Onde a API de tela cheia não existe (ex.: iPhone), o gesto é inócuo — nada quebra e nenhum diálogo aparece.
- Toque **não** é mecânica jogável: o alvo continua sendo notebook com mouse.

### Non-goals

Explicitamente **fora** do escopo da v1.0:

- Toque, celular e tablet como alvo jogável (única exceção: tela cheia no primeiro toque, `US12`).
- Controle por teclado para a criança. O teclado é do adulto.
- Travesso, vilão, Duende-equivalente, Abominável, Líder (`ADR 0001`).
- Bruce Banner, transformação, fúria, "modo raiva" (`ADR 0002`).
- Inversão de lado com espelhamento do cenário (a Teia v1.4 feature 2). Aqui quem troca de lado é o próximo obstáculo, por contador (`US3`).
- Mais de três amigos por cenário; quinto cenário.
- Quinto cenário, cidade, água/praia/rio, caverna.
- Combate, inimigo, vida, dano, game over, cronômetro, dificuldade.
- Dois jogadores, o jogo do Pedro.
- Publicação, distribuição, monetização, loja.
- Internacionalização. O jogo não tem texto (salvo dígitos do placar).
- Narração por voz, descartada em definitivo (herança da Teia).
- Analytics, telemetria, qualquer coleta de dado.
- Placar de recorde, ranking, pontuação por tempo, progresso salvo.

---

## 3. Requisitos de sistema de IA

**Não aplicável.** Não há modelo, inferência, agente, RAG, avaliação de qualidade de saída nem custo por token. O único "modelo" é uma máquina de estados determinística de 7 estados.

---

## 4. Especificação técnica

### Visão de arquitetura

Arquivo único, sem build, sem dependência. Cinco módulos com responsabilidade única, comunicando por chamadas diretas — **sem ECS, sem framework, sem barramento de eventos** (mesma decisão da Teia: ~12 elementos; ECS é cerimônia sem retorno).

```
murilo/hulk/forca-do-murilo.html   (único arquivo)
├── <svg id="palco">        camadas: céu · cenário · rachaduras · obstáculo · amigo · herói · estrelas · partículas · placar
├── Loop (rAF)              delta com clamp de 100 ms → update → render
├── Estado (FSM)            ESPERANDO → AGACHAR → SALTO → IMPACTO → ROMPIMENTO → GRATIDÃO → (FESTA) → ESPERANDO
├── Entrada                 pointerdown → coords virtuais → resolveAlvo() → transição
├── Cenários (dados)        array de 3 configs: paleta, silhuetas, obstáculo, amigo, âncoras esq/dir
├── Placar                  igual ao da Teia; nasce com o jogo
└── Áudio                   Web Audio sintetizado; destrava no primeiro pointerdown
```

**Renderização: SVG no DOM, não Canvas.** Mesma justificativa da Teia: arte vetor chapado, contagem de nós baixa, antialiasing e hit-testing trivial. Só `transform` e `opacity` são animados. **Gatilho de troca:** se K4 falhar de forma persistente, o render vira Canvas 2D mantendo a mesma FSM.

**Botão de voltar é HTML**, fora do SVG (não escala com letterbox, não soma nós).

### Máquina de estados

| Estado | Duração | Entrada | Saída |
| --- | --- | --- | --- |
| `ESPERANDO` | indefinida | — | clique válido, ou 8 s de ociosidade |
| `AGACHAR` | 250 ms | clique válido | automática |
| `SALTO` | 1000 ms | — | automática |
| `IMPACTO` | 250 ms | — | automática |
| `ROMPIMENTO` | 500 ms | — | automática |
| `GRATIDÃO` | 600 ms | — | automática; amigo sai; próximo obstáculo em 400 ms |
| `FESTA` | 3000 ms | 3º socorro | automática, troca de cenário |

Cliques só são lidos em `ESPERANDO`. Em qualquer outro estado, são descartados na entrada — não há fila.

**Não há `VOLTA`.** A gratidão acontece no ponto de pouso (`ADR 0003`).

### Sistema de coordenadas

Mundo virtual fixo de **1920×1080**, escalado por `min(largura/1920, altura/1080)` e centralizado com letterbox preto. Toda regra de acerto, distância e tamanho é expressa em unidades virtuais, e a entrada converte a posição do ponteiro pelo inverso da transformação. Consequência: o aim assist não muda de generosidade entre telas diferentes.

**Âncoras por cenário (dados, não `if`):** cada cenário define par de posições esquerda/direita para espera do herói, obstáculo+amigo e pouso. `jogo.lado` escolhe o par. Pouso e espera são o mesmo ponto (`ADR 0003`): o herói fica onde pousou.

| Grandeza | Valor virtual |
| --- | --- |
| Distância horizontal pouso → próximo obstáculo | 600–900 px |
| Raio de aim assist | 220 px |
| Pico do salto (y) | entre 25% e 35% da altura (270–378 px do topo) |
| Tremor | 2–3 px, ≤ 150 ms |
| Halo do obstáculo | mesmo padrão do halo dourado da Teia, reutilizado |

### Dados dos cenários

Data-driven: os três cenários são configuração, não código. Nenhum comportamento é hard-coded por cenário.

| id | Nome | Céu | Amigos (alternam por rodada) | Obstáculo |
| --- | --- | --- | --- | --- |
| `montanha` | Montanha | `#87CEEB → #E8F4F8` | Cabritinho → Gatinho → Pipa | Pedregulho |
| `floresta` | Floresta | `#C9E9F6 → #DFF6E4` | Cervinho → Balão → Patinho | Tronco caído |
| `campo` | Campo seco | `#A8DADC → #FFF3D6` | Coelhinho → Cachorrinho → Bola | Rocha rachada |

Cada cenário define `amigos[]` (id, template, som, reação, tom). A rodada (visita ao cenário) escolhe `amigos[(visitas-1) % amigos.length]`. Dentro da rodada, o amigo é sempre o mesmo. Objetos da Teia reutilizam desenhos e sons do jogo do Aranha.

*(Céu: cores do topo para a base da tela.)*

Cada cenário define: paleta, silhuetas (pinheiros / árvores / capim), par de âncoras esq/dir, template do obstáculo, template do amigo, sons. Nenhum `if (cenario === …)` no comportamento.

**Ordem do ciclo:** `montanha` → `floresta` → `campo` → festa → `montanha`…

### Placar (herdado e desde o primeiro dia)

Mesmo modelo da Teia v1.4 (`../docs/adr/0004-placar-e-inversao-de-lado.md`), sem a inversão:

- Constantes em `PLACAR`; `<g id="placar">` no topo (acima do cenário, abaixo do flash e do aviso de mudo).
- Estado `placarContagem` (id → nº) e `placarLinhas` (id → nó de texto).
- Em `entrar(S.ROMPIMENTO)`, `registrarSocorro()` lê `jogo.amigoId`/`jogo.amigoTpl` (gravados ao montar o obstáculo), incrementa e, no primeiro salvamento, clona a arte do template do amigo numa linha nova (sem halo).
- Dígito é `<text>` com fonte de sistema; linha nasce em `y` fixo; fundo (`<rect>`) cresce com o número de itens.
- Em telas de toque (`matchMedia('(pointer: coarse)')`): `translate(âncora) scale(1.5) translate(-âncora)`.
- **NF4 nasce em 240**, não em 150 (`ADR 0004` deste jogo).

### Áudio

- **100% sintetizado em Web Audio**, sem arquivo de áudio. É o que torna "arquivo único + offline" possível.
- `thud` = impacto seco, passa-baixa, 80–120 ms; `whoosh` = salto, curto, não grave; `pop`/estalo = rompimento; sons de amigo = um ramo por amigo; `grato` = duas notas senoidais ascendentes de 100 a 150 ms; jingle = arpejo maior de 8 notas; sopro de agachar = ruído curtíssimo.
- Contexto destravado no primeiro `pointerdown`; se `suspended`, `resume()`.
- Nível e limitador: ver **NF6**. Master −12 dBFS + limitador, iguais à Teia. Nenhum compressor novo.
- `M` zera e restaura o master gain instantaneamente.
- **Sem rugido. Sem "grrr". Sem narração.**

### Requisitos não-funcionais

Fonte única dos limites verificáveis. As seções acima explicam o *porquê*; esta tabela é o que se confere antes de dizer que está pronto, e o que se cita em code review.

| # | Requisito | Limite | Verificação |
| --- | --- | --- | --- |
| NF1 | Arquivo único, `file://`, sem build | 1 `.html`, zero instalação | abrir por duplo-clique em máquina limpa |
| NF2 | Tempo até o primeiro frame | ≤ 2 s | cronômetro, 3 execuções (K3) |
| NF3 | Frame time durante salto e festa | p95 ≤ 16,7 ms | DevTools › Performance, 60 s (K4) |
| NF4 | Carga de render | ≤ **240** nós SVG; ≤ 80 animados/frame; só `transform` e `opacity` | auditoria de código + Elements |
| NF5 | `devicePixelRatio` | ≤ 2 | auditoria de código |
| NF6 | Nível de áudio | master −12 dBFS com limitador; nenhum pico > −6 dBFS | `AnalyserNode` + audição |
| NF7 | Delta do loop | clamp em 100 ms | alt-tab e retorno, sem salto visual |
| NF8 | Rede em runtime | zero requisições | DevTools › Network (K5) |
| NF9 | Persistência | zero `localStorage`/`sessionStorage`/cookie/IndexedDB | DevTools › Application + auditoria |
| NF10 | Código e mídia de terceiros | zero dependência, script remoto, fonte ou imagem externa | auditoria do arquivo |
| NF11 | Determinismo | nenhuma aleatoriedade em decisão de jogo; sorteio só em partículas | auditoria de código |
| NF12 | Navegadores suportados | Chrome, Edge e Firefox, 2 versões mais recentes, Windows | execução manual nos três |

**Orçamento de nós (NF4) — estimativa da v1.0.**

| Conjunto | Nós estimados | Observação |
| --- | --- | --- |
| Cena sozinha (estilo Teia v1.3) | 130–150 | Céu, silhuetas, herói, obstáculo, amigo, estrelas |
| Placar com 3 amigos | ~30–40 | Estático; não entra nos 80 animados |
| Rachaduras (3) | 3 | Estáticas |
| **Pico esperado** | **~180–200** | Teto absoluto: **240** |

Se a arte do Hulk ou da montanha estourar, corta-se detalhe de cenário antes de cortar o amigo ou o placar.

### Integration points

**Nenhum, e isso é requisito** (**NF8**, **NF10**). Sem backend, sem CDN, sem fonte externa, sem imagem externa, sem analytics. O arquivo é autossuficiente por construção.

Único acoplamento **fora do arquivo**: o botão de voltar aponta para `../../index.html` (a vitrine), por URL relativa. Funciona em `file://` e em hospedagem. Se a vitrine não existir, o botão é inócuo — não quebra o jogo.

### Segurança e privacidade

**Política.** O jogo não fala com o mundo (**NF8**), não lembra de nada (**NF9**) e não carrega código de terceiros (**NF10**). Três consequências práticas: funciona em modo avião; fechou, esqueceu — inclusive o mudo; e não há superfície de supply chain a auditar.

**Dado pessoal: nenhum.** Não há campo, identificador, conta ou upload. Não há tratamento de dados sob a LGPD porque não há dado a tratar — e essa é a razão de a ausência de persistência ser requisito, não conveniência.

Vetor de risco residual: nenhum conhecido além da integridade do próprio arquivo.

---

## 5. Riscos e roadmap

### Riscos

| # | Risco | Prob. | Impacto | Mitigação | Plano B |
| --- | --- | --- | --- | --- | --- |
| R1 | **O tremor assusta** e o Murilo abandona sem saber explicar | Média | Alto | 2–3 px, 150 ms, uma vez; `prefers-reduced-motion`; L9 no som | Tirar o tremor; deixar só a poeira |
| R2 | Ele não descobre que precisa clicar no **obstáculo** (amigo é pequeno) | Média | Alto | Aim assist no obstáculo (220 px + terço central); halo; amigo mexendo; attract mode | Aumentar o halo; se precisar, aim assist no terço inteiro já cobre |
| R3 | Hulk lido como **bravo/monstro** | Média | Alto | Cara de esforço; sem rugido; sem sobrancelha fechada; `ADR 0002` | Suavizar mais o rosto; se persistir, revisar a silhueta |
| R4 | Autoplay bloqueia o áudio | Média | Baixo | Destravar no primeiro `pointerdown`; `resume()` | Ícone piscando pedindo um clique; nunca bloquear o jogo por áudio |
| R5 | Notebook antigo não sustenta 60 fps | Baixa | Médio | Só `transform`/`opacity`; DPR ≤ 2; ≤ 80 nós animados | Reduzir confete; se persistir, Canvas 2D |
| R6 | Ele fecha a janela ou sai da tela cheia | Média | Baixo | `F`; botão de voltar; área de jogo 100% da viewport | Aceitar: é responsabilidade do adulto |
| R7 | Escopo cresce para vilão, Bruce, cidade, quinta cenário | Alta | Médio | Non-goals explícitos; ADRs 0001/0002 | Nada. Este risco não tem mitigação técnica, só disciplina |
| R8 | Rachadura + placar + Hulk estouram 240 nós | Média | Médio | Metades saem da tela; rachadura é 1 path; placar de 3 linhas | Simplificar cenário; nunca cortar amigo/placar |
| R9 | Amigo pouco visível (pequeno, atrás do obstáculo) | Média | Alto | Sempre à vista; mexendo; halo no obstáculo; teste "onde ele clica primeiro" | Aumentar o amigo; trazer o amigo mais para a borda do obstáculo |
| R10 | Herói cobre o amigo ou o obstáculo ao pousar | Média | Médio | Pouso ao lado, nunca em cima; herói grande mas atrás na camada quando parado | Ajustar z-order e âncora de pouso |
| R11 | Salto longo demais e ele clica repetido | Baixa | Médio | 1000 ms de salto; total 2,4–2,8 s (mais curto que o teto da Teia) | Encurtar o salto para 800 ms |
| R12 | Confusão com a Teia (ele procura o Homem-Aranha) | Baixa | Baixo | Vitrine com figura do Hulk; identidade visual verde/roxo | Nada: é o esperado que ele escolha o jogo pela figura |

### Rollout

| Fase | Entrega | Critério de saída |
| --- | --- | --- |
| **F1 — Salto** | FSM completa sem `VOLTA`, 1 cenário (`montanha`), arte provisória, sem áudio, sem placar | **Concluída** (implementada junto com a F2) |
| **F2 — Voz e HUD** | Arte final dos 3 cenários, áudio, estrelas, festa, placar, botão de voltar, attract mode, `M`/`F`, rachaduras, tremor | **Concluída em 2026-11-03** — smoke test jsdom: zero erros, pico de 136 nós, festa e placar funcionando |
| **F3 — v1.0** | Arquivo único final, regressão NF1–NF12, sessão com o Murilo | **Concluída e homologada em 2026-11-03** — técnicos oficiais (NF3/NF6/NF12) e K1/K2 confirmados pelo operador |

### Definição de pronto

A v1.0 está pronta quando: um arquivo `.html` abre por duplo-clique em notebook offline, o Murilo aponta e um socorro acontece em menos de 2,8 s, três socorros produzem uma festa, o amigo esteve visível o tempo todo, o estrondo não assusta (tremor dentro do limite), o adulto muta com uma tecla e volta à vitrine, o placar mostra os amigos já salvos, **NF1–NF12 estão verificados**, e nenhuma das dez leis do `TEMA.md` foi violada.

**Situação:** F1, F2 e F3 concluídas; a v1.0 foi homologada pelo operador. A v1.1 (amigos por rodada) também foi homologada.

**Reversão / descarte:** o jogo é um arquivo novo em pasta própria. Remover `murilo/hulk/` e o cartão da vitrine restaura o estado anterior sem tocar na Teia.

---

## 6. Rastreabilidade

| Requisito | Origem |
| --- | --- |
| Zero derrota, zero cronômetro | Teia `docs/adr/0001-sem-derrota-sem-cronometro.md` (herdado) |
| Mouse como varinha, aim assist 100% | Teia `docs/adr/0002-mouse-como-varinha.md` (herdado) |
| Um alvo por vez | `TEMA.md` L4 |
| Sem travesso; obstáculo é a causa | `docs/adr/0001-sem-travesso-obstaculo-e-a-causa.md` |
| Sempre Hulk, sem Bruce | `docs/adr/0002-sempre-hulk-sem-bruce.md` |
| Herói fica no cenário | `docs/adr/0003-heroi-fica-no-cenario.md` |
| Placar desde a v1.0 | `docs/adr/0004-placar-desde-a-v1.md` + Teia `ADR 0004` |
| Zero texto (salvo placar) | `TEMA.md` L3 |
| Vocabulário (amigo, obstáculo, salto, rompimento, estrondo, rachadura) | `GLOSSARY.md` |
| Timings do roteiro | `TEMA.md` § *O roteiro* |
| Protocolo de teste | `TEMA.md` § *Como saber se funcionou* |
| Botão de voltar à vitrine | Teia `docs/adr/0005-botao-voltar-vitrine.md` (herdado) |
| Placar 1,5× no celular | Teia v1.4.2 (lição herdada) |
| Estrondo sem rugido | `TEMA.md` L9 + Q10 do design |

---

## 7. Tarefas de documentação (quando implementar)

- Registrar no `README.md` o status da v1.0 quando ela for implementada. **Feito.**
- Adicionar o cartão no array `JOGOS` da vitrine (`index.html`) **só quando o arquivo existir**.
- Se a v1.0 homologar, atualizar o **Status** deste PRD e o cabeçalho do `TEMA.md`.
- Regressão técnica em `murilo/hulk/docs/regressao-v1.0.md`, mesmos métodos da Teia.
