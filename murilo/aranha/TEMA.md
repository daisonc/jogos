# Aranha do Murilo — Tema

- **Versão**: 1.0 (implementado em 2026-11-03)
- **Jogador**: Murilo, 3 anos
- **Plataforma**: navegador de notebook — mouse e teclado obrigatórios
- **Celular (cortesia)**: o arquivo também abre no navegador do celular; lá, o **primeiro toque** entra em tela cheia (padrão da casa `../../PADRAES.md`). O alvo jogável continua sendo notebook com mouse.
- **Voltar à vitrine**: botão fixo e colorido no canto superior esquerdo, sem texto, só uma seta — `../../index.html`.
- **Arquivo**: `murilo/aranha/aranha-do-murilo.html`, arquivo único
- **Herói**: Homem-Aranha, identidade oficial do desenho clássico — **sempre Homem-Aranha, sem Peter Parker, sem troca de roupa** (`ADR 0002`)
- **Travesso**: nenhum — os brinquedos ficaram espalhados pelo vento (`ADR 0001`)

---

## A frase

> Murilo aponta, o Aranha atira a teia, e o brinquedo vem parar na caixa.

Se qualquer decisão futura contradizer essa frase, a decisão está errada.

## A missão

> **Guardar todo brinquedo que ficou para lá.** Sempre um por vez. Sem pressa. Sem inimigo.

## A fantasia

É o **irmão gêmeo do Raio do Murilo**. Mesmo verbo (apontar), mesma missão (guardar o brinquedo na caixa), mesmos cenários e mesmos brinquedos. Muda o herói e muda o alcance.

No Raio, o Super Homem **voa** e o que alcança é o olhar. Aqui, o Homem-Aranha **fica no chão** — na altura do Murilo — e o que alcança é a **teia**. A criança continua sendo o dedo; o herói continua sendo o braço; o que muda é de onde o braço sai.

**Corolário:** o mouse continua sendo varinha. O Aranha não é mira de precisão; ele **aponta e atira**.

## Relação com os irmãos

Este jogo existe porque o Murilo pediu: "o mesmo do Super Homem, mas com o Homem-Aranha". Então ele **repete** o Raio do Murilo de propósito — mesmos três cenários, mesmos nove brinquedos, mesma FSM, mesma caixa, mesmo placar.

Ele **não** é uma segunda versão do `Teia do Murilo`. Lá o Homem-Aranha balança e resgata; aqui ele fica de pé e recolhe brinquedos com a teia. Nomes e vocabulários não se misturam (ver `../../GLOSSARY-MAP.md`):

| | Teia do Murilo | Aranha do Murilo |
| --- | --- | --- |
| Herói | Homem-Aranha | Homem-Aranha |
| O que ele faz | Balança na teia e resgata | Fica no chão e atira a teia no brinquedo |
| Alvo | Resgatável | Brinquedo |
| Nome do momento | Resgate | Coleta / Guarda |

## Quem está jogando

Os mesmos fatos da Teia, do Força, do Veloz e do Raio — mesmo jogador, mesmo notebook, mesmo momento:

| Fato | Consequência |
| --- | --- |
| 3 anos: motricidade fina incompleta | Não haverá mira. Aim assist de 100%. |
| Pré-leitor | Zero texto, salvo os dígitos do placar (`../../PADRAES.md`). |
| Atenção de 2 a 5 minutos | Prazer nos primeiros 5 segundos, sem tutorial. |
| Perder não ensina | Zero derrota, zero cronômetro, zero som de erro. |
| Repetição é conforto | O mesmo roteiro pode rodar 20 vezes. |
| Causa e efeito é o prazer central | Todo clique em alvo tem consequência imediata: a teia, o brinquedo voando, a tampa abrindo. |
| Medo não tem rollback | Sem vilão. Nada que chegue perto. |
| Ele já conhece os quatro irmãos | Já sabe o verbo (apontar) e o placar. Este jogo não pode ser um passo atrás em generosidade. |

## As leis

As dez leis da casa, relidas para este jogo.

**L1 — Nenhuma derrota.** Não existe game over, vida, tentativa, brinquedo que escapa, "quase!". Clique no vazio não faz nada.

**L2 — Nenhum prazo.** Nenhum cronômetro. A coleta acontece quando ele apontar.

**L3 — Zero texto.** *(Exceção nominal: os dígitos do placar — `../../PADRAES.md`.)* O peito leva a aranha preta, sem letra.

**L4 — Um alvo por vez.** Um brinquedo. A tela nunca pede escolha. A caixa não é alvo.

**L5 — O mouse é varinha.** Aim assist de 100% (círculo de 240 px + terço central).

**L6 — Um clique, uma teia.** A teia tem começo, meio e fim. Cliques durante o roteiro são ignorados.

**L7 — Nada de ameaça que chegue perto.** Hoje não há vilão. Se o Murilo um dia pedir, vale a lei integral da casa.

**L8 — Nada de números.** *(Exceção nominal: o placar.)* A caixa não mostra dígitos.

**L9 — Nenhum pico de som.** A teia é um estalo curto (`thwip`), não sirene. Nenhum estouro.

**L10 — Ele decide o ritmo.** Fechar o jogo no meio não é fracasso.

## O roteiro

```
esperando ──clique no brinquedo──▶ mirar ──▶ teia ──▶ coleta ──▶ guarda ──▶ festa
     ▲                                                                   │
     └────────── próximo brinquedo (mesmo lado, 0,4 s) ◀──────────────────┘
                    │
                    └── 3 brinquedos ──▶ festa (caixa pulsa dourada) ──▶ novo cenário
```

| Etapa | Duração | O que o Murilo vê | O que ele ouve |
| --- | --- | --- | --- |
| Esperando | indefinida | Homem-Aranha **de pé no chão**, respirando; brinquedo mexendo com halo | Ambiente suave |
| Mirar | 0,26 s | Ele se agacha e inclina para o brinquedo | Sopro curto |
| Teia | 0,38 s | A teia sai do punho e **estica até o brinquedo**; um nó de teia se cola nele | Estalo curto (`thwip`) |
| Coleta | 0,42 s | O brinquedo **vem puxado pela teia** até a caixa; a tampa abre | `pop` + som do brinquedo |
| Guarda | 0,35 s | Tampa fecha; a caixa brilha dourada | Duas notas |
| Festa (a cada 3) | 3 s | Confete, a caixa pulsando dourada, ele dá pulinhos | Jingle de 8 notas |

**Uma ação completa dura ~1,8 s** — igual ao Raio.

**O que torna este jogo um jogo (não uma skin do Raio):**

1. **Ele fica no chão** — o Super Homem voa; o Aranha fica de pé, na altura do Murilo.
2. **A teia que estica** — o raio é instantâneo e reto; a teia cresce do punho até o brinquedo e o nó se cola nele.
3. **O brinquedo que é puxado** — a teia puxa o brinquedo até o chão, onde a caixa espera.

**O brinquedo está sempre à vista.** Mexendo devagar, com halo.

**Determinismo.** O brinquedo da rodada é `amigos[jogo.guardados % amigos.length]` — os três do cenário saem em sequência, sem repetir. Nenhum `Math.random` em decisão de jogo; só em partículas (confete, poeira).

## Placar

Desde a v1.0 (`../../PADRAES.md`, `ADR 0004`). Igual aos irmãos: canto superior direito, desenho à esquerda, número à direita, cumulativo na sessão, só itens já guardados, 1,5× no celular. NF4 nasce em **240**. A caixa **não** mostra dígitos.

## Elenco

**Herói — Homem-Aranha.** Máscara vermelha com teias e olhos grandes brancos, tronco vermelho com a aranha preta no peito, braços e pernas azuis, botas e luvas vermelhas. O braço direito fica estendido com o lançador de teia no punho. Silhueta em 200 ms: máscara vermelha + teia. Cara de calma. Grande na tela, mas nunca cobre o brinquedo. **Sempre Homem-Aranha, sem Peter Parker** (`ADR 0002`).

**Brinquedos.** Os mesmos nove do Raio do Murilo, três por cenário, sempre na mesma ordem. O contador de guardados escolhe o da rodada (`guardados % 3`).

| Cenário | 1º guardado | 2º guardado | 3º guardado |
| --- | --- | --- | --- |
| Metropolis | Bola (`ploc`) | Ursinho (`munchido`) | Balão (`ploc`) |
| Campo | Pipa (sopro de papel) | Carrinho (`vrum`) | Patinho (`patinho`) |
| Pôr do sol | Gatinho (`miau`) | Bola de praia (`bola`) | Sorvete (sino curto) |

**Caixa.** De madeira, tampa que abre, **no chão ao lado do herói** (`ADR 0006`). O brinquedo é puxado pela teia até ela e a tampa fecha. Na festa, a caixa pulsa dourada. Não mostra dígitos.

**Sem vilão.** Os brinquedos ficaram espalhados pelo vento (`ADR 0001`).

## Cenários

Os **mesmos três** do Raio do Murilo, nesta ordem, com uma diferença: agora há chão.

1. **Metropolis** — prédios, céu azul; ganhou uma **rua** no rodapé, onde ele fica de pé.
2. **Campo** — verde, celeiro ao fundo, cerros.
3. **Pôr do sol / nuvens** — laranja e rosa, o fecho.

## Áudio

- 100% sintetizado em Web Audio. Master −12 dBFS, limitador, pico ≤ −6 dBFS.
- `teia` = estalo curto (`thwip`); `pop` na coleta; amigos = mesmos sons da Teia/Força/Veloz/Raio; jingle de 8 notas.
- Sem rugido, sem sirene, sem narração.
- `M` muta, `F` tela cheia no desktop, primeiro toque no celular.

## O que este jogo não tem

Sem Peter Parker. Sem troca de roupa. Sem combate. Sem vilão. Sem água. Sem loop jogável. Sem cronômetro. Sem brinquedo que escapa. Sem teia-balanço (isso é o Teia do Murilo). Sem derrota. Sem tempo. Sem menu. Sem texto — só os dígitos do placar e a aranha do peito. Sem quarto cenário. Sem dois jogadores.

## Como saber se funcionou

Mesmo protocolo: 5 minutos, uma sessão.

| Sinal | Leitura |
| --- | --- |
| Onde ele clica primeiro | No brinquedo = entendi. No centro = aumentar aim assist. |
| Espera a teia ou clica repetido? | Repetido = teia longa demais. |
| Acha a caixa? | Não acha = caixa maior ou mais perto do olhar. |
| Pede "de novo" quando a festa acaba? | O único sinal que importa. |
| Levanta da cadeira antes de 2 minutos? | O cenário está errado, não o jogo. |

K1 (rir) e K2 (pedir "de novo"). K3/K4/K5 herdam.

## Lições herdadas (já nascem)

- Placar desde o primeiro dia, 1,5× no celular.
- Botão de voltar à vitrine.
- Attract mode após 8 s.
- `M` e `F`.
- Tela cheia no celular no primeiro toque.
- NF4 = 240 nós.
- Delta do loop com clamp de 100 ms.
- Zero rede, zero persistência, zero dependência.
- Só `transform` e `opacity` animam.
- DPR ≤ 2.

## Mapa de decisões

| Decisão | Escolha | Registro |
| --- | --- | --- |
| Plataforma | Notebook, navegador, mouse + teclado | este documento |
| Verbo | Apontar; teia automática | este documento |
| Missão | Recolher brinquedo na caixa | este documento |
| Identidade | Sempre Homem-Aranha, sem Peter Parker | `ADR 0002` |
| Travesso | Nenhum | `ADR 0001` |
| Chão | Fica no chão; a teia viaja até o brinquedo | `ADR 0003` |
| Placar | Desde a v1.0 | `ADR 0004` + `../../PADRAES.md` |
| Tela cheia no celular | Primeiro toque | `ADR 0005` + `../../PADRAES.md` |
| Teia | Do punho; nó se cola no brinquedo | `ADR 0006` |
| Caixa | No chão, ao lado do herói | `ADR 0006` |
| Estrutura | `murilo/aranha/` | este documento |

## Pendências

- Testar com o Murilo (K1/K2) antes de chamar de pronto.
- Medições oficiais de NF3/NF6/NF12.
