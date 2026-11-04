# O herói voa — nunca pousa; alterna de lado

Igual aos irmãos no alterna de lado (`murilo/hulk/docs/adr/0003-heroi-fica-no-cenario.md`, `murilo/sonic/docs/adr/0003-heroi-fica-onde-para.md`), diferente de todos no **nunca pousar**. O Super Homem paira o tempo todo: bob suave, capa ondulando. O brinquedo que viaja até a caixa, não o herói até o brinquedo. A FSM não tem `VOLTA`, não tem pouso, não tem derrapagem. O próximo brinquedo nasce do outro lado (contador, sem `Math.random`) e ele vira, atira, guarda.

## Status

accepted (2026-11-03)

## Considered Options

- **Voar até o brinquedo e pousar**: rejeitada. Duplicaria o voo e alongaria a ação; o pedido é o raio, não o voo de busca.
- **Sempre no centro, sem alterna de lado**: rejeitada. Os irmãos alternam de lado e o Murilo já lê esse lado.
- **Paira; alterna de lado; o brinquedo que viaja**: adotada.

## Consequences

- Sem estado de pouso, sem poeira de chegada, sem derrapagem.
- `jogo.lado` alterna o lado do próximo brinquedo.
- Distância do raio sempre entre 600 e 900 px; nenhum alvo fora de alcance (L1).
- O bob de voo é contínuo, inclusive em `ESPERANDO`.
