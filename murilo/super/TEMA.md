# Raio do Murilo — Tema

- **Versão**: 1.0 (implementado em 2026-11-03; falta a sessão K1/K2)
- **Jogador**: Murilo, 3 anos
- **Plataforma**: navegador de notebook — mouse e teclado obrigatórios
- **Celular (cortesia)**: o arquivo também abre no navegador do celular; lá, o **primeiro toque** entra em tela cheia (padrão da casa `../PADRAES.md`). O alvo jogável continua sendo notebook com mouse.
- **Voltar à vitrine**: botão fixo e colorido no canto superior esquerdo, sem texto, só uma seta — `../../index.html`.
- **Arquivo**: `murilo/super/raio-do-murilo.html`, arquivo único
- **Herói**: Super Homem, identidade oficial do desenho clássico — **sempre Super Homem, sem Clark, sem criptonita** (`ADR 0002`)
- **Travesso**: nenhum — os brinquedos ficaram espalhados pelo vento (`ADR 0001`)

---

## A frase

> Murilo aponta, o Super Homem olha, e o raio traz o brinquedo até a caixa.

Se qualquer decisão futura contradizer essa frase, a decisão está errada.

## A missão

> **Guardar todo brinquedo que ficou para lá.** Sempre um por vez. Sem pressa. Sem inimigo.

A missão é recolher por olhar. O prazer é o raio; o brinquedo entrando na caixa é a prova de que funcionou. Anéis não existem aqui. A caixa é a prova encarnada.

## A fantasia

Não é "um jogo do Super Homem". É a experiência de **ter o olhar que alcança e guarda**.

Aos 3 anos, a criança não alcança o brinquedo que caiu do outro lado do quarto. O que ela tem é um dedo. O jogo pega esse dedo e dá a ele os olhos do Super Homem.

O prazer não está em voar para buscar, e sim em **apontar e o raio trazer o brinquedo**.

**Corolário:** o mouse não é mira de sniper, é varinha. Murilo não segura o raio, não recarrega, não mira fino. Ele **aponta para o brinquedo**.

## Quem está jogando

Os mesmos fatos da Teia, do Força e do Veloz — mesmo jogador, mesmo notebook, mesmo momento:

| Fato | Consequência |
| --- | --- |
| 3 anos: motricidade fina incompleta | Não haverá mira. Aim assist de 100%. |
| Pré-leitor | Zero texto, salvo os dígitos do placar (`../PADRAES.md`). |
| Atenção de 2 a 5 minutos | Prazer nos primeiros 5 segundos, sem tutorial. |
| Perder não ensina | Zero derrota, zero cronômetro, zero som de erro. |
| Repetição é conforto | O mesmo roteiro pode rodar 20 vezes. |
| Causa e efeito é o prazer central | Todo clique em alvo tem consequência imediata: o raio, o brinquedo voando, a tampa abrindo. |
| Medo não tem rollback | Sem vilão. Nada que chegue perto. |
| Ele já conhece a Teia, o Força e o Veloz | Já sabe o verbo (apontar) e o placar. Este jogo não pode ser um passo atrás em generosidade. |

## As leis

As dez leis da casa, relidas para este jogo.

**L1 — Nenhuma derrota.** Não existe game over, vida, tentativa, brinquedo que escapa, "quase!". Clique no vazio não faz nada.

**L2 — Nenhum prazo.** Nenhum cronômetro. A coleta acontece quando ele apontar.

**L3 — Zero texto.** *(Exceção nominal: os dígitos do placar — `../PADRAES.md`.)* O logo do peito é um losango, sem letra.

**L4 — Um alvo por vez.** Um brinquedo. A tela nunca pede escolha. A caixa não é alvo.

**L5 — O mouse é varinha.** Aim assist de 100% (círculo de 220 px + terço central), mirando no brinquedo.

**L6 — Um clique, um raio.** O raio tem começo, meio e fim. Cliques durante o roteiro são ignorados.

**L7 — Nada de ameaça que chegue perto.** Hoje não há vilão. Se um dia houver Lex, vale a lei integral da Teia.

**L8 — Nada de números.** *(Exceção nominal: o placar.)* A caixa não mostra dígitos.

**L9 — Nenhum pico de som.** O raio é zumbido agudo curto, não sirene. Nenhum estouro.

**L10 — Ele decide o ritmo.** Fechar o jogo no meio não é fracasso.

## O roteiro

```
esperando ──clique no brinquedo──▶ mirar ──▶ laser ──▶ coleta ──▶ guarda ──▶ festa
     ▲                                                                    │
     └────────── próximo brinquedo (outro lado, 0,4 s) ◀──────────────────┘
                    │
                    └── 3 brinquedos ──▶ festa (caixa pulsa dourada) ──▶ novo cenário
```

| Etapa | Duração | O que o Murilo vê | O que ele ouve |
| --- | --- | --- | --- |
| Esperando | indefinida | Super Homem **voando** (nunca pousa), caixa no braço, capa ondulando; brinquedo mexendo com halo | Ambiente suave |
| Mirar | 0,25 s | Ele vira para o brinquedo; capa estica | Sopro curto |
| Laser | 0,45 s | **Dois raios vermelhos finos, um de cada olho**, que se juntam no brinquedo | Zumbido agudo curto |
| Coleta | 0,4 s | O brinquedo solta e **voa em arco até a caixa**; a tampa abre | `pop` + som do brinquedo |
| Guarda | 0,35 s | Tampa fecha; a caixa brilha; ele volta a voar de frente | Duas notas |
| Festa (a cada 3) | 3 s | Confete, a caixa pulsa dourada, ele gira no ar | Jingle de 8 notas |

**Uma ação completa dura ~1,8 s** — o mais curto dos quatro jogos (o raio é imediato).

**O que torna este jogo um jogo (não uma skin):**

1. **Ele nunca pousa** — Teia pende, Força pousa, Veloz derrapa, Super **voa o tempo todo**.
2. **O brinquedo que viaja, não o herói** — o raio alcança de onde ele está.
3. **A caixa carregada** — o brinquedo não some: entra na caixa.

**O brinquedo está sempre à vista.** Mexendo devagar, com halo.

**Determinismo.** Lado do herói é contador (`jogo.lado ^= 1` a cada rodada e a cada troca de cenário). O brinquedo da rodada é `amigos[jogo.guardados % amigos.length]` — os três do cenário saem em sequência, sem repetir. Nenhum `Math.random` em decisão de jogo; só em partículas (confete, poeira).

## Placar

Desde a v1.0 (`../PADRAES.md`, `ADR 0004`). Igual aos irmãos: canto superior direito, desenho à esquerda, número à direita, cumulativo na sessão, só itens já guardados, 1,5× no celular. NF4 nasce em **240**. A caixa **não** mostra dígitos.

## Elenco

**Herói — Super Homem.** Capa vermelha, azul chapado, losango no peito (sem letra), botas vermelhas. Silhueta em 200 ms: capa + caixa. Três poses: voando, mirando, laser nos olhos. Cara de calma, nunca de raiva. Grande na tela, mas nunca cobre o brinquedo. **Sempre Super Homem, sem Clark** (`ADR 0002`).

**Brinquedos.** Uma lista de três por cenário, sempre na mesma ordem. O contador de guardados escolhe o da rodada (`guardados % 3`), então dentro do mesmo cenário nunca repete: base, depois o segundo, depois o terceiro. Sempre visíveis. Cada um com som e reação próprios.

| Cenário | 1º guardado | 2º guardado | 3º guardado |
| --- | --- | --- | --- |
| Metropolis | Bola (`ploc`) | Ursinho (`munchido`) | Balão (`ploc`) |
| Campo | Pipa (sopro de papel) | Carrinho (`vrum`) | Patinho (`patinho`) |
| Pôr do sol | Gatinho (`miau`) | Bola de praia (`bola`) | Sorvete (sino curto) |

**Caixa.** De madeira, tampa que abre, presa no braço esquerdo. O brinquedo entra e a tampa fecha. Na festa, a caixa pulsa dourada. Não mostra dígitos (`ADR 0006`).

**Lado.** O herói não espelha: ele **inclina** o corpo (até 9°) na direção do alvo. Assim a caixa fica sempre no mesmo braço — esquerda da tela — e nunca passa na frente do brinquedo.

**Sem vilão.** Os brinquedos ficaram espalhados pelo vento. Nenhum Lex, nenhum Zod. Se um dia pedir vilão, Lex de longe com L7 integral — igual Eggman.

## Cenários

Três, nesta ordem. Céu e cidade, coerentes com voar. Água fica de fora.

1. **Metropolis** — prédios, céu azul, ele voa entre as torres.
2. **Campo (Smallville)** — verde, celeiro ao fundo, cerros.
3. **Pôr do sol / nuvens** — laranja e rosa, o fecho.

## Áudio

- 100% sintetizado em Web Audio. Master −12 dBFS, limitador, pico ≤ −6 dBFS.
- `laser` = zumbido agudo curto (não sirene); `pop` na coleta; amigos = mesmos sons da Teia/Força/Veloz; `munchido` do ursinho; `vrum` do carrinho; jingle de 8 notas.
- Sem rugido, sem sirene, sem narração.
- `M` muta, `F` tela cheia no desktop, primeiro toque no celular.

## O que este jogo não tem

Sem Clark. Sem criptonita. Sem Super Homem de aço prateado. Sem combate. Sem vilão. Sem água. Sem loop jogável. Sem cronômetro. Sem brinquedo que escapa. Sem anéis. Sem derrota. Sem tempo. Sem menu. Sem texto — só os dígitos do placar e o losango do peito (sem letra). Sem quinto cenário. Sem two players.

## Como saber se funcionou

Mesmo protocolo: 5 minutos, uma sessão.

| Sinal | Leitura |
| --- | --- |
| Onde ele clica primeiro | No brinquedo = entendi. No centro = aumentar aim assist. |
| Espera o raio ou clica repetido? | Repetido = raio longo demais. |
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
| Verbo | Apontar; raio automático | este documento |
| Missão | Recolher brinquedo na caixa | este documento |
| Identidade | Sempre Super Homem, sem Clark | `ADR 0002` |
| Travesso | Nenhum | `ADR 0001` |
| Voo | Nunca pousa; alterna de lado | `ADR 0003` |
| Placar | Desde a v1.0 | `ADR 0004` + `../PADRAES.md` |
| Tela cheia no celular | Primeiro toque | `ADR 0005` + `../PADRAES.md` |
| Laser | Dois raios, um de cada olho | `ADR 0006` |
| Caixa | Presa no braço esquerdo | `ADR 0006` |
| Estrutura | `murilo/super/` | este documento |

## Pendências

- Testar com o Murilo (K1/K2) antes de chamar de pronto.
- Cartão na vitrine: adicionar quando a sessão confirmar (hoje o arquivo existe e abre direto pelo caminho).
