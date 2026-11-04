# Padrões da casa

Dois critérios que valem para **todo jogo** deste repositório a partir de 2026-11-03. Não são lembrança de cada PRD: são pré-requisitos de qualquer v1.0. Um jogo novo que não os traz não está pronto.

## 1. Placar desde a v1.0

Todo jogo nasce com o **placar** no canto superior direito: uma linha por ser/objeto já salvo, **desenho à esquerda**, **número de salvamentos à direita**, cumulativo na sessão, só itens já salvos (nada de ícone com zero). A linha nasce no primeiro salvamento e não se move. No celular, ampliado **1,5×**.

É a **única** exceção nominal às leis L3 (zero texto) e L8 (nada de números). Nenhum outro número ou letra entra.

| Jogo | Registro |
| --- | --- |
| Teia do Murilo | `murilo/docs/adr/0004-placar-e-inversao-de-lado.md` (entrou na v1.4; desde então é padrão) |
| Força do Murilo | `murilo/hulk/docs/adr/0004-placar-desde-a-v1.md` (nasce com o placar) |
| Veloz do Murilo | `murilo/sonic/docs/adr/0004-placar-desde-a-v1.md` (nasce com o placar) |
| Raio do Murilo | `murilo/super/docs/adr/0004-placar-desde-a-v1.md` (nasce com o placar) |

## 2. Tela cheia no celular no primeiro toque

No celular, o jogo entra em tela cheia **no primeiro toque** — sem duplo-toque, sem gesto especial, sem instrução.

A API de fullscreen exige gesto do usuário. Então:

1. **No carregamento**, se o ponteiro primário é grosso (`(pointer: coarse)`), o jogo **tenta** entrar em tela cheia (funciona em PWA / "Adicionar à Tela de Início" e onde o navegador permitir sem gesto).
2. **No primeiro `pointerdown` de toque**, se ainda não estiver em tela cheia, entra. O mesmo toque continua sendo jogada (resgate/socorro) — não é gasto com o gesto de tela cheia.
3. **Desktop (mouse)** continua com a tecla `F`. Mouse não alterna tela cheia.
4. Onde a API não existe (iPhone), o gesto é **inócuo** — nada quebra, nenhum diálogo aparece. O caminho é "Adicionar à Tela de Início".

| Jogo | Registro |
| --- | --- |
| Teia do Murilo | `murilo/docs/adr/0006-tela-cheia-no-primeiro-toque.md` (v1.6; substitui o duplo-toque da v1.4) |
| Força do Murilo | `murilo/hulk/docs/adr/0005-tela-cheia-no-primeiro-toque.md` |
| Veloz do Murilo | `murilo/sonic/docs/adr/0005-tela-cheia-no-primeiro-toque.md` |
| Raio do Murilo | `murilo/super/docs/adr/0005-tela-cheia-no-primeiro-toque.md` |
