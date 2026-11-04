# Tela cheia no celular no primeiro toque

Padrão da casa (`PADRAES.md`). A v1.0 deste jogo nasceu com o duplo-toque herdado da Teia. O operador pediu que o jogo já abra em tela cheia no celular, sem duplo-toque. Mesma decisão da Teia v1.6 (`murilo/docs/adr/0006-tela-cheia-no-primeiro-toque.md`): o carregamento tenta se o ponteiro é grosso, e o primeiro toque garante. O mesmo toque continua sendo socorro. Desktop continua com `F`. iPhone: gesto inócuo; "Adicionar à Tela de Início".

## Status

accepted (2026-11-03)

## Considered Options

- **Manter duplo-toque**: rejeitada. Padrão da casa agora é primeiro toque.
- **Overlay "toque para começar"**: rejeitada. UI + texto (L3).
- **Carregamento tenta; primeiro toque garante**: adotada.

## Consequences

- `aoToqueRapido` / `ultimoToque` saem. Entra `entrarTelaCheia()` + `aoToque`.
- `viewport-fit=cover` entra.
- O primeiro toque é socorro **e** tela cheia ao mesmo tempo.
- Sair da tela cheia no celular é do adulto (`L10`).
