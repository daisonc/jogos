# Botão de voltar para a vitrine

Com a criação da vitrine (`index.html`), o jogo passou a ser alcançado a partir dela. O Murilo tem 3 anos e **não usa o botão "voltar" do navegador** — sem uma saída visível dentro do jogo, ele fica preso na partida (ou fecha a janela). Pedido do operador: um botão bonito e colorido, no canto superior esquerdo, que leve de volta à vitrine. Isso adiciona ao jogo uma **UI persistente**, algo que a doutrina do `TEMA.md` evitava (a única UI existente era o feedback de mudo, que aparece por 1 s e some). Decidimos aceitar a exceção porque, num produto com mais de um jogo, a navegação de volta é infraestrutura, não decoração — e porque a alternativa (ensinar a criança a usar o navegador) não existe. O botão não tem texto: é um ícone (seta para a esquerda), coerente com "se a informação não cabe em forma, cor ou som, ela não entra".

## Status

accepted (2026-11-03)

## Considered Options

- **Não colocar saída; ensinar o botão do navegador.** Rejeitada: a premissa do pedido é que a criança não usa o navegador, e o `L10` diz que fechar/voltar é comportamento normal de uma criança de 3 anos.
- **Desenhar o botão dentro do SVG do jogo.** Rejeitada: escalaria junto com o letterbox e ficaria minúsculo no celular; além de somar nós e depender do `viewBox`.
- **Botão com texto ("Voltar").** Rejeitada: o Murilo não lê; o ícone basta.

## Consequences

- O botão é um **elemento HTML fixo** (`<a class="voltar">`), fora do `<svg id="palco">`: não escala com a cena (fica grande e clicável no celular), não soma nós SVG (NF4 intacto — segue 240 com oito itens) e não interage com o `viewBox`.
- Aponta para `../index.html` (a vitrine), então funciona em `file://` e em hospedagem (a URL é relativa).
- **Sem texto**: só uma seta (chevron) desenhada em SVG. Alvo ≥ 56 px; respeita `env(safe-area-inset-*)` e `prefers-reduced-motion`.
- O **indicador de mudo** foi movido do canto superior esquerdo para o **topo-centro** (agora vive em `translate(899 0)` no SVG), para não coincidir com o botão quando a tela está em 16:9 cheio.
- Reversível: remover o `<a class="voltar">` e o bloco `.voltar` do CSS restaura o comportamento anterior; o indicador de mudo volta ao canto desfazendo o `translate`.
