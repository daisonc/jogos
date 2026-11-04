# Teia do punho; caixa no chão ao lado do herói

Duas marcas do personagem, decididas juntas — e que distinguem o jogo do Raio.

**Teia:** um fio branco que sai do **punho direito** (com o lançador de teia) e estica até o brinquedo; ao chegar, um **nó de teia** se cola nele. Duração 0,38 s, estalo curto (`thwip`), não sirene (`L9`). Como o herói está de pé e o braço já aponta para a direita, os brinquedos ficam **sempre à direita** — não há feixe dos olhos nem dois braços.

**Caixa:** de madeira, tampa que abre, **parada no chão ao lado do herói**. No Raio a caixa era carregada no braço porque o herói voava com as mãos livres; aqui o herói usa as duas mãos para a teia, então a caixa espera no chão, que é onde o brinquedo chega. O brinquedo é puxado em arco até ela; a tampa abre na coleta e fecha na guarda. A caixa não mostra dígitos (o placar do canto já é a exceção nominal a L8).

## Status

accepted (2026-11-03)

## Considered Options

- **Dois fios, um de cada punho**: rejeitada. O pedido é "atirar a teia", no singular; um braço só.
- **Caixa presa nas costas / no braço**: rejeitada. O herói está no chão e de frente; a caixa no braço taparia a teia.
- **Caixa flutuando**: rejeitada. O herói não voa; a caixa também não.
- **Fio do punho + caixa no chão**: adotada.

## Consequences

- A teia é um `<path>` do punho (ponto local `(90, -42)` transformado com o herói) até o brinquedo; o nó é um grupo pré-criado — só `transform`/`opacity`.
- A caixa é grupo **irmão** do herói (não acompanha o `transform`); a tampa é nó separado (gira na coleta).
- Se a teia assustar (L9/K1), encurtar a duração ou baixar o ganho; não transformar em sirene.
