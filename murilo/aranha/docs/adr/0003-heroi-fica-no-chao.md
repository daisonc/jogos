# O herói fica no chão — nunca voa, nunca balança

O pedido do operador foi "o mesmo do Super Homem, mas o Homem-Aranha, que **não voa**". Então a diferença central deste jogo em relação ao irmão Raio é a postura: o Super Homem paira o tempo todo; o Homem-Aranha **fica de pé no chão**, na altura do Murilo. Ele tem uma sombra no chão, respira de leve, e a única coisa que viaja até o brinquedo é a teia.

Isso também distingue este jogo do **Teia do Murilo**, onde o herói balança entre pontos de teia. Aqui não há balanço, não há voo, não há pouso: ele já está no chão.

Sem "alternar de lado": como ele está de pé, teleportar de um lado para o outro seria estranho. Ele fica numa posição fixa por cenário e apenas se inclina para o brinquedo. O brinquedo é que muda de lugar — há **três pontos** por cenário, um por rodada.

## Status

accepted (2026-11-03)

## Considered Options

- **Voar como o irmão**: rejeitada. Descaracteriza o Aranha e contradiz o pedido.
- **Balançar entre postes (teia)**: rejeitada. Seria o Teia do Murilo de novo; e o herói precisa estar parado para atirar.
- **Andar até o brinquedo**: rejeitada. Já resolvido com a teia; andar gastaria FSM e arte sem somar.
- **Ficar de pé, atirar a teia; o brinquedo viaja**: adotada.

## Consequences

- Sem estado de voo, sem bob de altura, sem derrapagem, sem `jogo.lado`.
- Herói com `chao` por cenário (y ≈ 892–902) e uma sombra que encolhe no pulinho da festa.
- `posHeroi()` devolve x fixo + `chao`; `posBrinquedo()` devolve o ponto da rodada.
- Distância do braço sempre dentro do alcance da teia.
