# Placar desde a v1.0

Padrão da casa (`../../PADRAES.md`). Todo jogo nasce com o placar: canto superior direito, desenho do brinquedo à esquerda, número à direita, cumulativo na sessão, só itens já guardados, 1,5× no celular. NF4 nasce em **240**. Nenhum outro número entra. A caixa **não** é um segundo placar e **não** mostra dígitos.

Idêntico ao Raio do Murilo, porque este jogo é de propósito o mesmo com outro herói.

## Status

accepted (2026-11-03)

## Consequences

- L3/L8 têm a exceção nominal no primeiro dia.
- Com três brinquedos por cenário × três cenários, o placar tem no máximo **nove linhas**.
- `registrarGuarda()` dispara em `entrar(S.GUARDA)`.
- A caixa é encarnação do progresso, não um contador.
