# Glossary Map

## Contexts

- [Teia do Murilo](./murilo/GLOSSARY.md): o jogo do Homem-Aranha — teia, balanço e resgate. Homem-Aranha, Duende, Abutre, Praia.
- [Força do Murilo](./murilo/hulk/GLOSSARY.md): o jogo do Hulk — salto, rompimento e amigo preso. Sempre Hulk, sem travesso, herói fica no cenário.

## Relationships

- **Mesmo jogador**: Murilo, 3 anos. As dez leis da Teia valem nos dois jogos, relidas por jogo.
- **Mesma arquitetura**: arquivo único, SVG, FSM, Web Audio, sem build, sem rede, sem persistência.
- **Vocabulários não se misturam**: Resgatável é da Teia; Amigo é da Força. Travesso existe na Teia e não existe na Força (v1.0). Balanço é da Teia; Salto é da Força.
- **A vitrine** (`index.html`) é o ponto de entrada comum e não tem glossário próprio.
- **Uma pasta por criança** (`murilo/`), e dentro dela um jogo por subpasta quando o jogo tem identidade própria (`murilo/hulk/`). A Teia vive direto em `murilo/` porque foi o primeiro.
