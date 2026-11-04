# Glossary Map

## Contexts

- [Teia do Murilo](./murilo/GLOSSARY.md): o jogo do Homem-Aranha — teia, balanço e resgate. Homem-Aranha, Duende, Abutre, Praia.
- [Força do Murilo](./murilo/hulk/GLOSSARY.md): o jogo do Hulk — salto, rompimento e amigo preso. Sempre Hulk, sem travesso, herói fica no cenário.
- [Veloz do Murilo](./murilo/sonic/GLOSSARY.md): o jogo do Sonic — disparo, borrão e amigo preso. Sempre Sonic, Eggman de longe, herói fica onde para.
- [Raio do Murilo](./murilo/super/GLOSSARY.md): o jogo do Super Homem — raio dos olhos, brinquedo na caixa. Sempre Super Homem, sem vilão, herói nunca pousa.
- [Aranha do Murilo](./murilo/aranha/GLOSSARY.md): o jogo do Homem-Aranha no chão — teia no brinquedo, caixa no chão. Sempre Homem-Aranha, sem vilão, herói nunca sai do chão.

## Relationships

- **Mesmo jogador**: Murilo, 3 anos. As dez leis da Teia valem nos dois jogos, relidas por jogo.
- **Mesma arquitetura**: arquivo único, SVG, FSM, Web Audio, sem build, sem rede, sem persistência.
- **Vocabulários não se misturam**: Resgatável é da Teia; Amigo é da Força e do Veloz; Brinquedo é do Raio e do Aranha. Travesso existe na Teia (Duende, Abutre) e no Veloz (Eggman); o Raio e o Aranha não têm vilão. Balanço é da Teia; Salto é da Força; Disparo é do Veloz; Raio é do Raio; Teia/Estica/Gruda são do Aranha. Rompimento é da Força; Abertura é do Veloz; Coleta/Guarda é do Raio e do Aranha.
- **Dois Homens-Aranha, verbos diferentes**: a Teia balança e resgata; o Aranha fica no chão e recolhe brinquedos com a teia. Não confundir Resgate (Teia) com Coleta (Aranha).
- **A vitrine** (`index.html`) é o ponto de entrada comum e não tem glossário próprio.
- **Uma pasta por criança** (`murilo/`), e dentro dela um jogo por subpasta quando o jogo tem identidade própria (`murilo/hulk/`). A Teia vive direto em `murilo/` porque foi o primeiro.
