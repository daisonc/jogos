# Laser dos dois olhos; caixa presa no braço esquerdo

Duas marcas do personagem, decididas juntas. **Laser:** dois feixes vermelhos finos, um de cada olho, que se juntam no brinquedo — visão de calor clássica. Duração 0,45 s, zumbido agudo curto, não sirene (`L9`). **Caixa:** de madeira, tampa que abre, presa no braço esquerdo — "carregar consigo" é o pedido do operador. O brinquedo voa em arco até ela; a tampa abre na coleta e fecha na guarda. A caixa não mostra dígitos (o placar do canto já é a exceção nominal a L8). O logo do peito é um **losango sem letra** (L3).

## Status

accepted (2026-11-03)

## Considered Options

- **Um raio grosso:** rejeitada. Dois raios dos olhos é a marca.
- **Caixa flutuando ao lado:** rejeitada. "Carregar consigo" é o pedido; mais fácil seria, mas descaracteriza.
- **Caixa com número de brinquedos:** rejeitada. Seria um segundo placar (L8).
- **Dois raios + caixa no braço:** adotada.

## Consequences

- O raio é dois `<path>`/`<line>` que nascem nos olhos e se encontram no alvo — só `transform`/`opacity`.
- A caixa é um grupo que acompanha o `transform` do herói; a tampa é nó separado (gira na coleta).
- Se o raio assustar (L9/K1), encurtar para 0,3 s ou baixar o ganho; não transformar em sirene.
