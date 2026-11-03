# Quarto cenário: Praia

O non-goal "novos cenários" existe desde a v1.0, quando a disciplina protegia o escopo de três cenários. Na v1.1 ele foi reafirmado explicitamente (§6.3 do PRD). Decidimos abrir **uma** exceção na v1.3: a Praia entra como quarto cenário do ciclo, com o mesmo loop, o mesmo aim assist e a mesma festa. A razão é que o cenário é a única forma de novidade que não pede do Murilo nenhuma habilidade nova — um lugar diferente depois da terceira festa é surpresa sem regra nova, que é exatamente o que o `TEMA.md` chama de variedade. O custo aceito é quebrar um non-goal registrado e repor a disciplina com um teto explícito de nós (≤ 140), com a proibição de um quinto cenário e com o registro desta decisão.

## Status

accepted (2026-11-03)

## Considered Options

- **Manter três cenários.** Rejeitada: o prazer de repetição já está confortável, e o cenário é a novidade mais barata em risco (não toca a mecânica).
- **Adicionar também um quinto cenário.** Rejeitada: o quinto reabre o escopo que esta decisão fecha; a exceção é única e nominal.
- **Trocar um dos três cenários por praia.** Rejeitada: removeria conteúdo já observado com o Murilo sem ganho.

## Consequences

- O número de cenários passa de ímpar (3) para par (4). A alternância de travesso só por rodada (`rodada % 2`) fixaria um travesso por cenário (telhados/rua sempre com o Duende, parque/praia sempre com o Abutre). A escolha passa a somar a volta: `(rodada + floor(rodada / CENARIOS.length)) % 2`, determinística e sem `if` por cenário.
- A Praia carrega orçamento próprio: ≤ 140 nós SVG (folga ≥ 10 sobre o teto de 150), com ≤ 80 animados por frame.
- O gradiente do céu da Praia vive dentro do template do cenário, e não no `defs` global, para não somar nós às outras cenas.
- Um quinto cenário permanece fora de escopo; nova exceção exigiria novo ADR.
