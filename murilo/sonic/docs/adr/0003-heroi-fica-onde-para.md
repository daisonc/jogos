# O herói fica onde para

Igual ao Hulk (`murilo/hulk/docs/adr/0003-heroi-fica-no-cenario.md`), diferente da Teia. O Sonic não volta correndo ao ponto de partida: **pousa e fica**. O próximo amigo nasce do outro lado (contador, sem `Math.random`) e ele dispara de onde está. A novidade do Sonic é o disparo, não o retorno. A FSM não tem `VOLTA`; tem `GRATIDÃO` no ponto de chegada.

## Status

accepted (2026-11-03)

## Considered Options

- **Correr de volta** (igual Teia): rejeitada. Duplicaria o disparo e alongaria a ação.
- **Fica onde para; próximo do outro lado**: adotada.

## Consequences

- Sem estado `VOLTA`.
- `jogo.lado` alterna o lado do próximo amigo.
- Distância de disparo sempre entre 600 e 900 px; nenhum alvo fora de alcance (L1).
