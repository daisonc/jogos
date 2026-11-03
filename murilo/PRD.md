# PRD — Teia do Murilo

- **Versão do documento**: 1.7 (v1.0, v1.1, v1.2, v1.3 e v1.4 homologadas; v2.0 fora deste PRD, §5)
- **Escopo do produto**: v1.0 — três cenários, loop completo; v1.1 — variações de resgatável e segundo travesso; v1.2 — gratidão do resgatado e travesso trapalhão; v1.3 — quarto cenário (Praia); v1.4 — placar de resgates e inversão de lado (§8)
- **Fonte de verdade de design**: [`TEMA.md`](./TEMA.md), [`GLOSSARY.md`](./GLOSSARY.md), [`docs/adr/`](./docs/adr/)
- **Status**: v1.0 concluída e homologada em 2026-11-03 — F1–F3 implementados, K1/K2 observados, NF1–NF12 verificados e conferência visual aprovada pelo operador. **v1.1 concluída e homologada em 2026-11-03** (variações de resgatável + Abutre; §6): F4.1, F4.2 e F4.3 concluídas; NF1–NF12 verdes (regressão em `docs/regressao-v1.1.md`) e K1/K2 reconfirmados pelo operador. **v1.2 concluída e homologada em 2026-11-03** (gratidão do resgatado + travesso trapalhão; §7): F5.1, F5.2 e F5.3 concluídas; NF1–NF12 verdes (regressão em `docs/regressao-v1.2.md`), teto de 149/150 nós, e K1/K2 reconfirmados pelo operador. **v1.3 concluída e homologada em 2026-11-03** (quarto cenário — Praia; §7): F6.1 e F6.2 concluídas; NF1–NF12 verdes (regressão em `docs/regressao-v1.3.md`), Praia em 138/150 nós, e K1/K2 reconfirmados pelo operador. **v1.4 entregue em 2026-11-03** (placar de resgates + inversão de lado; §8) — implementada; NF1–NF12 verdes salvo o rebaseline de NF4 (pico de 240 nós, ver §8.4), K1/K2 pendentes de sessão observacional. Com a v1.4, o roadmap original deste PRD está completo; a v2.0 (`pedro/`) fica fora deste documento.

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
- Narração por voz, descartada em definitivo.
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
| **v1.1** | Variações de resgatável (pipa, patinho, sorvete) e Abutre como 2º travesso — ver §6 | **Concluída e homologada em 2026-11-03** — F4.1–F4.3 concluídas; K1/K2 reconfirmados e NF1–NF12 verdes |
| **v1.2** | Gratidão do resgatado e travesso trapalhão — ver §7 | **Concluída e homologada em 2026-11-03** — F5.1–F5.3 concluídas; NF1–NF12 verdes; K1/K2 reconfirmados pelo operador |
| **v1.3** | Quarto cenário (Praia) — ver §7 | **Concluída e homologada em 2026-11-03** — F6.1–F6.2 concluídas; NF1–NF12 verdes; Praia em 138/150 nós; K1/K2 reconfirmados. Emenda ao non-goal "novos cenários" registrada no `ADR 0003` |
| **v2.0** | `pedro/` — o jogo do irmão, com as próprias leis | Fora deste PRD |

### Definição de pronto

A v1.0 está pronta quando: um arquivo `.html` abre por duplo-clique em notebook offline, o Murilo clica e um resgate acontece em menos de 3,2 s, três resgates produzem uma festa, o Duende aparece e foge sem close, o adulto muta com uma tecla, **NF1–NF12 estão verificados**, e nenhuma das dez leis do `TEMA.md` foi violada.

**Situação em 2026-11-03:** todos os critérios acima foram atendidos e homologados pelo operador.

---

## 6. Planejamento da v1.1

> Emenda de planejamento. A v1.0 permanece congelada e homologada; esta seção descreve apenas o que muda na v1.1.

### 6.1 Escopo e decisões

A v1.1 adiciona **conteúdo**, não mecânica: três variações de resgatável e um segundo travesso. O loop, o aim assist, os tempos da FSM, as estrelas, a festa e os três cenários permanecem exatamente como estão.

| Decisão | Escolha | Consequência |
| --- | --- | --- |
| Formato | Emenda a este PRD | Uma fonte de verdade; a v1.0 continua sendo o contrato homologado |
| Variações | Alternância por resgate | Cada cenário mantém o resgatável base e ganha um alternativo; a base aparece primeiro em cada cenário |
| 2º travesso | Abutre | Segundo travesso aéreo, visto de longe, sem close |
| Entrada do Abutre | Alternância por rodada | Rodada 0 Duende, rodada 1 Abutre, rodada 2 Duende… |
| Narração | Descartada em definitivo | Fora de qualquer versão |

**Mapa de variações (aprovado em 2026-11-03):**

| Cenário | Base | Variante | Som da base | Som da variante |
| --- | --- | --- | --- | --- |
| `telhados` | Gatinho | Pipa | `miau` | sopro de papel + assobio curto |
| `parque` | Balão | Patinho de borracha | `ploc` | guincho de borracha |
| `rua` | Cachorrinho | Sorvete | latido curto | sino curto |

**Regra de alternância:** o objeto alterna **a cada resgate**, não a cada troca de cenário. Em cada cenário o 1º resgate mostra a base, o 2º a variante e o 3º volta à base. Como `jogo.resgates` zera na festa, todo cenário recomeça pela base. A alternância é determinística (`jogo.resgates % alvos.length`), nunca sorteada.

**Regra do Abutre:** a cada rodada (três resgates + festa + troca de cenário), o travesso alterna. O Abutre usa a mesma rota de fuga do Duende no cenário; mudam a silhueta e o som. Ele é causa, não obstáculo.

**Generalização de US4:** na v1.1, onde `US4` diz "Duende", vale "travesso da rodada" (Duende ou Abutre); todos os demais critérios de segurança permanecem.

**Aprovação de design:** mapa de variações, direção de arte e sons aprovados pelo operador em 2026-11-03.

**Critérios de sucesso:** K1 e K2 reconfirmados em sessão observacional; NF1–NF12 verdes; nenhuma lei do `TEMA.md` violada. Não há KPI novo — a v1.1 é conteúdo, não mudança de comportamento.

### 6.2 Histórias de usuário

#### US7 — Reconhecer um resgatável novo

*Como Murilo, quero ver um brinquedo diferente no mesmo lugar, para ter uma surpresa nova sem aprender uma regra nova.*

**Critérios de aceite**

- Exatamente um resgatável ativo por vez; a variação não muda o aim assist (círculo de 220 px + terço central), a FSM nem a duração de nenhuma etapa.
- O objeto alterna base → variante → base **a cada resgate** dentro da rodada; o primeiro resgate de cada cenário é sempre a base.
- A variante tem som próprio, distinto dos outros cinco resgatáveis e do `pop` genérico, tocado no resgate.
- A silhueta da variante é reconhecível em 200 ms, sem texto, sem ícone e sem UI nova.
- O halo dourado da base é reutilizado; nenhum gradiente novo é criado.
- A alternância é determinística e não usa `Math.random`; não há estado persistido.
- O cenário com a variante respeita o teto de 150 nós SVG (orçamento em §6.4).

#### US8 — Entender que o segundo travesso também é a causa

*Como Murilo, quero ver outro travesso fugindo com o brinquedo, para o resgate não ser sempre a mesma cena.*

**Critérios de aceite**

- O Abutre aparece em rodadas alternadas (rodada 0 Duende, rodada 1 Abutre, rodada 2 Duende…).
- Valem as mesmas regras de segurança do Duende (`US4`/`L7`): distância horizontal ≥ 60% da largura virtual (≥ 1152 px) na aparição, altura aparente ≤ 40%, nunca de frente (de costas, de perfil ou em silhueta), desaparece em ≤ 2 s, nunca se aproxima do herói ou do espectador.
- O Abutre tem som próprio: grasnido curto e leve + duas batidas de asa; nada grave, longo ou de susto (`L9`).
- Nenhum estado novo de FSM, nenhuma colisão, nenhum combate; o Abutre não bloqueia o resgate.
- A rota é dado do cenário, não `if` por cenário; o Abutre reutiliza a rota do Duende no cenário.
- O total de nós não aumenta: apenas um travesso é renderizado por vez (§6.4).

#### US9 — Não perder o que já funcionava

*Como adulto, quero que a v1.1 não quebre o que já foi homologado.*

**Critérios de aceite**

- K1 e K2 são reconfirmados em sessão observacional com o Murilo.
- NF1–NF12 continuam válidos; a regressão é medida com os mesmos métodos da v1.0.
- O loop, os tempos, o aim assist, as estrelas e a festa não mudam.
- O arquivo continua único, `file://`, sem build, sem rede e sem dependência.
- Se a criança reagir pior, o conteúdo novo pode ser removido sem tocar no loop (templates + contadores).

### 6.3 Non-goals da v1.1

- Novos cenários, novas mecânicas, novos estados de FSM, fases, dificuldade, chefes, combate, morte.
- Narração por voz (descartada em definitivo).
- Placar, pontuação, progresso salvo, texto, ícone novo ou UI nova.
- Mais de uma variante por cenário e mais de um travesso novo.
- Mudar o aim assist, os tempos do loop, a festa ou a estrutura de três estrelas.
- Dependência, biblioteca, build, rede, armazenamento.

### 6.4 Especificação técnica

**Modelo de dados**

`CENARIOS[i]` ganha `alvos`, uma lista ordenada de variantes:

```js
alvos: [
  { id: 'gatinho', tpl: 'tpl-alvo-telhados',      som: 'miau' },
  { id: 'pipa',    tpl: 'tpl-alvo-telhados-pipa', som: 'pipa' },
]
```

- `jogo.resgates` conta os resgates da rodada e zera em `terminarFesta()`. `montarAlvo()` (chamada por `nascerAlvo()`, a cada resgate) escolhe `v = jogo.resgates % alvos.length`, clona `alvos[v].tpl` e guarda `alvos[v].som` em `jogo.somAlvo` (usado por `Som.resgate`). O índice 0 (base) é sempre o primeiro resgate de cada cenário. O campo `som` de `CENARIOS[i]` é substituído por `alvos[].som`; `montarCenario()` monta só cenário e travesso.
- `TRAVESSOS = [{ id:'duende', tpl:'tpl-travesso-duende', som:'ihihi' }, { id:'abutre', tpl:'tpl-travesso-abutre', som:'abutre' }]`.
- `jogo.rodada` incrementa em `terminarFesta()`; `TRAVESSOS[jogo.rodada % 2]` escolhe o travesso.
- `#travesso` é substituído pelo clone do template (sem aninhar um `<g>` extra), mantendo o id e a referência em `el.travesso`. O Duende sai do markup fixo e vira template, como os alvos. Isso mantém **um** travesso renderizado por vez, com o mesmo custo de nós.
- Renomear `duende` → `travesso` no estado, no elemento e nas funções (`iniciarTravesso`, `desenharTravesso`); a rota continua em `CENARIOS[i]` (`rotaTravesso`), com os mesmos valores.

**Orçamento de nós**

| Cenário | Total atual | Resgatável atual | Teto da variante | Total com variante |
| --- | --- | --- | --- | --- |
| `telhados` | 147 | 17 | ≤ 20 | ≤ 150 |
| `parque` | 142 | 6 | ≤ 14 | ≤ 150 |
| `rua` | 148 | 16 | ≤ 18 | ≤ 150 |

Meta: manter o total atual (147/142/148). Teto absoluto: 150. O `rua` é o cenário crítico (2 nós de folga). O Abutre deve custar o mesmo que o Duende (≤ 12 elementos de arte), para não alterar o total.

**Direção de arte (aprovada em 2026-11-03):** pipa em losango azul-turquesa com rabiola amarela (contrasta com o pôr do sol); patinho amarelo de bico laranja (contrasta com o verde do parque); sorvete com casquinha waffle, bola rosa e cereja (contrasta com o azul da rua); Abutre de asas largas, visto de costas, sem rosto, em verde e roxo suaves. Todos com contorno escuro e silhueta legível, no estilo chapado do `TEMA.md`.

**Áudio**

- `Som.resgate(som)` ganha três ramos:
  - `pipa` — sopro de papel (ruído em band-pass, 120–180 ms) + assobio curto.
  - `patinho` — guincho de borracha (oscilador com variação de altura, 100–150 ms).
  - `sorvete` — sino curto (duas parciais senoidais, 100–150 ms).
- `Som.travesso(som)`: `duende` mantém o `ihihi`; `abutre` toca grasnido curto + duas batidas de asa.
- Todos passam pelo master de −12 dBFS e pelo limitador existentes; nenhum compressor novo.
- Aceite: pico ≤ −6 dBFS medido com `AnalyserNode`; sem clipping; sem som grave/longo.

**Render e performance**

- Só `transform` e `opacity` animam; nenhum `filter`, `mask` ou `stroke-dashoffset` animado.
- O custo por quadro não aumenta: a variante tem o mesmo porte da base e só um travesso é renderizado.
- Limites: ≤ 150 nós SVG, ≤ 80 animados/frame, p95 ≤ 16,7 ms.

**Determinismo e dados**

- Nenhum `Math.random`; alternância por contadores.
- Nenhum `if (cenario === ...)` no comportamento; variante e travesso vêm de dados.
- A arte nova vive em `<template>`, como a arte atual.

### 6.5 Riscos

| ID | Risco | Prob. | Impacto | Mitigação | Plano B |
| --- | --- | --- | --- | --- | --- |
| R7 | Variante estoura o teto de nós (rua tem 2 de folga) | Média | Médio | Teto por variante (§6.4); troca por template; medir após cada arte | Simplificar a arte ou remover um nó de cenário no `rua` |
| R8 | Abutre assusta e o Murilo abandona | Média | Alto | Silhueta de costas/perfil, sem rosto, longe, ≤ 2 s, grasnido leve; K1/K2 | Remover o Abutre e manter só as variações |
| R9 | Som novo estala ou assusta | Baixa | Médio | Curto, band-limited, medir pico; limitador | Baixar o ganho ou trocar por som mais suave |
| R10 | Alternância confunde | Baixa | Médio | Determinística, base primeiro, sem sorteio; observar | Fixar a variante por cenário ou alternar a cada 2 resgates |
| R11 | Escopo cresce | Alta | Médio | Non-goals; só 3 variantes + 1 travesso | Parar no escopo planejado |

### 6.6 Rollout

| Fase | Entrega | Critério de saída |
| --- | --- | --- |
| **F4.1 — Variações** | Templates da pipa, patinho e sorvete; sons; `alvos` + `jogo.resgates` | Cada variante aparece no resgate correto; total ≤ 150 nós; pico ≤ −6 dBFS; 0 erros |
| **F4.2 — Abutre** | Template do Abutre; som; `TRAVESSOS` + `jogo.rodada`; troca por template | Abutre alterna por rodada; L7/US4 respeitados; total ≤ 150; nenhum estado novo |
| **F4.3 — Regressão e observação** | Reexecutar NF1–NF12; sessão com o Murilo | **Concluída em 2026-11-03** — técnicos verdes; K1/K2 reconfirmados; reversível |

**Situação em 2026-11-03:** F4.3 concluída — NF1–NF12 verdes, NF4 no teto de 148/150 nós, alternância de resgatável e de travesso confirmada, reversível, e K1/K2 reconfirmados pelo operador. Evidências em `docs/regressao-v1.1.md`.

### 6.7 Definição de pronto da v1.1

A v1.1 está pronta quando: as três variações aparecem alternadas por resgate dentro de cada cenário (base primeiro), o Abutre alterna por rodada com o Duende, cada resgatável e cada travesso tem som próprio, NF1–NF12 continuam verdes, os tetos de 150 nós e 80 animados/frame são respeitados, K1/K2 são reconfirmados em sessão observacional, e nenhuma das dez leis do `TEMA.md` foi violada.

**Situação em 2026-11-03:** todos os critérios acima foram atendidos; a v1.1 foi homologada pelo operador.

**Reversão:** o conteúdo novo é aditivo. Remover os templates das variantes, o template do Abutre e os contadores restaura o comportamento da v1.0 sem tocar no loop.

### 6.8 Tarefas de documentação

- Atualizar `TEMA.md` § Elenco: o Abutre como segundo travesso, com as mesmas regras de L7.
- Atualizar `GLOSSARY.md`: verbete **Abutre** (travesso específico, sempre visto de longe) e, se necessário, **Variação**.
- Atualizar a tabela de resgatáveis do `TEMA.md` com as três variações e seus sons.
- Registrar no `README.md` o status da v1.1 quando ela for implementada.

---

## 7. Planejamento da v1.2 e da v1.3

> Emenda de planejamento. A v1.0, a v1.1 e a v1.2 permanecem homologadas; esta seção descreve o que as versões seguintes acrescentam. A v1.2 (M1+M2) foi homologada em 2026-11-03 e a **v1.3 (M3 — Praia) foi aprovada pelo operador e implementada em 2026-11-03** (disciplina do R6).

### 7.1 Escopo e decisões

A v1.2 adiciona **polimento** (reação do resgatado e uma trapalhada na fuga do travesso) sem nó novo e sem mudar o loop. A v1.3 adiciona **conteúdo** (um quarto cenário). Nenhuma das duas mexe no aim assist, na FSM, nos tempos, nas estrelas ou na festa.

| Decisão | Escolha | Consequência |
| --- | --- | --- |
| Formato | Emenda a este PRD | Uma fonte de verdade; v1.0 e v1.1 continuam como contratos |
| Faseamento | v1.2 = M1 + M2; v1.3 = M3 | A v1.2 é só animação e som; o risco de arte e de nós fica isolado na v1.3 |
| Gatilho | Após a F4.3 da v1.1 | A v1.1 permanece observável e reversível sozinha |
| Determinismo | Tudo por tabela de dados e contador | Nenhum `Math.random` em decisão de jogo (NF11) |

| Id | Melhoria | Versão | Natureza |
| --- | --- | --- | --- |
| M1 | Gratidão do resgatado | v1.2 | Animação + som, 0 nós |
| M2 | Travesso trapalhão | v1.2 | Animação + som, ≤ 1 nó |
| M3 | Quarto cenário: Praia | v1.3 | Arte + dados + sons |

**Critérios de sucesso:** K1 e K2 reconfirmados em sessão observacional a cada versão; NF1–NF12 verdes; nenhuma lei do `TEMA.md` violada. Não há KPI novo. A prancheta da sessão ganha uma anotação **qualitativa** (não é KPI): se o Murilo sorri ou ri no momento da reação (M1) e da trapalhada (M2).

### 7.2 Histórias de usuário

#### US10 — Receber um "obrigado" (M1, v1.2)

*Como Murilo, quero ver o bichinho feliz depois de salvo, para sentir que o que eu fiz deu certo.*

**Critérios de aceite**

- Em todo resgate, uma reação do resgatável e do herói começa no início de `VOLTA` e termina em **≤ 600 ms**, dentro da duração existente. Nenhuma etapa da FSM é alongada (a faixa de 2,3 s a 3,2 s de `US2` se mantém).
- Cada um dos seis resgatáveis tem reação própria, reconhecível sem texto (tabela em §7.4).
- Um som curto de gratidão (**≤ 150 ms**) toca no início da reação. Ele **não substitui** o som do resgate.
- Só `transform` e `opacity` animam, em nós existentes: **0 nós novos**.
- Cliques durante `VOLTA` continuam ignorados (`TEMA.md` L6). Aim assist, FSM e estrelas não mudam.
- Sem texto, ícone ou número. A reação é determinística, escolhida pelo índice do resgatável.

#### US11 — Rir da trapalhada do travesso (M2, v1.2)

*Como Murilo, quero ver o travesso se atrapalhar ao fugir, para achar graça nele em vez de ter medo.*

**Critérios de aceite**

- Durante a fuga já existente, cada travesso faz **uma** trapalhada: o Duende perde o chapéu, que cai girando; o Abutre dá uma cambalhota no ar (rotação no plano da tela). Ele continua fugindo e some em **≤ 2 s**, como em `US4`/`US8`.
- Valem todas as regras de segurança de `US4`/`US8`: distância horizontal ≥ 60% da largura virtual (≥ 1152 px), altura aparente ≤ 40%, nunca de frente, nunca se aproxima do herói ou do espectador. Durante o gag, a distância horizontal ao herói **não diminui**.
- Sem queda ao chão, sem impacto, sem dor e sem choro: a graça é o movimento, nunca o dano.
- Som próprio e leve: assobio descendente curto (**≤ 150 ms**), agudo-médio, nada grave, longo ou de susto (`TEMA.md` L9).
- Nenhum estado novo de FSM, nenhuma colisão, e o gag não bloqueia o resgate nem altera o tempo de nenhuma etapa.
- A trapalhada é dado do travesso (`TRAVESSOS[i].gag`), não `if` por travesso. Remover o campo restaura a fuga da v1.1.

#### US12 — Um lugar novo para salvar (M3, v1.3)

*Como Murilo, quero ir para um lugar novo depois da festa, para ter uma surpresa sem aprender uma regra nova.*

**Critérios de aceite**

- A Praia entra como o **quarto** cenário do ciclo: três resgates, festa e troca de cenário, como nos demais. Nada no loop muda.
- Resgatável base **tartaruguinha**, variante **bola de praia**, pela mesma regra de alternância da `US7` (base primeiro, `jogo.resgates % alvos.length`).
- Os sons são próprios e distintos dos seis atuais (propostos em §7.4, a aprovar).
- A rota de fuga dos travessos é dado do cenário e cumpre `US4`/`US8`.
- Total de nós da Praia **≤ 140** (folga ≥ 10 sobre o teto de 150), com ≤ 80 animados por frame.
- Mar calmo e sem perigo: sem ondas grandes, sem animais que possam assustar. Sem gradiente novo além do céu do cenário.
- Os três cenários anteriores não mudam.

### 7.3 Non-goals da v1.2 e da v1.3

- Quinto cenário, novas mecânicas, novos estados de FSM, fases, dificuldade, chefes, combate, morte.
- Qualquer interação nova durante `VOLTA` ou durante a fuga (clique ou tecla da criança).
- Gag aleatório, gag que machuque ou assuste, travesso novo ou segundo gag por travesso.
- Placar, pontuação, progresso salvo, texto, ícone ou UI nova, narração por voz.
- Dependência, biblioteca, build, rede, armazenamento.

### 7.4 Especificação técnica

**Emenda ao non-goal "novos cenários".** O §6.3 exclui novos cenários **da v1.1**. A v1.3 abre **uma exceção**, apenas para a Praia. Um quinto cenário continua fora de escopo.

**M1 — Gratidão (modelo de dados).** `alvos[]` ganha `reacao` (id de uma tabela `REACOES` com os valores de `transform` e a duração) e `tomGrato` (altura da 1ª nota). O som `grato` são duas notas senoidais ascendentes de 100 a 150 ms, com a altura vinda do dado. A reação roda a partir da entrada em `VOLTA`.

| Resgatável | Reação do objeto (`transform`) | Reação do herói |
| --- | --- | --- |
| Gatinho | espreguiça: escala vertical sobe e volta | pulinho + inclinação de aceno |
| Pipa | giro de 360° | pulinho + inclinação de aceno |
| Balão | balanço pendular de ±12° | pulinho + inclinação de aceno |
| Patinho | dois quiques curtos | pulinho + inclinação de aceno |
| Cachorrinho | abana: rotação rápida de ±10° | pulinho + inclinação de aceno |
| Sorvete | pulso de escala 1 → 1,08 → 1 | pulinho + inclinação de aceno |
| Tartaruguinha | cabeceia: balanço de ±18° | pulinho + inclinação de aceno |
| Bola de praia | três quiques | pulinho + inclinação de aceno |

O aceno do herói é feito por `transform` do grupo do herói, sem nó novo. O sorvete usa o mesmo som, com outro tom. (As duas últimas linhas são da v1.3.)

**M2 — Travesso trapalhão (modelo de dados).** `TRAVESSOS[i]` ganha `gag: { id, quando, som }`, onde `quando` é a fração da fuga em que a trapalhada começa. Exemplos: `{ id:'chapeu', quando:0.5, som:'uepa' }` e `{ id:'cambalhota', quando:0.5, som:'uepa' }`. O Duende precisa que o chapéu seja um nó separado: se hoje ele é parte do corpo, separá-lo custa **+1 nó** por cenário. O Abutre reutiliza o próprio grupo e não custa nó. O limite de 12 elementos de arte por travesso (§6.4) continua valendo.

| Cenário | Total na v1.1 | Teto com o chapéu | Total máximo |
| --- | --- | --- | --- |
| `telhados` | 147 | +1 | 148 |
| `parque` | 142 | +1 | 143 |
| `rua` | 148 | +1 | 149 |

**M3 — Praia.** `CENARIOS[]` ganha o quarto item, com paleta, silhuetas, posição do resgatável, rota do travesso e `alvos` próprios. A Praia exige seus próprios templates de arte. A proposta abaixo foi **aprovada pelo operador** em 2026-11-03:

| id | Nome | Céu | Base | Variante | Som da base | Som da variante |
| --- | --- | --- | --- | --- | --- | --- |
| `praia` | Praia | `#7EC8E3 → #FFF1C1` | Tartaruguinha | Bola de praia | trinado suave de duas notas | quique de borracha leve |

**Consequência da paridade (decisão).** Com três cenários e alternância por rodada (`TRAVESSOS[jogo.rodada % 2]`), cada cenário recebe um travesso diferente a cada volta. Com **quatro** cenários (número par), cada cenário passaria a ter **sempre o mesmo** travesso (`telhados` e `rua` com o Duende, `parque` e `praia` com o Abutre). Para manter a variedade, a escolha passa a ser `TRAVESSOS[(jogo.rodada + floor(jogo.rodada / CENARIOS.length)) % 2]`. Isso continua determinístico, sem sorteio e sem `if` por cenário.

**Áudio.** `Som.gratidao(tom)`, `Som.trapalhada(som)` e dois ramos novos em `Som.resgate` para a Praia. Todos passam pelo master de −12 dBFS e pelo limitador existentes; nenhum compressor novo. Aceite: pico ≤ −6 dBFS medido com `AnalyserNode`, sem clipping, nada grave ou longo.

**Render e performance.** Só `transform` e `opacity` animam; nenhum `filter`, `mask` ou `stroke-dashoffset` animado. O M1 e o M2 não adicionam nós animados por frame além dos já existentes na cena, e o M3 mantém ≤ 80. p95 ≤ 16,7 ms.

### 7.5 Riscos

| ID | Risco | Prob. | Impacto | Mitigação | Plano B |
| --- | --- | --- | --- | --- | --- |
| R12 | A reação de gratidão "engasga" ou parece um defeito (é cortada pelo próximo alvo) | Média | Médio | Janela de ≤ 600 ms dentro de `VOLTA`, easing, medir os tempos da FSM | Reduzir a amplitude ou encurtar a reação |
| R13 | A trapalhada assusta, ou o Murilo a entende como machucado | Baixa | Alto | Sem queda, impacto ou dor; movimento no ar; assobio leve; observar K1/K2 | Remover o campo `gag` e voltar à fuga da v1.1 |
| R14 | Separar o chapéu estoura o teto de nós | Baixa | Médio | Teto de +1 por cenário (§7.4); medir após a arte | Animar o travesso inteiro, sem separar o chapéu |
| R15 | A paridade par de cenários fixa um travesso por cenário | Média | Baixo | Fórmula com `floor(rodada / CENARIOS.length)` (§7.4) | Aceitar a fixação, que é previsível |
| R16 | A Praia estoura o escopo ou o orçamento de nós | Média | Médio | Teto de 140 nós; só 1 cenário novo; sem quinto cenário | Não fazer a v1.3 |

### 7.6 Rollout

| Fase | Entrega | Critério de saída |
| --- | --- | --- |
| **F5.1 — Gratidão** | `reacao`, `REACOES`, `Som.gratidao` | Reação em todos os seis resgatáveis; ≤ 600 ms; 0 nós novos; pico ≤ −6 dBFS |
| **F5.2 — Trapalhão** | `gag` em `TRAVESSOS`, `Som.trapalhada`, chapéu separado | Gag em ambos os travessos; US4/US8 respeitadas; totais ≤ 149 nós |
| **F5.3 — Regressão e observação (v1.2)** | Reexecutar NF1–NF12; sessão com o Murilo | Técnicos verdes; K1/K2 reconfirmados; reversível |
| **F6.1 — Praia** | Cenário, templates, sons, fórmula de paridade | Total ≤ 140 nós; alternância correta; US4/US8 respeitadas |
| **F6.2 — Regressão e observação (v1.3)** | Reexecutar NF1–NF12; sessão com o Murilo | Técnicos verdes; K1/K2 reconfirmados; reversível |

**Situação em 2026-11-03:** F5.1–F5.3 concluídas — reação de gratidão nos seis resgatáveis (≤ 600 ms, 0 nós novos), trapalhada nos dois travessos e chapéu do Duende como nó separado. NF1–NF12 verdes; teto de nós em 149/150 (evidências em `docs/regressao-v1.2.md`). K1/K2 e as medições oficiais (NF3/NF6/NF12) reconfirmadas pelo operador. **v1.2 homologada.**

**Situação em 2026-11-03:** F6.1–F6.2 concluídas — a Praia entra como quarto cenário (céu `#7EC8E3 → #FFF1C1`, tartaruguinha + bola de praia, sons próprios, resgatáveis repousando na areia e herói pousando com os pés na areia), total em **138/150 nós**, alternância de travesso por cenário confirmada (fórmula com `floor(rodada / CENARIOS.length)`), US4/US8 respeitadas. NF1–NF12 verdes (evidências em `docs/regressao-v1.3.md`), K1/K2 e medições oficiais reconfirmadas pelo operador. **v1.3 homologada.**

### 7.7 Definição de pronto

**v1.2:** os seis resgatáveis reagem em ≤ 600 ms dentro de `VOLTA`, ambos os travessos fazem a trapalhada sem violar `US4`/`US8`, NF1–NF12 continuam verdes, K1/K2 são reconfirmados e nenhuma das dez leis do `TEMA.md` foi violada.

**Situação em 2026-11-03:** todos os critérios acima foram atendidos; a v1.2 foi homologada pelo operador.

**v1.3:** a Praia entra como quarto cenário com resgatável base e variante, o travesso alterna sem fixar por cenário, a Praia fica em ≤ 140 nós, NF1–NF12 continuam verdes, K1/K2 são reconfirmados e nenhuma lei foi violada.

**Situação em 2026-11-03:** todos os critérios acima foram atendidos; a v1.3 foi homologada pelo operador. O roadmap deste PRD está completo.

**Reversão:** o conteúdo é aditivo. Remover `reacao`, `gag` e o item da Praia restaura a v1.1 sem tocar no loop.

### 7.8 Tarefas de documentação

- Atualizar `TEMA.md` § *O loop* (reação de gratidão em `VOLTA`), § *Elenco* (trapalhada dos travessos) e § *Cenários* (Praia).
- Atualizar `GLOSSARY.md`: verbetes **Gratidão** e **Trapalhada**, e a Praia na lista de cenários.
- Registrar no `README.md` o status da v1.2 e da v1.3 quando forem implementadas.
- Se a v1.3 for aprovada, registrar a exceção ao non-goal "novos cenários" em `docs/adr/`.

---

## 8. Planejamento da v1.4

> Emenda de planejamento. A v1.0–v1.3 permanecem homologadas; esta seção descreve o que a v1.4 acrescenta: **placar de resgates** (feature 1) e **inversão de lado a cada salvamento** (feature 2), ambas pedidas pelo operador.

### 8.1 Escopo e decisões

A v1.4 adiciona **um HUD de leitura sem leitura** (o placar) e **variedade de movimento** (a inversão de lado). Nenhuma das duas mexe no aim assist, na FSM, nos tempos, nas estrelas ou na festa.

| Decisão | Escolha | Consequência |
| --- | --- | --- |
| Formato | Emenda a este PRD | v1.0–v1.3 continuam como contratos |
| Placar com dígitos | Sim, sobrepõe L8 e L3 | Exceção **nominal** registrada no `ADR 0004`; nenhum outro número entra |
| Escopo do placar | Cumulativo na sessão | Não zera na festa nem na troca de cenário |
| Itens do placar | Só os já salvos | Sem ícone com zero; a linha nasce no primeiro salvamento |
| Inversão de lado | A cada resgate, contínua | Atravessa festa e troca de cenário; espelha o cenário inteiro |
| Tela cheia no toque | Duplo-toque (cortesia) | Exceção mínima ao non-goal "toque": é só o atalho de tela cheia; a jogabilidade segue mouse + teclado |

**Emenda ao non-goal "Placar, recorde, pontuação".** O §2 exclui placar. A v1.4 abre **uma exceção nominal**, apenas para o placar de resgates descrito nesta seção: nada de recorde, ranking, pontuação por tempo, dificuldade ou progresso salvo. A emenda está registrada no `ADR 0004`.

### 8.2 Histórias de usuário

#### US13 — Ver o que eu já salvei (feature 1)

*Como Murilo, quero ver o desenho de cada bicho/brinquedo que eu salvei e quantos eu já salvei, para sentir a coleção crescer sem precisar ler.*

**Critérios de aceite**

- Placar no **canto superior direito** (coordenadas virtuais), sem cobrir o herói nem o resgatável ativo.
- Uma linha por resgatável **já salvo**: o desenho do item à esquerda e o número de salvamentos à direita.
- Só aparecem itens já salvos; nenhum ícone com zero.
- A contagem é **cumulativa na sessão** — não zera na festa nem na troca de cenário.
- A linha nasce no primeiro salvamento do item e **não se reposiciona** depois (o placar só cresce para baixo).
- Determinístico e sem `Math.random`; nenhum estado persistido.
- Só `transform` e `opacity` animam; o placar é **estático** (não soma animados/frame).
- É a **única** exceção a L3 e L8; nenhum outro texto, letra ou número aparece no jogo.

#### US14 — O lado muda a cada resgate (feature 2)

*Como Murilo, quero que o herói e o objeto troquem de lado a cada salvamento, para o balanço não ser sempre na mesma direção.*

**Critérios de aceite**

- A cada resgate, o lado em que o herói começa **inverte**: herói à esquerda/objeto à direita ⇄ objeto à esquerda/herói à direita, indefinidamente.
- A alternância é por **contador** (`jogo.lado`), determinística e **contínua** — atravessa a festa e a troca de cenário.
- O cenário é espelhado em torno de `x = W/2`, **inclusive a rota do travesso**; na aparição, `|herói − travesso| ≥ 1152 px` (≥ 60% da largura), preservando `US4`/`US8` e `L7` em todos os cenários e nos dois lados.
- Nenhum estado novo de FSM; o loop, o aim assist, os tempos, as estrelas e a festa não mudam.
- A `VOLTA` pousa no **lado novo** (o herói já virou para lá no resgate), sem salto de posição entre o fim de uma etapa e o início da seguinte.
- Nenhum `Math.random`; a inversão é determinística.

#### US15 — Abrir em tela cheia no celular (cortesia)

*Como adulto, quero colocar em tela cheia sem teclado, para aproveitar o jogo quando ele for aberto no celular.*

**Critérios de aceite**

- A tecla `F` continua valendo no desktop, inalterada.
- No toque, **dois toques rápidos** (≤ 320 ms) em área vazia alternam a tela cheia via `requestFullscreen`; um toque no alvo continua sendo resgate (o segundo toque chega com a FSM fora de `ESPERANDO` e não conta).
- Um toque único **não** alterna; o mouse é ignorado pelo gesto.
- As metatags de instalação (`mobile-web-app-capable`, `apple-mobile-web-app-capable`, `apple-mobile-web-app-status-bar-style`) permitem que "Adicionar à Tela de Início" abra sem a barra de endereço.
- Onde a API de tela cheia não existe (ex.: iPhone), o gesto é inócuo — nada quebra e nenhum diálogo aparece.

### 8.3 Non-goals da v1.4

- Placar de pontuação, recorde, ranking, tempo, dificuldade ou progresso salvo — o placar é **só** de contagem de resgates por item.
- Texto, letra ou qualquer outro número fora do placar.
- Terceira feature, novo cenário, nova mecânica, novo estado de FSM, novos resgatáveis ou travessos.
- Mudar o aim assist, os tempos do loop, a festa ou a estrutura de três estrelas.
- Toque, celular e tablet como alvo jogável continuam fora de escopo; a única exceção é o **duplo-toque para tela cheia** (`US15`), sem mecânica de toque nova.
- Dependência, biblioteca, build, rede, armazenamento.

### 8.4 Especificação técnica

**Placar.** Constantes em `PLACAR`; `<g id="placar">` no topo (acima do cenário, abaixo do flash e do aviso de mudo). Estado `placarContagem` (id → nº) e `placarLinhas` (id → nó de texto). Em `entrar(S.RESGATE)`, `registrarResgate()` lê `jogo.alvoId`/`jogo.alvoTpl` (gravados em `montarAlvo()`), incrementa o contador do item e, no primeiro salvamento, clona a arte do template do resgatável numa linha nova (sem o halo dourado). O dígito é um `<text>` com fonte de sistema; a linha nasce em `y` fixo e o fundo (`<rect>`) cresce com o número de itens. Reaproveita a arte real de cada item — é o desenho que o Murilo reconhece.

**Inversão de lado.** Estado `jogo.lado` (0/1). `espelharLayout(c)` devolve o cenário com `heroi`, `ancorAlvo`, `centroAlvo.x`, `cordaAlvo.x` e `rotaTravesso` espelhados em `x = W/2`; `layoutAtual()` escolhe entre o cenário base e o espelhado. `posAlvo`, `iniciarTravesso`, `iniciarTeia` e `desenhar` passam a ler `layoutAtual()`. Em `entrar(S.RESGATE)`, `jogo.lado ^= 1` (a virada acontece depois do balanço, para a volta já pousar no lado novo). A `VOLTA` deixa de retornar ao ponto de partida e passa a pousar em `layoutAtual().heroi`, com controle de arco próprio.

**Orçamento de nós (NF4) — rebaseline.** O placar soma nós (a arte real de cada item). Pico medido com oito itens salvos: **240 nós** (antes 149), durante o ciclo completo dos quatro cenários. Os nós do placar são **estáticos**: não entram no limite de ≤ 80 animados/frame nem no frame time (NF3). O teto de NF4 passa a **240** para a v1.4 (amendment no `ADR 0004`); a alternativa de ícones pictográficos próprios foi descartada por risco de descaracterizar o item.

**Determinismo e dados.** Nenhum `Math.random` na contagem nem na inversão; ambos são contadores. Nenhum `if (cenario === …)`; o espelhamento é geométrico e a contagem é por id. Sem persistência.

**Áudio, seguranças e loop.** Inalterados: nenhum som novo, nenhum nó animado a mais, aim assist (círculo de 220 px + terço central), tempos da FSM, estrelas e festa intactos.

### 8.5 Riscos

| ID | Risco | Prob. | Impacto | Mitigação | Plano B |
| --- | --- | --- | --- | --- | --- |
| R17 | O placar polui a tela ou cobre o travesso | Média | Baixo | Canto superior direito, fundo translúcido, só itens salvos; observar K1/K2 | Encolher os ícones ou mover o placar para uma faixa no topo |
| R18 | O número confunde e some a leitura do desenho | Baixa | Médio | Desenho à esquerda, dígito à direita, alto contraste; item reconhecível em 200 ms | Reduzir a fonte do dígito ou destacar mais o desenho |
| R19 | NF4 estoura de vez com oito itens | Média | Baixo | Rebaseline para 240, nós estáticos; medir a cada item | Ícones pictográficos compactos por item |
| R20 | A inversão de lado confunde o Murilo | Baixa | Médio | Determinística, contínua, mesma cena espelhada; observar | Fixar o lado ou inverter a cada rodada |
| R21 | Espelhar a rota do travesso aproxima-o do herói | Baixa | Alto | Rota espelhada junto; `|herói − travesso| = 1170–1220 px` nos dois lados | Não espelhar o lado do travesso, só o do herói/objeto |
| R22 | Escopo cresce para "placar de tudo" | Alta | Médio | Non-goals; exceção nominal no `ADR 0004` | Parar no placar de resgates |

### 8.6 Rollout

| Fase | Entrega | Critério de saída |
| --- | --- | --- |
| **F7.1 — Placar** | `PLACAR`, `#placar`, `registrarResgate`, `jogo.alvoId/alvoTpl` | Uma linha por item salvo; número correto; cumulativo; 0 erros |
| **F7.2 — Inversão de lado** | `jogo.lado`, `espelharLayout`/`layoutAtual`, VOLTA no lado novo | Lado alterna a cada resgate; L7/US4 respeitadas nos dois lados |
| **F7.3 — Regressão e observação** | Reexecutar NF1–NF12 (NF4 rebaselinado); sessão com o Murilo | Técnicos verdes (ressalva NF4); K1/K2 reconfirmados; reversível |

**Situação em 2026-11-03:** F7.1 e F7.2 implementadas e verificadas (sessão dirigida em Node/jsdom, ciclo completo dos quatro cenários, 14 resgates, 8 itens no placar, zero erros de console, alternância de lado confirmada; pico de nós 240). **F7.3 pendente** da medição oficial de NF3/NF6/NF12 e da sessão observacional (K1/K2) pelo operador.

### 8.7 Definição de pronto

A v1.4 está pronta quando: o placar mostra, por resgatável já salvo, o desenho e o número de salvamentos, cumulativo na sessão; o lado em que o herói começa inverte a cada resgate, continuamente, com o travesso sempre a ≥ 60% de distância nos dois lados; NF1–NF12 seguem verdes (com o rebaseline de NF4 para 240 nós); K1/K2 são reconfirmados em sessão observacional; e nenhuma outra lei do `TEMA.md` foi violada além da exceção nominal registrada.

**Reversão:** o conteúdo é aditivo. Remover o bloco do placar e a inversão de lado (`jogo.lado`, `espelharLayout`/`layoutAtual`) restaura a v1.3 sem tocar no loop.

### 8.8 Tarefas de documentação

- Registrar a exceção a L3/L8 e ao non-goal "sem placar" e o rebaseline de NF4 em `docs/adr/0004-placar-e-inversao-de-lado.md`. **Feito.**
- Atualizar `TEMA.md` § *As leis* (L3/L8), § *O que este jogo não tem* e § *Placar e lado*. **Feito.**
- Atualizar `GLOSSARY.md`: verbetes **Placar** e **Inversão de lado**. **Feito.**
- Registrar no `README.md` o status da v1.4. **Feito.**
- Registrar a regressão técnica em `docs/regressao-v1.4.md`. **Feito.**

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
| Variações de resgatável (pipa, patinho, sorvete) | `TEMA.md` § *Elenco* + §6 deste PRD |
| Segundo travesso (Abutre) | `TEMA.md` § *Elenco* + §6 deste PRD |
| Gratidão do resgatado (M1) | `TEMA.md` § *O loop* + §7 deste PRD |
| Travesso trapalhão (M2) | `TEMA.md` L7, L9 + §7 deste PRD |
| Quarto cenário, Praia (M3) | `TEMA.md` § *Cenários* + §7 deste PRD |
| Placar de resgates (feature 1) | `TEMA.md` § *Placar e lado* + `ADR 0004` + §8 deste PRD |
| Inversão de lado (feature 2) | `TEMA.md` § *Placar e lado* + `ADR 0004` + §8 deste PRD |
| Exceção nominal a L3/L8 | `ADR 0004` |
