# Sem derrota, sem cronômetro

O jogo é para uma criança de 3 anos. Decidimos que não existe game over, vida, tentativa, contagem regressiva nem alvo que expira: errar simplesmente não é um evento, e a teia volta sem som de erro. A razão é que perder, nessa idade, não ensina nada — só encerra a brincadeira, e a criança não consegue explicar que o motivo do abandono foi a mecânica. O custo aceito é que o jogo não tem desafio e não vai segurar uma criança mais velha por muito tempo; isso é intencional, e a resposta correta para um dia parecer fácil demais é fazer outro jogo, não endurecer este.

## Status

accepted

## Considered Options

- **Falha explícita** (vidas, tentativas, placar): rejeitada. Aos 3 anos o placar não é lido, e a punição chega antes do entendimento.
- **Falha silenciosa** (erra a teia, ela volta, sem som): foi o que adotamos, mas sem nenhum estado de derrota em volta.
- **Dificuldade adaptativa**: rejeitada por ora. Não há dado de nenhuma sessão real do Murilo; calibrar dificuldade sem observação é adivinhação.

## Consequences

- Não há motivo para placar, ranking, "recorde" ou tela de resultado — o que simplifica o jogo inteiro.
- O progresso precisa ser visível sem números: estrelas que acendem, o resgatável de volta ao lugar.
- Se em algum momento o jogo parecer fácil demais, a saída é outro jogo (por idade), não um modo difícil neste.
