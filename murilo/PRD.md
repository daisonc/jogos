# PRD — Teia do Murilo

- **Versão do documento**: 1.1
- **Escopo do produto**: v1.0 — três cenários, loop completo
- **Fonte de verdade de design**: [`TEMA.md`](./TEMA.md), [`GLOSSARY.md`](./GLOSSARY.md), [`docs/adr/`](./docs/adr/)
- **Status**: v1.0 concluída e homologada em 2026-11-03 — F1–F3 implementados, K1/K2 observados, NF1–NF12 verificados e conferência visual aprovada pelo operador

> Este documento **não repete** a doutrina de design. Ele a referencia. `TEMA.md` responde *por que o jogo é assim*; este PRD responde *o que sai e como se mede*. Se os dois divergirem, o `TEMA.md` ganha na intenção e este PRD ganha na entrega até a divergência ser resolvida.

---

## 1. Sumário executivo

### Problema

O Murilo tem 3 anos, joga em notebook com mouse, e não consegue mirar, ler, nem perder. Qualquer jogo convencional falha em pelo menos um desses três eixos — e um jogo que exige precisão de mouse produz, para ele, uma sequência de fracassos silenciosos que ele não sabe nomear, só abandonar.

### Solução

Um jogo de navegador em arquivo único onde o mouse é varinha, não volante: ele aponta, a teia gruda, o Homem-Aranha balança e um resgate acontece. Zero texto, zero derrota, zero cronômetro, três cenários, e um Duende que rouba de longe e foge.

### Critérios de sucesso

Dois observacionais (sessão real com o Murilo, protocolo em `TEMA.md` § *Como saber se funcionou*) e três técnicos (medição direta no notebook).

| # | Tipo | Critério | Como medir |
| --- | --- | --- | --- |
| K1 | Observacional | O Murilo ri ao menos uma vez por sessão | Observação, prancheta de 5 min |
| K2 | Observacional | Ao fim da festa, pede "de novo" ou volta a clicar sem ser convidado | Observação, prancheta de 5 min |
| K3 | Técnico | Duplo-clique no `.html` renderiza o primeiro frame em ≤ 2 s, offline | Cronômetro manual, 3 execuções |
| K4 | Técnico | Frame time p95 ≤ 16,7 ms (60 fps) durante balanço e festa | DevTools › Performance, 60 s de gravação |
| K5 | Técnico | Zero requisições de rede e zero erros no console em 5 min de sessão | DevTools › Network + Console |

**Fora dos KPIs, por decisão:** tempo de sessão com alvo numérico. "5 minutos" para uma criança de 3 anos é uma métrica que parece rigorosa e não é. K1 e K2 medem a coisa certa.

---

## 2. Experiência e funcionalidade

### Personas

**Murilo — jogador primário (3 anos).** Pré-leitor. Motricidade fina incompleta: não mira, não arrasta, não combina botões. Atenção de 2 a 5 minutos. Aprende por repetição e por causa-efeito, não por instrução. Não sabe nomear por que parou de gostar de algo.

**Adulto — operador (pai).** Abre o arquivo, regula o volume, tira o filho do jogo quando a hora acabou. Não quer instalar nada e não quer ler manual. É a interface de configuração do jogo, porque o jogo não tem configuração.

### Histórias de usuário

#### US1 — Apontar e causar

*Como Murilo, quero apontar para um bichinho preso e ver uma teia sair, para sentir que fui eu que causei aquilo.*

**Critérios de aceite**

- Exatamente **um** resgatável ativo por vez, sempre visível, sempre em movimento sutil de flutuação.
- Um clique válido dispara a teia em ≤ 100 ms.
- Aim assist em duas camadas, em coordenadas virtuais 1920×1080:
  - **(a)** círculo de acerto com raio de **220 px** ao redor do resgatável (≈ 11,5% da largura);
  - **(b)** se houver exatamente um resgatável ativo, qualquer clique dentro do **terço central** (30%–70% da largura, 25%–75% da altura) também conta.
- Clique fora de (a) e (b) não produz **nada**: sem som, sem animação, sem mudança de estado, sem cursor de erro.
- Nenhum texto nem tutorial é exibido em momento algum. Ícones só existem como feedback de tecla do adulto (`US5`), nunca durante o jogo em si.

#### US2 — O resgate acontece sozinho

*Como Murilo, quero que o herói balance até o bichinho e o traga de volta, para ver o resgate sem precisar fazer mais nada.*

**Critérios de aceite**

- Após o clique válido, a sequência `teia → balanço → resgate → volta` completa em **2,3 s a 3,2 s** sem nenhuma entrada adicional.
- Cliques durante a sequência são **ignorados**, não enfileirados (`TEMA.md` L6; `ADR 0002`).
- Ao final, o resgatável é removido e o próximo entra após 0,4 s.
- O movimento do herói é um arco com aceleração e desaceleração (easing), nunca linear.

#### US3 — Progresso visível, sem números

*Como Murilo, quero ver uma festa a cada três resgates, para saber que avancei sem saber ler.*

**Critérios de aceite**

- Três estrelas na base da tela; uma acende por resgate.
- No terceiro resgate, a festa começa automaticamente: confete, jingle de 8 notas, herói pulando.
- A festa dura **3,0 s ± 0,2 s** e transiciona para o próximo cenário sem menu, sem tela intermediária e sem texto.
- Ao reiniciar o ciclo, as três estrelas apagam.
- Nenhum número, dígito ou contador é exibido em nenhum estado (`TEMA.md` L8).

#### US4 — Entender por que está resgatando

*Como Murilo, quero ver o Duende fugindo com o balão, para entender de onde vem o problema.*

**Critérios de aceite**

- O Duende aparece no início de cada ciclo de resgate, no planador, **fugindo** do herói.
- Distância horizontal mínima do herói: **60% da largura virtual** no momento da aparição.
- Altura máxima aparente: **40% da altura da tela** — nunca há close, nunca há zoom.
- O rosto **nunca** é exibido de frente: sempre de costas, de perfil ou em silhueta (`TEMA.md` L7).
- Ele desaparece em ≤ 2 s, sem nunca se aproximar do herói ou do espectador.
- Trilha sonora de fuga: risada `ihihi` sintetizada, aguda e curta, nunca grave nem prolongada (`TEMA.md` L9).

#### US5 — Abrir e calar, sem manual

*Como adulto, quero abrir o jogo com dois cliques e mutar com uma tecla, para não precisar de instruções.*

**Critérios de aceite**

- Um único arquivo `.html`, aberto por duplo-clique (`file://`), sem servidor, sem instalação, sem internet.
- A tecla `M` alterna mudo em ≤ 100 ms, com feedback visual discreto de 1 s (ícone de som riscado no canto).
- A tecla `F` alterna tela cheia.
- Nenhum `alert`, `confirm`, prompt de permissão ou diálogo do navegador em nenhum fluxo.

#### US6 — Não quebra, não pede, não trava

*Como adulto, quero poder deixar ele jogando sozinho, para não virar eu o controle do jogo.*

**Critérios de aceite**

- Nenhuma requisição de rede durante toda a execução.
- Trocar de aba e voltar não faz a animação saltar: o delta do loop é **limitado a 100 ms**.
- Após 8 s sem nenhuma entrada, o herói executa **um** resgate de demonstração automático (attract mode), que ensina o gesto a quem está olhando. Qualquer clique cancela a demonstração imediatamente.
- A demonstração não altera a contagem de estrelas do jogador além do resgate que ela própria realiza.

### Non-goals

Explicitamente **fora** do escopo da v1.0:

- Toque, celular e tablet. O alvo é notebook com mouse — requisito declarado.
- Controle por teclado para a criança. O teclado é do adulto (`ADR 0002`).
- Placar, recorde, pontuação, ranking, progresso salvo.
- Dificuldade ajustável, fases, chefes, combate, morte, game over.
- Dois jogadores, modo cooperativo formal, o jogo do Pedro.
- Publicação, distribuição, monetização, publicação em loja.
- Internacionalização. O jogo não tem texto, então não há o que traduzir.
- Narração por voz (fica para v1.1, condicionada ao teste).
- Analytics, telemetria, qualquer coleta de dado.

---

## 3. Requisitos de sistema de IA

**Não aplicável.** Não há modelo, inferência, agente, RAG, avaliação de qualidade de saída nem custo por token. O único "modelo" é uma máquina de estados determinística de 6 estados.

---

## 4. Especificação técnica

### Visão de arquitetura

Arquivo único, sem build, sem dependência. Cinco módulos com responsabilidade única, comunicando por chamadas diretas — **sem ECS, sem framework, sem barramento de eventos** (`game-engine` › *Avoid Premature Abstraction*: o jogo tem ~12 elementos; ECS aqui é cerimônia sem retorno).

```
index.html  (único arquivo)
├── <svg id="palco">        camadas: céu · prédios · duende · herói · resgatável · estrelas · partículas
├── Loop (rAF)              delta com clamp de 100 ms → update → render
├── Estado (FSM)            ESPERANDO → TEIA → BALANCO → RESGATE → VOLTA → (FESTA) → ESPERANDO
├── Entrada                 pointerdown → coords virtuais → resolveAlvo() → transição
├── Cenários (dados)        array de 3 configs: paleta, prédios, resgatável, rota do Duende
└── Áudio                   Web Audio sintetizado; destrava no primeiro pointerdown
```

**Renderização: SVG no DOM, não Canvas.** Justificativa: a arte é vetor chapado (`TEMA.md` § *Cenários*) e a contagem de nós é baixa; SVG entrega antialiasing, hit-testing trivial e nenhum pipeline de textura. Só `transform` e `opacity` são animados, o que mantém a composição na GPU. **Gatilho de troca:** se K4 falhar de forma persistente, o render vira Canvas 2D mantendo a mesma FSM — a arquitetura isola a decisão.

### Máquina de estados

| Estado | Duração | Entrada | Saída |
| --- | --- | --- | --- |
| `ESPERANDO` | indefinida | — | clique válido, ou 8 s de ociosidade |
| `TEIA` | 250 ms | clique válido | automática |
| `BALANCO` | 1500 ms ± 300 ms | — | automática |
| `RESGATE` | 500 ms | — | automática |
| `VOLTA` | 600 ms | — | automática |
| `FESTA` | 3000 ms | 3º resgate | automática, troca de cenário |

Cliques só são lidos em `ESPERANDO`. Em qualquer outro estado, são descartados na entrada — não há fila.

### Sistema de coordenadas

Mundo virtual fixo de **1920×1080**, escalado por `min(largura/1920, altura/1080)` e centralizado com letterbox preto. Toda regra de acerto, distância e tamanho é expressa em unidades virtuais, e a entrada converte a posição do ponteiro pelo inverso da transformação. Consequência: o aim assist não muda de generosidade entre telas diferentes.

### Dados dos cenários

Data-driven (`game-engine` › *Data-Driven Design*): os três cenários são configuração, não código.

| id | Nome | Céu | Resgatável | Som do resgatável |
| --- | --- | --- | --- | --- |
| `telhados` | Telhados ao pôr do sol | `#6B4E8F → #F08A5D → #FFD08A` | Gatinho | `miau` agudo |
| `parque` | Parque | `#87CEEB → #C9E9F6` | Balão | `ploc` de borracha |
| `rua` | Rua do bairro | `#A8DADC` | Cachorrinho | latido curto |

*(Céu: cores do topo para a base da tela; uma cor = céu uniforme.)*

Cada cenário define: paleta, silhuetas de prédios/árvores/casas, posição do resgatável, e a rota de fuga do Duende. Nenhum comportamento é hard-coded por cenário.

### Áudio

- **100% sintetizado em Web Audio**, sem arquivo de áudio. É o que torna "arquivo único + offline" possível.
- `whoosh` = ruído branco filtrado com envelope; `thwip` = transiente curto em passa-alta; `pop` = seno curto; `ihihi` = três blips quadrados ascendentes; jingle = arpejo maior de 8 notas.
- Contexto destravado no primeiro `pointerdown` (política de autoplay); se `suspended`, `resume()`.
- Nível e limitador: ver **NF6** (`TEMA.md` L9).
- `M` zera e restaura o master gain instantaneamente.

### Requisitos não-funcionais

Fonte única dos limites verificáveis. As seções acima explicam o *porquê*; esta tabela é o que se confere antes de dizer que está pronto, e o que se cita em code review.

| # | Requisito | Limite | Verificação |
| --- | --- | --- | --- |
| NF1 | Arquivo único, `file://`, sem build | 1 `.html`, zero instalação | abrir por duplo-clique em máquina limpa |
| NF2 | Tempo até o primeiro frame | ≤ 2 s | cronômetro, 3 execuções (K3) |
| NF3 | Frame time durante balanço e festa | p95 ≤ 16,7 ms | DevTools › Performance, 60 s (K4) |
| NF4 | Carga de render | ≤ 150 nós SVG; ≤ 80 animados/frame; só `transform` e `opacity` | auditoria de código + Elements |
| NF5 | `devicePixelRatio` | ≤ 2 | auditoria de código |
| NF6 | Nível de áudio | master −12 dBFS com limitador; nenhum pico > −6 dBFS | `AnalyserNode` + audição |
| NF7 | Delta do loop | clamp em 100 ms | alt-tab e retorno, sem salto visual |
| NF8 | Rede em runtime | zero requisições | DevTools › Network (K5) |
| NF9 | Persistência | zero `localStorage`/`sessionStorage`/cookie/IndexedDB | DevTools › Application + auditoria |
| NF10 | Código e mídia de terceiros | zero dependência, script remoto, fonte ou imagem externa | auditoria do arquivo |
| NF11 | Determinismo | nenhuma aleatoriedade em decisão de jogo; sorteio só em partículas | auditoria de código |
| NF12 | Navegadores suportados | Chrome, Edge e Firefox, 2 versões mais recentes, Windows | execução manual nos três |

### Integration points

**Nenhum, e isso é requisito** (**NF8**, **NF10**). Sem backend, sem CDN, sem fonte externa (stack de fontes do sistema), sem imagem externa, sem analytics. O arquivo é autossuficiente por construção — o que também zera risco de supply chain.

### Segurança e privacidade

**Política.** O jogo não fala com o mundo (**NF8**), não lembra de nada (**NF9**) e não carrega código de terceiros (**NF10**). Três consequências práticas: funciona em modo avião; fechou, esqueceu — inclusive o mudo; e não há superfície de supply chain a auditar.

**Dado pessoal: nenhum.** Não há campo, identificador, conta ou upload. Não há tratamento de dados sob a LGPD porque não há dado a tratar — e essa é a razão de a ausência de persistência ser requisito, não conveniência.

Vetor de risco residual: nenhum conhecido além da integridade do próprio arquivo.

---

## 5. Riscos e roadmap

### Riscos

| # | Risco | Prob. | Impacto | Mitigação | Plano B |
| --- | --- | --- | --- | --- | --- |
| R1 | **O Duende assusta** e o Murilo abandona o jogo sem saber explicar | Média | Alto | `L7`: sempre de longe, de costas, sem close, risada aguda curta; altura máxima 40% da tela | Remover o personagem e deixar só o planador vazio + mãos verdes; o resgate continua funcionando sem antagonista |
| R2 | Ele não descobre que precisa clicar | Média | Alto | Aim assist de 100%; attract mode após 8 s de ociosidade | Reduzir a ociosidade para 3 s e aumentar a zona generosa para a metade central |
| R3 | Autoplay bloqueia o áudio e a sessão fica silenciosa | Média | Baixo | Destravar no primeiro `pointerdown`; `resume()` se suspenso | Ícone piscando pedindo um clique; nunca bloquear o jogo por áudio |
| R4 | Notebook antigo não sustenta 60 fps | Baixa | Médio | Só `transform`/`opacity`; DPR limitado a 2; ≤ 80 nós animados | Reduzir confete de 60 para 24 partículas; se persistir, migrar render para Canvas 2D |
| R5 | Ele fecha a janela ou sai da tela cheia com o mouse | Média | Baixo | `F` para tela cheia; área de jogo cobre 100% da viewport | Aceitar: é responsabilidade do adulto, não do jogo |
| R6 | Escopo cresce para "mais um cenário", "mais um vilão" | Alta | Médio | Non-goals explícitos; v1.1 só depois da v1.0 homologada (feito em 2026-11-03) | Nada. Este risco não tem mitigação técnica, só disciplina |

### Rollout

| Fase | Entrega | Critério de saída |
| --- | --- | --- |
| **F1 — Loop** | FSM completa, 1 cenário (`telhados`), arte provisória, sem áudio | Clicar produz balanço e resgate; K3 e K4 verdes |
| **F2 — Voz do jogo** | Arte final dos 3 cenários, Duende, áudio sintetizado, estrelas, festa | K5 verde; duração da festa e timings dentro da tolerância |
| **F3 — v1.0** | Arquivo único final, attract mode, `M`/`F` | **Concluída e homologada em 2026-11-03** — K1/K2, NF12 e conferência visual confirmados pelo operador |
| **v1.1** | Narração opcional, variações de resgatável (pipa, patinho, sorvete), 2º travesso | Liberada (K1 e K2 confirmados em 2026-11-03) |
| **v2.0** | `pedro/` — o jogo do irmão, com as próprias leis | Fora deste PRD |

### Definição de pronto

A v1.0 está pronta quando: um arquivo `.html` abre por duplo-clique em notebook offline, o Murilo clica e um resgate acontece em menos de 3,2 s, três resgates produzem uma festa, o Duende aparece e foge sem close, o adulto muta com uma tecla, **NF1–NF12 estão verificados**, e nenhuma das dez leis do `TEMA.md` foi violada.

**Situação em 2026-11-03:** todos os critérios acima foram atendidos e homologados pelo operador.

---

## Rastreabilidade

| Requisito | Origem |
| --- | --- |
| Zero derrota, zero cronômetro | `docs/adr/0001-sem-derrota-sem-cronometro.md` |
| Mouse como varinha, aim assist 100% | `docs/adr/0002-mouse-como-varinha.md` |
| Um alvo por vez | `TEMA.md` L4 + consequência do `ADR 0002` |
| Duende sempre de longe | `TEMA.md` L7 |
| Zero texto | `TEMA.md` L3 |
| Vocabulário (rodada, resgate, resgatável, festa) | `GLOSSARY.md` |
| Timings do loop | `TEMA.md` § *O loop* |
| Protocolo de teste | `TEMA.md` § *Como saber se funcionou* |
