# Veloz do Murilo — Tema

- **Versão**: 0.2 (v1.0 implementada em 2026-11-03; pendente K1/K2 e medições oficiais)
- **Jogador**: Murilo, 3 anos
- **Plataforma**: navegador de notebook — mouse e teclado obrigatórios
- **Celular (cortesia)**: o arquivo também abre no navegador do celular; lá, o **primeiro toque** entra em tela cheia (padrão da casa `PADRAES.md`). O alvo jogável continua sendo notebook com mouse.
- **Voltar à vitrine**: botão fixo e colorido no canto superior esquerdo, sem texto, só uma seta — `../../index.html`.
- **Arquivo**: `murilo/sonic/veloz-do-murilo.html`, arquivo único
- **Herói**: Sonic, identidade oficial do desenho clássico — **sempre Sonic, sem Super Sonic** (`ADR 0002`)
- **Travesso**: Dr. Eggman, sempre de longe, no Egg Mobile, fugindo (`ADR 0001`)

---

## A frase

> Murilo aponta, o Sonic dispara, e o mundo vira um borrão até o amigo aparecer.

Se qualquer decisão futura contradizer essa frase, a decisão está errada.

## A missão

> **Chegar rápido onde ninguém chegou.** Sempre um amigo por vez. Sem pressa dele — a pressa é do Sonic.

A missão é resgate por velocidade. O prazer é o disparo; o amigo no fim é a prova de que funcionou. Anéis são enfeite, não missão.

## A fantasia

Não é "um jogo do Sonic". É a experiência de **ter a velocidade que resolve a distância**.

Aos 3 anos, a criança não alcança nada: o brinquedo do outro lado da sala, o pai na porta, o cachorro no quintal. O que ela tem é um dedo. O jogo pega esse dedo e dá a ele o corpo do ouriço azul.

O prazer não está em vencer uma corrida, e sim em **apontar e o mundo obedecer em um borrão**.

**Corolário:** o mouse não é acelerador, é varinha. Murilo não segura botão, não carrega dash, não desvia. Ele **aponta para onde o Sonic vai chegar**.

## Quem está jogando

Os mesmos fatos da Teia e do Força — mesmo jogador, mesmo notebook, mesmo momento:

| Fato | Consequência |
| --- | --- |
| 3 anos: motricidade fina incompleta | Não haverá mira. Aim assist de 100%. |
| Pré-leitor | Zero texto, salvo os dígitos do placar (`PADRAES.md`). |
| Atenção de 2 a 5 minutos | Prazer nos primeiros 5 segundos, sem tutorial. |
| Perder não ensina | Zero derrota, zero cronômetro, zero som de erro. |
| Repetição é conforto | O mesmo roteiro pode rodar 20 vezes. |
| Causa e efeito é o prazer central | Todo clique em alvo tem consequência imediata e exagerada: o mundo vira listra, o amigo aparece. |
| Medo não tem rollback | Eggman nunca chega perto, nunca de frente, nunca close. Risada aguda e curta. |
| Ele já conhece a Teia e o Força | Já sabe o verbo (apontar) e o placar. Este jogo não pode ser um passo atrás em generosidade. |

## As leis

As dez leis da casa, relidas para este jogo.

**L1 — Nenhuma derrota.** Não existe game over, vida, tentativa, corrida perdida, "quase!". Clique no vazio não faz nada.

**L2 — Nenhum prazo.** Nenhum cronômetro, nenhuma corrida contra o tempo. A pressa é do Sonic, nunca do Murilo.

**L3 — Zero texto.** *(Exceção nominal: os dígitos do placar — `PADRAES.md`.)*

**L4 — Um alvo por vez.** Um amigo preso, um alvo. A tela nunca pede escolha. Anéis do cenário não são alvo.

**L5 — O mouse é varinha.** Aim assist de 100% (círculo de 220 px + terço central), mirando no amigo/obstáculo.

**L6 — Um clique, um disparo.** O disparo tem começo, meio e fim. Cliques durante o roteiro são ignorados.

**L7 — Nada de ameaça que chegue perto.** Eggman sempre de longe, no Egg Mobile, fugindo, de costas ou de perfil, nunca de frente, nunca close, some em ≤ 2 s. Risada `ohoho` aguda e curta, nunca grave (`ADR 0001`).

**L8 — Nada de números.** *(Exceção nominal: o placar.)* Anéis não contam, não têm contador, não são pontuação.

**L9 — Nenhum pico de som.** O disparo é `whoosh` longo e filtrado. Nenhum estouro. Nenhuma sirene.

**L10 — Ele decide o ritmo.** Fechar o jogo no meio não é fracasso.

## O roteiro

```
esperando ──clique no amigo──▶ girar ──▶ disparo ──▶ chegada ──▶ abertura ──▶ gratidão
     ▲                                                                            │
     └────────── próximo amigo (outro lado, 0,4 s) ◀──────────────────────────────┘
                    │
                    └── 3 amigos ──▶ festa (chuva de anéis) ──▶ novo cenário
```

| Etapa | Duração | O que o Murilo vê | O que ele ouve |
| --- | --- | --- | --- |
| Esperando | indefinida | Sonic tapando o pé; amigo mexendo; halo no alvo | Ambiente suave |
| Girar | 0,25 s | Sonic vira bola girando no lugar | Sopro curto + `spin` |
| Disparo | 0,9 s | **O espetáculo:** rastro azul, linhas de velocidade, cenário estica; arrancada e chegada com easing | `whoosh` longo |
| Chegada | 0,25 s | Pouso derrapando, poeira azul | `thud` curto + freio |
| Abertura | 0,5 s | Gaiola/obstáculo se abre; amigo levanta | Estalo + som do amigo |
| Gratidão | 0,6 s | Amigo pula; Sonic acena | Duas notas |
| Festa (a cada 3) | 3 s | Confete **+ anéis dourados girando** (visual, sem número) | Jingle de 8 notas |

**Uma ação completa dura ~2,5 s.**

**O que torna este jogo um jogo (não uma skin):**

1. **O disparo horizontal** — Teia é pêndulo, Força é salto, Sonic é reta em velocidade. O cenário estica (só `transform`).
2. **Anéis na festa** — identidade Sonic, sem violar L8.
3. **O herói fica onde para** — o próximo amigo nasce do outro lado (`ADR 0003`).

**O amigo está sempre à vista.** Preso = imóvel com parte do corpo para fora, mexendo devagar.

**Determinismo.** Lado do próximo amigo é contador. Escolha do amigo é contador de visitas ao cenário. Nenhum `Math.random` em decisão de jogo.

## Placar

Desde a v1.0 (`PADRAES.md`, `ADR 0004`). Igual ao da Teia e do Força: canto superior direito, desenho à esquerda, número à direita, cumulativo na sessão, só itens já salvos, 1,5× no celular. NF4 nasce em **240**.

## Elenco

**Herói — Sonic.** Azul chapado, barriga bege, tênis vermelhos, contorno escuro. Silhueta em 200 ms: espinhos para trás, tênis vermelhos. Três poses: tapando o pé, bola girando, derrapando. Cara de coragem, nunca de raiva. Grande na tela, mas nunca cobre o amigo.

**Amigos.** Uma lista por cenário. A rodada (visita ao cenário) escolhe um: base primeiro, depois objetos da Teia. Sempre visíveis. Cada um com som e reação próprios.

| Cenário | Base (1ª rodada) | 2ª rodada | 3ª rodada |
| --- | --- | --- | --- |
| Green Hill | Coelhinho (`fungado`) | Gatinho (`miau`) | Balão (`ploc`) |
| Canyon | Cervinho (`cervo`) | Pipa (sopro de papel) | Cachorrinho (`latido`) |
| Cidade | Patinho (`patinho`) | Bola (`bola`) | Sorvete (sino curto) |

**Travesso — Dr. Eggman.** Sempre de longe, no Egg Mobile, fugindo, de costas ou de perfil, sem close. Causa do resgate, não obstáculo. Some em ≤ 2 s. Risada `ohoho` aguda e curta. Reutiliza a lógica do Duende da Teia (`ADR 0001`). Plano B: remover e deixar só a gaiola.

**Anéis.** Enfeite do cenário (estáticos) e chuva na festa. Não são alvo, não contam, não têm contador (`L8`). Nenhum número novo entra no jogo.

## Cenários

Três, nesta ordem. Natureza + um urbano, como a Teia. Água fica de fora (afogamento é susto; água é caro).

1. **Green Hill** — xadrez verde, céu azul, palmeiras, girassóis. Abertura: o cenário do Sonic, reconhecível em 1 s.
2. **Canyon** — laranja e pedra, mesas ao fundo. Loops ao fundo como paisagem, nunca jogáveis.
3. **Cidade / Estação** — prédios, trilhos, o mais próximo da vida real dele (como a rua da Teia).

## Áudio

- 100% sintetizado em Web Audio. Master −12 dBFS, limitador, pico ≤ −6 dBFS.
- `spin` = whoosh curto agudo; `disparo` = whoosh longo; freio = ruído curtíssimo; `ohoho` = três blips agudos; amigos = mesmos sons da Teia/Força; jingle de 8 notas.
- Sem rugido, sem sirene, sem narração.
- `M` muta, `F` tela cheia no desktop, primeiro toque no celular.

## O que este jogo não tem

Sem Super Sonic. Sem transformação. Sem combate. Sem água. Sem loop jogável. Sem cronômetro. Sem corrida contra Eggman. Sem anéis que contam. Sem derrota. Sem tempo. Sem menu. Sem texto — só os dígitos do placar. Sem quinta cenário. Sem two players.

## Como saber se funcionou

Mesmo protocolo: 5 minutos, uma sessão.

| Sinal | Leitura |
| --- | --- |
| Onde ele clica primeiro | No amigo = entendi. No centro = aumentar aim assist. |
| Espera o disparo ou clica repetido? | Repetido = disparo longo demais. |
| Ri do Eggman ou desvia o olhar? | Desvia = Eggman mais longe ainda. |
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
| Verbo | Apontar; disparo automático | este documento |
| Missão | Resgate por velocidade | este documento |
| Identidade | Sempre Sonic, sem Super Sonic | `ADR 0002` |
| Travesso | Eggman de longe | `ADR 0001` |
| Herói no cenário | Fica onde para | `ADR 0003` |
| Placar | Desde a v1.0 | `ADR 0004` + `PADRAES.md` |
| Tela cheia no celular | Primeiro toque | `ADR 0005` + `PADRAES.md` |
| Anéis | Enfeite, não contam | este documento |
| Estrutura | `murilo/sonic/` | este documento |

## Pendências

- ~~Implementar `veloz-do-murilo.html`~~ — **feito** (v1.0, 2026-11-03).
- ~~Adicionar o cartão na vitrine~~ — **feito**.
- Testar com o Murilo (K1/K2) antes de chamar de pronto.
