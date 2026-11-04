# Tela cheia no celular no primeiro toque

Padrão da casa (`PADRAES.md`). Até a v1.5, o celular entrava em tela cheia com **dois toques rápidos** — o atalho `F` exige teclado, e a API de fullscreen exige gesto. O operador pediu que o jogo **já abra em tela cheia** no celular, sem duplo-toque. Decidimos: o carregamento **tenta** entrar em tela cheia se o ponteiro primário é grosso (`(pointer: coarse)`), e o **primeiro toque** garante se o navegador exigir gesto. O mesmo toque continua sendo resgate. Desktop (mouse) continua com `F`; mouse não alterna. Onde a API não existe (iPhone), o gesto é inócuo — o caminho segue "Adicionar à Tela de Início". O duplo-toque sai.

## Status

accepted (2026-11-03)

## Considered Options

- **Duplo-toque para alternar** (até a v1.5): rejeitada como padrão. É um gesto que o Murilo não vai descobrir sozinho, e o operador pediu tela cheia sem ele.
- **Tentar no carregamento, sem gesto**: fica. Funciona em PWA e onde o navegador permitir; falha em silêncio onde exige gesto.
- **Primeiro toque entra em tela cheia**: adotado como garantia.
- **Overlay "toque para começar"**: rejeitada. É UI e é texto — L3.

## Consequences

- `aoToqueRapido` / `ultimoToque` saem. Entra `entrarTelaCheia()` + `aoToque`.
- `viewport-fit=cover` entra, para a cena usar a tela inteira (notch incluído).
- O primeiro toque no jogo é jogada **e** tela cheia ao mesmo tempo — não se perde um resgate.
- Sair da tela cheia no celular é responsabilidade do adulto (botão do navegador / gesto do sistema). `L10` vale: fechar é comportamento normal.
