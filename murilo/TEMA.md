# Teia do Murilo — Tema

- **Versão**: 1.5 (v1.0–v1.5 homologadas em 2026-11-03 — v1.4 placar e inversão de lado; v1.4.1 balanço em pêndulo ancorado num ponto alto; v1.4.2 placar ampliado no celular; v1.5 botão de voltar para a vitrine)
- **Jogador**: Murilo, 3 anos
- **Plataforma**: navegador de notebook — mouse e teclado obrigatórios
- **Celular (cortesia)**: o arquivo também abre no navegador do celular; lá, dois toques rápidos alternam a tela cheia (a tecla `F` exige teclado). O alvo jogável continua sendo notebook com mouse — toque não é mecânica suportada.
- **Voltar à vitrine**: um botão fixo e colorido no canto superior esquerdo da tela do jogo leva de volta à vitrine (`../index.html`). É a única UI persistente do jogo — sem texto, só uma seta (`ADR 0005`).
- **Arquivo**: `murilo/teia-do-murilo.html`, arquivo único
- **Herói**: Homem-Aranha, identidade oficial, referência do desenho clássico
- **Travesso**: Duende Verde, suavizado em tom, nunca em close

---

## A frase

> Murilo aponta, a teia gruda, e o mundo balança até o resgate acontecer.

Se qualquer decisão futura contradizer essa frase, a decisão está errada.

## A fantasia

Não é "um jogo do Homem-Aranha". É a experiência de **ter o poder de mover o mundo com um gesto**.

Aos 3 anos a criança não tem força, não tem altura, não tem precisão e não tem permissão para quase nada. O que ela tem é um dedo. O jogo pega esse dedo e dá a ele o corpo do Homem-Aranha.

O prazer não está em vencer, e sim em **causar**. Apontar e ver a cidade obedecer é a fantasia inteira; o resgate é apenas a prova de que funcionou.

**Corolário:** o Murilo não pilota, ele **dirige**. O mouse não é volante, é varinha.

## Quem está jogando

Toda a doutrina abaixo deriva destes fatos, não de preferência estética:

| Fato | Consequência |
| --- | --- |
| 3 anos: motricidade fina incompleta | Mouse é o pior dispositivo possível para ele — exige mira, travessia de tela e clique coordenado. Não haverá mira. |
| Pré-leitor | Zero texto. Nenhum caractere na tela, em nenhum momento — nem título, nem menu. |
| Atenção de 2 a 5 minutos | O jogo precisa entregar prazer nos primeiros 5 segundos, não depois de um tutorial. |
| Perder não ensina, só encerra a brincadeira | Zero derrota, zero cronômetro, zero som de erro. |
| Repetição é conforto, não tédio | O mesmo loop pode rodar 20 vezes. Variedade é bônus, não requisito. |
| Causa e efeito é o prazer central | Todo clique em alvo tem consequência imediata e exagerada. |
| Medo não tem rollback | Um susto e ele simplesmente não quer mais jogar — e não sabe explicar por quê. O Duende nunca chega perto. |
| Ele joga com adulto por perto | O jogo não precisa se explicar sozinho nos primeiros minutos. |

## As leis

Não são recomendações. São restrições de projeto — qualquer uma delas quebrada descaracteriza o jogo.

**L1 — Nenhuma derrota.** Não existe game over, vida, tentativa, "quase!", nem tela de falha. Não existe estado em que o Murilo perdeu.

**L2 — Nenhum prazo.** Nenhum cronômetro, nenhum alvo que expira, nenhum "clique rápido". O jogo espera o tempo que ele precisar.

**L3 — Zero texto.** Nem título, nem instrução, nem "você conseguiu". Se a informação não cabe em forma, cor ou som, ela não entra. *(Exceção nominal da v1.4: os dígitos do placar — ver `ADR 0004`.)*

**L4 — Um alvo por vez.** A tela nunca pede escolha. Escolher entre duas coisas é uma habilidade que ele ainda não tem, e a escolha errada (na cabeça dele) é indistinguível de fracasso.

**L5 — O mouse é varinha.** Aim assist de 100%: qualquer clique suficientemente perto acerta em cheio. Clique no vazio não faz nada — nem som, nem tremor, nem olhar de reprovação.

**L6 — Um clique, uma teia.** A teia tem começo, meio e fim. Cliques durante o balanço são ignorados, não enfileirados — senão a tela vira caos e o caos é ilegível para ele.

**L7 — O Duende nunca encara.** Sempre de longe, sempre no planador, sempre fugindo, sempre de costas ou de perfil. Ele é paisagem com intenção, nunca ameaça.

**L8 — Nada de números.** Progresso é visível (estrelas que acendem, objetos que voltam para o lugar), nunca contado com dígitos. *(Exceção nominal da v1.4: o **placar** mostra o desenho do resgatável salvo e o número de salvamentos, a pedido do operador — ver `ADR 0004`. Nenhum outro número entra no jogo.)*

**L9 — Nenhum pico de som.** Volume constante, nenhum estouro súbito, nenhum silêncio punitivo. Susto auditivo é tão ruim quanto visual.

**L10 — Ele decide o ritmo.** Fechar o jogo no meio não é fracasso, é o comportamento normal de uma criança de 3 anos. O jogo não cobra retorno.

## O loop

```
esperando ──clique perto do alvo──▶ teia ──▶ balanço ──▶ resgate ──▶ volta
     ▲                                                                  │
     └──────────────── 3 resgates ──▶ festa ──▶ novo cenário ◀──────────┘
```

| Etapa | Duração | O que o Murilo vê | O que ele ouve |
| --- | --- | --- | --- |
| Esperando | indefinida | Herói pendurado, balançando devagar. Alvo brilhando. | Ambiente suave |
| Teia | 0,25 s | Fio branco sai do pulso e gruda | `thwip` |
| Balanço | 1,2–1,8 s | Teia gruda num ponto alto; herói desce e sobe em pêndulo (ferradura), cruzando o resgatável | `whoosh` |
| Resgate | 0,5 s | Objeto salvo voa para o colo do herói | `pop` + gargalhada do resgatável |
| Volta | 0,6 s | Herói pousa no próximo ponto; o resgatado **agradece** e o herói dá um pulinho | som curto de gratidão |
| Festa (a cada 3) | 3 s | Confete, estrelas acendendo, herói pulando | jingle de 8 notas |

**Uma rodada completa dura 10–15 segundos de conteúdo.** O resto do tempo é o Murilo reagindo — e reagir é jogar.

**A âncora da teia (v1.4.1).** No balanço, a teia gruda num **ponto alto** e o herói pende dele: o corpo descreve um arco de pêndulo (ferradura), desce, cruza o fundo e sobe até o resgatável, que fica sobre a mesma circunferência. A teia está sempre esticada do pivô até o herói — nunca apontada para o objeto. O pivô é calculado na partida (bissetriz de partida/resgatável, na altura `TEIA_ANCORA_Y`) e não é arte: nenhum nó novo entra na cena.

## Placar e lado (v1.4)

Duas adições pedidas pelo operador, ambas reversíveis e sem tocar no loop (`ADR 0004`).

**Placar.** No canto superior direito, uma linha por resgatável **já salvo**: o desenho do item à esquerda e o número de salvamentos à direita. A contagem é **cumulativa na sessão** (não zera na festa nem na troca de cenário) e a linha nasce no primeiro salvamento, sem nunca se mover depois. Só entram itens que já foram salvos — nada de ícone com zero. O placar é a **única** exceção às leis L8 e L3, e o número existe porque o Murilo não lê: o desenho identifica o item, o dígito conta. No **celular**, o placar é ampliado em **1,5×** — a cena inteira encolhe pelo letterbox e o placar encolheria junto; no desktop o tamanho não muda.

**Lado.** A cada resgate, o lado em que o herói começa **inverte**: ele começa à esquerda e o objeto à direita; depois do primeiro salvamento, o objeto vai para a esquerda e o herói para a direita; e assim por diante, de forma contínua — atravessando a festa e a troca de cenário. O cenário inteiro é espelhado em torno do eixo central, **inclusive a rota de fuga do travesso**, que continua sempre a ≥ 60% da largura do herói (`L7`). Como o herói já virou para o lado novo no resgate, a volta deixa de ser a travessia de retorno e passa a ser um pouso curto: o herói **pinga** de um lado para o outro a cada ciclo.

## Elenco

**Herói — Homem-Aranha.** Traje clássico: torso vermelho, braços e pernas azuis, teias pretas, olhos brancos enormes. Precisa ser reconhecível pela silhueta em 200 ms, a 1 metro de distância, sem detalhe nenhum. Só três poses são necessárias: pendurado, balançando, pousando.

**Travesso — Duende Verde.** Visto exclusivamente de longe, no planador, contra o céu. Agarra o objeto, solta, some. Risada `ihihi` de desenho animado, nunca a gargalhada sinistra. Ele é **causa**, não obstáculo: existe para que haja o que resgatar, e é isso que impede o jogo de ser só coleta.

**Segundo travesso — Abutre.** Alterna com o Duende por rodada — com quatro cenários (número par), a escolha soma a volta à rodada para não fixar um travesso por cenário (`ADR 0003`). Visto exclusivamente de longe, de costas, de asas largas e **sem rosto**; reutiliza a rota de fuga do Duende no cenário e não se aproxima nunca. Grasnido curto e leve + duas batidas de asa, nada grave nem prolongado. Vale **L7 integralmente**: ele é paisagem com intenção, não ameaça.

**Trapalhada.** Na fuga, cada travesso faz **um** gesto cômico: o Duende perde o chapéu, que cai girando; o Abutre dá uma cambalhota no ar. Tudo acontece no ar e de longe (L7 vale integralmente): nunca há queda, impacto, dor nem choro — a graça é o movimento, nunca o dano. Um assobio descendente curto acompanha. O gesto é dado do travesso e não muda a duração da fuga.

**Resgatáveis.** Um por vez, um par por cenário (base + variação), cada um com som próprio e óbvio. O objeto alterna **a cada resgate**: base, variação, base. Como a rodada tem três resgates e recomeça a cada cenário, todo cenário abre pela base. Salvo, cada resgatável reage do seu jeito (espreguiça, gira, balança, quica, abana ou pulsa), sempre acompanhado de um som curto de gratidão.

| Cenário | Base | Som | Variação | Som da variação |
| --- | --- | --- | --- | --- |
| Telhados | Gatinho | `miau` agudo | Pipa | sopro de papel + assobio curto |
| Parque | Balão | `ploc` de borracha | Patinho de borracha | guincho de borracha |
| Rua do bairro | Cachorrinho | latido curto | Sorvete | sino curto |
| Praia | Tartaruguinha | trinado suave de duas notas | Bola de praia | quique de borracha leve |

## Cenários

Quatro, nesta ordem. Cada um precisa ser reconhecível em 1 segundo, sem texto.

1. **Telhados ao pôr do sol** — laranja e roxo, silhuetas de prédios, muito vazio embaixo. É o cenário de abertura porque é onde o balanço tem mais graça: altura e queda visíveis.
2. **Parque** — verde, céu azul claro, árvores redondas, balões. O resgate mais óbvio que existe: balão preso na árvore.
3. **Rua do bairro** — casas coloridas, carros, poste. O fecho, porque é o mais próximo da vida real dele.
4. **Praia** — céu azul-claro a creme, mar calmo, areia, coqueiro e guarda-sol. Entrou na v1.3 (`ADR 0003`): lugar novo, sem perigo, para a surpresa depois da terceira festa.

**A praia já foi descartada** na v1.0 ("água é caro de desenhar"). A v1.3 revisitou a decisão por outro ângulo — a de que um lugar novo é a forma de variedade que não pede habilidade nova — e a incluiu como quarto cenário. Um quinto continua fora de escopo.

## Áudio

- Efeitos sintetizados (Web Audio), música ambiente em loop, `M` para mudo.
- Áudio destravado no primeiro clique — exigência do navegador, não escolha.
- Sem narração de voz, em nenhuma versão. TTS de navegador pode encantar ou assustar, e não vale o risco.

## O que este jogo não tem

Sem vidas. Sem tempo. Sem menu. Sem texto — só os dígitos do placar da v1.4. Sem game over. Sem combate. Sem morte. Sem leitura. Sem precisão de mouse. Sem escolha. Sem tutorial.

Cada ausência desta lista é uma decisão registrada, não um esquecimento.

## Como saber se funcionou

Tema de jogo infantil que não foi testado com a criança é ficção. Protocolo: 5 minutos, uma sessão, cinco sinais observados.

| Sinal | Leitura |
| --- | --- |
| Onde ele clica primeiro | No alvo = entendi o verbo. No centro da tela = alvo pequeno, aumentar aim assist. |
| Ele espera o balanço ou clica repetido? | Repetido e frenético = o balanço está longo demais. |
| Ri do Duende ou desvia o olhar? | Desvio = Duende mais longe ainda, ou fora de quadro. |
| Pede "de novo" quando a festa acaba? | O único sinal que importa de verdade. |
| Levanta da cadeira antes de 2 minutos? | O cenário está errado, não o jogo. Trocar o cenário, não a mecânica. |

## Mapa de decisões

| Decisão | Escolha | Registro |
| --- | --- | --- |
| Plataforma | Notebook, navegador, mouse + teclado | este documento |
| Verbo | Teia e balanço, com física automática | este documento |
| Falha | Nenhuma, nenhum cronômetro | `docs/adr/0001-sem-derrota-sem-cronometro.md` |
| Entrada | Mouse como varinha, aim assist 100% | `docs/adr/0002-mouse-como-varinha.md` |
| Identidade | Homem-Aranha oficial, desenho clássico | este documento |
| Travesso | Duende e Abutre (alternados por rodada), sempre de longe | este documento |
| Estilo | Vetor chapado, SVG inline | este documento |
| Áudio | Efeitos + música, com mudo | este documento |
| Progressão | Rodadas de 3 resgates + festa | este documento |
| Placar | Desenho do item salvo + número de salvamentos, cumulativo na sessão | `docs/adr/0004-placar-e-inversao-de-lado.md` |
| Lado | Invertido a cada resgate, de forma contínua (cenário espelhado) | `docs/adr/0004-placar-e-inversao-de-lado.md` |
| Estrutura | Uma pasta por criança | este documento |

## Pendências para a v1.1

Não são dúvidas — são coisas que só o teste com o Murilo podia decidir. O teste foi feito e homologado em 2026-11-03 (K1/K2); a v1.1 está liberada.

- Um segundo travesso, se o Duende funcionar.
- O jogo do Pedro, em `pedro/`, quando ele pedir.
