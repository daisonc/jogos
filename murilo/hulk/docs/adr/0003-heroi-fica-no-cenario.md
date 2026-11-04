# O herói fica no cenário — não há VOLTA

A Teia tem o estado `VOLTA` porque o Homem-Aranha balança de volta ao ponto de partida. O Hulk não tem motivo para voltar andando, e teleportar com fumaça quebraria a fisicalidade que faz o estrondo crível. Decidimos: **o herói pousa e fica**. O próximo Obstáculo nasce do outro lado (contador, sem `Math.random`) e ele salta de onde está. A ação completa (~2,6 s) não gasta 600 ms com retorno; a Gratidão acontece no lugar do rompimento. O custo aceito é perder a previsibilidade do loop fechado da Teia — o herói migra pela cena. Em troca, o jogo tem um diferencial de loop que não é skin, e o lado do próximo salto dá variedade sem espelhar a tela inteira (a inversão de lado da Teia v1.4 fica de fora).

## Status

accepted (2026-11-03)

## Considered Options

- **VOLTA ao ponto de partida** (igual Teia): rejeitada. Teleporte ou caminhada sem graça.
- **Some e reaparece** no início: rejeitada. Quebra a fisicalidade.
- **Fica onde pousou; próximo obstáculo do outro lado**: adotada.

## Consequences

- A FSM não tem `VOLTA`; tem `GRATIDÃO` no lugar, no ponto de pouso.
- `jogo.lado` alterna o lado do próximo Obstáculo, não o espelhamento do cenário.
- O layout precisa garantir que o próximo alvo esteja sempre a uma distância de salto (nada de alvo fora de alcance — L1).
- A posição do herói é contínua entre etapas, como na Teia, mas o ponto de espera muda a cada resgate.
