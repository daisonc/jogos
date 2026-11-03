# Placar de resgates e inversão de lado

Na v1.4 o operador pediu duas features novas. A segunda — **inverter o lado em que o herói começa a cada salvamento** — é aditiva: não fere nenhuma lei e só amplia o vocabulário de cenário. A primeira — um **placar** no canto superior direito com o desenho do resgatável salvo e o **número** de salvamentos — contradiz de frente a lei **L8** ("nada de números … nunca contado com dígitos") e o non-goal registrado "Placar, recorde, pontuação, ranking, progresso salvo", além de tocar **L3** ("zero texto") por introduzir os primeiros caracteres do jogo (dígitos). O pedido é explícito e justificado pelo operador: o Murilo não lê, então o placar usa o **desenho** do item para identificação e o número como contagem. Decidimos **implementar as duas** e registrar a exceção, em vez de recusar a feature. A mitigação é de escopo: o placar é só de resgates (nada de recorde, ranking, pontuação por tempo ou progresso salvo), é cumulativo na sessão, mostra apenas os itens já salvos, é determinístico e é reversível. Diferente da exceção da Praia (`ADR 0003`), aqui a exceção é **permanente e nominal**: nenhum outro número entra no jogo.

## Status

accepted (2026-11-03)

## Considered Options

- **Recusar o placar e manter L8/L3 intactas.** Rejeitada: é o operador (o dono do produto) pedindo, com justificativa de acessibilidade; recusar trocaria uma lei por um produto que não atende ao pedido.
- **Placar sem dígitos (marcas/ícones repetidos).** Rejeitada pelo operador: o número é parte do pedido ("o número de salvamentos").
- **Placar de pontuação/recorde.** Rejeitada: reabriria o non-goal inteiro em vez de uma exceção nominal.
- **Não mostrar itens ainda não salvos.** Aceita: mantém o placar enxuto e evita linhas com "0".

## Consequences

- **L8 passa a ter uma exceção nominal** (o placar da v1.4), registrada no `TEMA.md`. **L3** passa a valer para palavras/letras; dígitos existem apenas no placar.
- **NF4 (≤ 150 nós SVG) é rebaselinado.** O placar reaproveita a arte real de cada resgatável (é o desenho que o Murilo reconhece), então soma nós: o pico medido passa de **149** para **240** (oito itens já salvos). Os nós do placar são **estáticos** — não entram no limite de ≤ 80 animados/frame nem no frame time (NF3), porque nada neles anima. Alternativa de ícones pictográficos próprios foi descartada por risco de o item deixar de ser reconhecível em 200 ms.
- **Determinismo preservado (NF11).** A contagem é por dicionário de ids e a inversão é por contador (`jogo.lado ^= 1`) — nenhum `Math.random`.
- **Inversão de lado espelha o cenário inteiro** em torno de `x = W/2`, inclusive a rota do travesso; sem espelhar a rota, ele apareceria a menos de 60% da largura do herói e violaria **L7/US4**. Verificado: `|herói − travesso| = 1170 px` nos dois lados (telhados/rua), 1220 (parque), 1200 (praia).
- **A volta (VOLTA) muda de papel.** Como o herói já virou para o lado novo no resgate, a volta deixa de ser a travessia de retorno e passa a ser um reposicionamento curto no mesmo lado. O balanço (travessia) segue com a mesma geometria; a diferença é o herói **pingar** de um lado para o outro a cada ciclo.
- **Reversível.** Remover o bloco do placar (constante `PLACAR`, funções `montarPlacar`/`atualizarFundoPlacar`/`adicionarLinhaPlacar`/`registrarResgate`, `<g id="placar">` e a chamada em `entrar(S.RESGATE)`) e a inversão (`jogo.lado`, `espelharLayout`/`layoutAtual` e a linha `jogo.lado ^= 1`) restaura a v1.3 sem tocar no loop, no aim assist ou nos tempos.
