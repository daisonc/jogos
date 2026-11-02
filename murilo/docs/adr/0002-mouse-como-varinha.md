# Mouse como varinha

O jogo roda em notebook, com mouse, mas o jogador tem 3 anos e não consegue mirar. Decidimos que o mouse não é volante e sim varinha: qualquer clique suficientemente perto do resgatável conta como acerto em cheio (aim assist de 100%), e clique no vazio não produz nada — nem som, nem animação, nem reprovação. A razão é que precisão de mouse é a única habilidade que a criança não tem e a única que o jogo não precisa exigir para a fantasia funcionar; a escolha real do jogador é *quando* lançar a teia, não *onde*. O custo aceito é que o jogo não ensina coordenação motora e não recompensa habilidade — o que descarta este desenho como jogo para uma criança de 6 anos.

## Status

accepted

## Considered Options

- **Mira real com tolerância pequena**: rejeitada. Transforma cada tentativa em fracasso estatístico para quem tem motricidade fina incompleta.
- **Auto-aim total** (o jogo escolhe o alvo e dispara sozinho): rejeitada. É diferente de aim assist: elimina a agência, e sem agência não sobra a fantasia de "eu movi o mundo".
- **Controle por teclado**: rejeitada. Aos 3 anos o teclado é do adulto; serve para mudo e tela cheia, não para jogar.

## Consequences

- Só pode existir um resgatável por vez na tela; com dois, o aim assist teria que escolher por ele, e escolher por ele é auto-aim.
- A tolerância precisa ser generosa a ponto de um clique no terço central da tela valer, e é calibrável por observação, não por teoria.
- Cliques durante o balanço são ignorados, não enfileirados: uma teia por vez, com começo, meio e fim.
