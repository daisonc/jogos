# Força do Murilo — Tema

- **Versão**: 0.4 (v1.0 implementada em 2026-11-03; v1.1 — amigos alternam por rodada, com objetos da Teia; tela cheia no celular no primeiro toque; pendente K1/K2)
- **Jogador**: Murilo, 3 anos
- **Plataforma**: navegador de notebook — mouse e teclado obrigatórios
- **Celular (cortesia)**: o arquivo também abre no navegador do celular; lá, o **primeiro toque** entra em tela cheia (sem duplo-toque). O alvo jogável continua sendo notebook com mouse. Onde a API não existe (iPhone), o gesto é inócuo (`ADR 0005`).
- **Voltar à vitrine**: botão fixo e colorido no canto superior esquerdo, sem texto, só uma seta — mesma solução da Teia (`../index.html` da Teia virou `../../index.html` aqui, porque este jogo vive um nível abaixo).
- **Arquivo**: `murilo/hulk/forca-do-murilo.html`, arquivo único
- **Herói**: Incrível Hulk, identidade oficial, referência do desenho clássico — **sempre Hulk, sem Bruce** (`ADR 0002`)
- **Travesso**: nenhum na v1.0 — o Obstáculo é a causa (`ADR 0001`)

---

## A frase

> Murilo aponta, o Hulk pula, e o obstáculo se parte até o amigo aparecer.

Se qualquer decisão futura contradizer essa frase, a decisão está errada.

## A missão

> Libertar o amigo que ficou preso. Sempre um amigo por vez. Sem pressa. Sem inimigo.

A missão não é destruir, não é vencer, não é proteger de algo que chega. É **abrir o que prende**. O prazer é ver o amigo sair — o estrondo é só a prova de que a força funcionou.

## A fantasia

Não é "um jogo do Hulk". É a experiência de **ter a força que socorre**.

Aos 3 anos a criança não consegue levantar nada pesado, não move pedra, não abre caminho. O que ela tem é um dedo. O jogo pega esse dedo e dá a ele o corpo do Hulk.

O prazer não está em quebrar por quebrar, e sim em **causar o aparecimento do amigo**. O rompimento é o meio; o amigo saindo é o fim.

**Corolário:** o mouse não é punho, é varinha. Murilo não soca, não segura botão, não carrega fúria. Ele **aponta para onde a força vai agir**.

## Quem está jogando

Toda a doutrina abaixo deriva destes fatos, não de preferência estética. São os mesmos fatos da Teia — mesmo jogador, mesmo notebook, mesmo momento da vida:

| Fato | Consequência |
| --- | --- |
| 3 anos: motricidade fina incompleta | Não haverá mira. Aim assist de 100%. |
| Pré-leitor | Zero texto, salvo os dígitos do placar (exceção nominal, `ADR 0004`). |
| Atenção de 2 a 5 minutos | Prazer nos primeiros 5 segundos, sem tutorial. |
| Perder não ensina | Zero derrota, zero cronômetro, zero som de erro. |
| Repetição é conforto | O mesmo roteiro pode rodar 20 vezes. |
| Causa e efeito é o prazer central | Todo clique em alvo tem consequência imediata e exagerada: o chão treme, a pedra se parte, o amigo aparece. |
| Medo não tem rollback | Um susto e ele não quer mais. Sem vilão, sem rugido, sem cara de raiva, sem tremor de cinema. |
| Ele já conhece a Teia | Já sabe o verbo (apontar) e já conhece o placar. Este jogo não pode ser um passo atrás em generosidade. |

## As leis

Não são recomendações. São as dez leis da Teia, relidas para este jogo. Qualquer uma quebrada descaracteriza o jogo.

**L1 — Nenhuma derrota.** Não existe game over, vida, tentativa, "quase!", nem tela de falha. Não existe estado em que o Murilo perdeu. Clique no vazio não faz nada.

**L2 — Nenhum prazo.** Nenhum cronômetro, nenhum obstáculo que expira, nenhum "rápido!". O jogo espera o tempo que ele precisar.

**L3 — Zero texto.** Nem título, nem instrução, nem "você conseguiu". *(Exceção nominal desde a v1.0: os dígitos do placar — `ADR 0004`. Nenhum outro número ou letra entra.)*

**L4 — Um alvo por vez.** Um amigo preso, um obstáculo. A tela nunca pede escolha.

**L5 — O mouse é varinha.** Aim assist de 100%, as mesmas duas camadas da Teia (círculo de 220 px + terço central), mirando no **obstáculo** — que é o que está grande na tela. Clique no vazio não faz nada: sem som, sem tremor, sem cara de erro.

**L6 — Um clique, um salto.** O salto tem começo, meio e fim. Cliques durante o roteiro são ignorados, não enfileirados.

**L7 — Nada de ameaça que chegue perto.** Hoje não há travesso. Se um dia houver, vale a lei integral da Teia: sempre de longe, sempre de costas ou de perfil, nunca de frente, nunca se aproxima. E o Hulk não é ameaça: cara de esforço, nunca de raiva; sem rugido; sem close na fúria; sobrancelha nunca fechada de monstro.

**L8 — Nada de números.** Progresso é visível (estrelas que acendem, amigos que ficam). *(Exceção nominal da v1.0: o placar — `ADR 0004`.)*

**L9 — Nenhum pico de som.** O estrondo é **visual** (poeira, tremor curto, pedra se partindo). O som é thud seco de 80–120 ms, band-limited, pico ≤ −6 dBFS. Nenhum rugido. Nenhum "grrr". Nada grave e longo.

**L10 — Ele decide o ritmo.** Fechar o jogo no meio não é fracasso. O jogo não cobra retorno.

## O roteiro

```
esperando ──clique no obstáculo──▶ agachar ──▶ salto ──▶ impacto ──▶ rompimento ──▶ gratidão
     ▲                                                                                │
     └────────── próximo amigo (outro lado, 0,4 s) ◀──────────────────────────────────┘
                    │
                    └── 3 amigos ──▶ festa ──▶ novo cenário ──▶ esperando
```

Não é o loop da Teia. O herói **não volta** ao ponto de partida (`ADR 0003`): ele fica onde pousou e o próximo obstáculo nasce do outro lado. A travessia é sempre um salto novo.

| Etapa | Duração | O que o Murilo vê | O que ele ouve |
| --- | --- | --- | --- |
| Esperando | indefinida | Hulk agachado, respirando; amigo mexendo devagar; obstáculo com halo sutil | Ambiente suave |
| Agachar | 0,25 s | Hulk desce mais; poeirinha nos pés | Sopro curto |
| Salto | 1,0 s | Arco alto; calça roxa contra o céu; easing de sobe-e-desce, nunca linear | `whoosh` curto, não grave |
| Impacto | 0,25 s | Pouso ao lado do obstáculo; tremor de 2–3 px por 150 ms; poeira sobe | `thud` seco (80–120 ms) |
| Rompimento | 0,5 s | Obstáculo parte em dois; metades caem **para fora da tela**; amigo levanta | Estalo seco + som do amigo |
| Gratidão | 0,6 s | Amigo pula; Hulk inclina a cabeça | Duas notas |
| Festa (a cada 3) | 3 s | Confete, estrelas acendendo, Hulk pulando | Jingle de 8 notas |

**Uma ação completa dura ~2,6 s.** O resto do tempo é o Murilo reagindo — e reagir é jogar.

**O amigo está sempre à vista.** Preso = imóvel com cabeça e pata para fora, mexendo devagar. Nunca escondido debaixo do obstáculo. O clique mira no conjunto amigo+obstáculo.

**A marca que fica.** As metades do obstáculo saem voando para fora da tela. No chão fica uma **rachadura** (um `<path>` barato), até a festa. Um cenário nunca passa de três rachaduras + um obstáculo ativo. A cena lembra o que ele fez sem estourar o orçamento de nós.

**Determinismo.** O lado do próximo obstáculo é contador (`jogo.lado`), nunca `Math.random`. A rachadura é geometria, não sorteio. Nenhum `if (cenario === …)` no comportamento.

## Placar

Desde a v1.0 (`ADR 0004`). Igual ao da Teia, porque o Murilo já conhece:

- Canto superior direito; uma linha por amigo **já salvo**: desenho à esquerda, número à direita.
- Só amigos já salvos; nada de ícone com zero.
- Cumulativo na sessão; não zera na festa nem na troca de cenário.
- A linha nasce no primeiro salvamento e não se move.
- No celular, ampliado 1,5× (lição da v1.4.2 da Teia, aqui desde o primeiro dia).
- Nós do placar são estáticos: NF4 nasce em **240**, não em 150.

## Elenco

**Herói — Hulk.** Pele verde chapada, calça roxa, contorno escuro, sem sombreamento. Reconhecível pela silhueta em 200 ms, a 1 metro, sem detalhe nenhum. Três poses apenas: **agachado**, **no ar**, **pousado/rompendo**. Grande na tela, mas nunca cobre o amigo nem o obstáculo ativo. Cara de esforço, nunca de raiva.

**Amigos.** Uma lista por cenário. A **rodada** (visita ao cenário) escolhe um: a 1ª visita mostra a base (bicho do mato); as seguintes alternam com os **objetos da Teia** (gatinho, pipa, balão, patinho, cachorrinho, bola) — os mesmos desenhos e sons do jogo do Aranha. Sempre visíveis, presos, mexendo devagar. Cada um com som próprio no rompimento e reação de gratidão própria. Determinístico (`visitas` do cenário), sem sorteio. Dentro de uma rodada, o amigo é sempre o mesmo (os três socorros daquela visita).

| Cenário | Base (1ª rodada) | 2ª rodada | 3ª rodada |
| --- | --- | --- | --- |
| Montanha | Cabritinho (`balido`) | Gatinho (`miau`) | Pipa (sopro de papel) |
| Floresta | Cervinho (`cervo`) | Balão (`ploc`) | Patinho (`patinho`) |
| Campo seco | Coelhinho (`fungado`) | Cachorrinho (`latido`) | Bola (`bola`) |

**Obstáculos.** A causa do resgate. Não são inimigos, não são armadilhas de ninguém. Um tipo por cenário, como dado do cenário:

| Cenário | Obstáculo | Como prende |
| --- | --- | --- |
| Montanha | Pedregulho | amigo espremido na lateral |
| Floresta | Tronco caído | amigo com o rabo/pata para fora |
| Campo seco | Rocha rachada | amigo atrás, cabeça para fora |

**Sem travesso.** Nenhum Duende, nenhum Abutre, nenhum vilão do Hulk (Abominável e Líder estão fora por susto). O obstáculo já é a causa. Se um dia o Murilo pedir "mais alguém fugindo", aí se abre a porta — com L7 integral.

## Cenários

Três, nesta ordem. Cada um reconhecível em 1 segundo, sem texto. Natureza, não cidade, não água.

1. **Montanha** — céu azul claro, pedras cinza, pinheiros escuros. Abertura porque é onde o salto tem mais graça: altura e pedra.
2. **Floresta** — verde, raios de sol, tronco caído. O resgate mais óbvio: amigo atrás do tronco.
3. **Campo seco** — capim dourado, céu claro, rocha rachada. O fecho. Rio, praia e cidade ficam de fora: água é caro de desenhar (lição da Teia) e cidade põe o Hulk quebrando prédio, que é imagem difícil de defender para 3 anos.

## Áudio

- 100% sintetizado em Web Audio, sem arquivo de áudio. Mesmo orçamento da Teia: master −12 dBFS, limitador, pico ≤ −6 dBFS.
- `thud` = impacto seco, passa-baixa, 80–120 ms. A força está no **visual** do estrondo (poeira + tremor), não no volume.
- `whoosh` do salto: curto, não grave.
- Sem rugido. Sem "grrr". Sem narração. Sem música com tensão.
- `M` muta, `F` tela cheia — iguais à Teia.
- Contexto destravado no primeiro `pointerdown`.

## O que este jogo não tem

Sem vilão. Sem Bruce Banner. Sem transformação. Sem rugido. Sem fúria. Sem combate. Sem cidade. Sem água. Sem derrota. Sem tempo. Sem menu. Sem texto — só os dígitos do placar. Sem inversão de lado (a tela não espelha; quem muda de lado é o próximo obstáculo, por contador). Sem quinto cenário.

Cada ausência desta lista é uma decisão registrada, não um esquecimento.

## Como saber se funcionou

Mesmo protocolo da Teia: 5 minutos, uma sessão, os sinais observados. O teste com o Murilo é o que decide — tema de jogo infantil que não foi testado com a criança é ficção.

| Sinal | Leitura |
| --- | --- |
| Onde ele clica primeiro | No obstáculo/amigo = entendi o verbo. No centro da tela = aumentar aim assist. |
| Ele espera o salto ou clica repetido? | Repetido e frenético = o salto está longo demais. |
| Ri do estrondo ou se assusta com o tremor? | Assusto = tirar o tremor, deixar só a poeira. |
| Acha o amigo na tela? | Não acha = amigo mais visível, halo mais forte. |
| Pede "de novo" quando a festa acaba? | O único sinal que importa de verdade. |
| Levanta da cadeira antes de 2 minutos? | O cenário está errado, não o jogo. |

K1 (rir ao menos uma vez) e K2 (pedir "de novo") continuam sendo os critérios observacionais. K3/K4/K5 (primeiro frame ≤ 2 s, p95 ≤ 16,7 ms, zero rede/erro) herdam da Teia.

## Lições herdadas da Teia (já nascem neste jogo)

A Teia levou várias versões para aprender isto. Aqui não é para refazer o caminho:

- **Placar desde o primeiro dia**, com ampliação de 1,5× no celular.
- **Botão de voltar à vitrine** desde o primeiro dia (HTML, fora do SVG, não soma nós).
- **Attract mode** após 8 s de ociosidade.
- **`M` e `F`** do adulto.
- **Duplo-toque** para tela cheia no celular.
- **NF4 = 240 nós** (placar já entra na conta).
- **Delta do loop com clamp de 100 ms.**
- **Zero rede, zero persistência, zero dependência.**
- **Só `transform` e `opacity` animam.**
- **DPR ≤ 2.**

## Mapa de decisões

| Decisão | Escolha | Registro |
| --- | --- | --- |
| Plataforma | Notebook, navegador, mouse + teclado | este documento |
| Verbo | Apontar; salto e rompimento automáticos | este documento |
| Missão | Libertar o amigo preso | este documento |
| Falha | Nenhuma | herda a Teia `ADR 0001` |
| Entrada | Mouse como varinha, aim assist 100% | herda a Teia `ADR 0002` |
| Identidade | Sempre Hulk, sem Bruce | `ADR 0002` deste jogo |
| Travesso | Nenhum; o Obstáculo é a causa | `ADR 0001` deste jogo |
| Herói no cenário | Fica onde pousa; não volta | `ADR 0003` deste jogo |
| Placar | Desde a v1.0, igual ao da Teia | `ADR 0004` deste jogo + `PADRAES.md` |
| Tela cheia no celular | Primeiro toque entra; sem duplo-toque | `docs/adr/0005-tela-cheia-no-primeiro-toque.md` + `PADRAES.md` |
| Estilo | Vetor chapado, SVG inline | herda a Teia |
| Áudio | Thud + whoosh; sem rugido | este documento |
| Progressão | Rodadas de 3 amigos + festa | herda a Teia |
| Estrutura | `murilo/hulk/`, irmã da Teia | este documento |

## Pendências

Não são dúvidas de design — são o que vem depois:

- ~~Escrever o PRD~~ — **feito** ([`PRD.md`](./PRD.md), 2026-11-03).
- ~~Implementar `forca-do-murilo.html`~~ — **feito** (v1.0, 2026-11-03).
- ~~Adicionar o cartão na vitrine~~ — **feito**.
- Testar com o Murilo (K1/K2) antes de chamar de pronto.
