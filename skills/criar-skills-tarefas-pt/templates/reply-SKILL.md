---
name: {{PREFIXO}}-reply
description: >-
  Fecha o ciclo de planejamento de uma task do {{GERENCIADOR}}: depois que um humano marcou as caixas
  de decisão e respondeu as perguntas no plano do {{PREFIXO}}-plan, lê essas escolhas, revisa o caminho
  decidido no código (somente leitura) e escreve UM bloco "Plano decidido" na task, pronto para
  implementar. Use quando o usuário disser que decidiu — "concluir o plano {{EXEMPLO_ID}}", "marquei
  as caixas, conclua", "reply {{EXEMPLO_ID}}". Nunca altera o código. Não serve para planejar do zero
  ({{PREFIXO}}-plan) nem para implementar ({{PREFIXO}}-implement).
---

# {{PREFIXO}} reply — concluir o plano decidido

O plan mostrou opções; o humano marcou caixas e respondeu perguntas. Você transforma isso em **um
plano decidido na task**, para quem for implementar ver uma resposta só, não um cardápio. O valor
está na **reconciliação**: as escolhas do humano costumam refinar ou contradizer parte da proposta, e
dizer isso claramente é o ponto.

## Leia antes

Na pasta da skill plan (`../{{PREFIXO}}-plan/references/`): `config.md`, `gerenciador.md` e
`formato-plano.md` (§ O contrato e § O plano decidido).

## Passo 1 — Busque a task e o plano

Busque a task e os comentários. Ache o bloco `<!-- {{PREFIXO}}-PLAN:START -->`.
- **Sem bloco de plano** → não há o que concluir; indique `/{{PREFIXO}}-plan`.
- **Já existe um bloco `{{PREFIXO}}-DECIDIDO`** → é uma nova rodada; você substitui só esse bloco.

## Passo 2 — Leia as decisões

De todos os lugares onde podem estar:
- **Caixas** — `- [x]` ou `- [X]` marcadas; `- [ ]` não.
- **Respostas inline** — texto depois de `R:`, `>` ou abaixo de uma pergunta.
- **Comentários** da task.

## Passo 3 — Valide antes de concluir

- **Nenhuma opção marcada**, **caixas em conflito** ou **pergunta bloqueante sem resposta** → pergunte
  no chat só isso e continue.
- **Escolheram uma opção `N/A`** ou as respostas contradizem a abordagem → diga claramente e confirme;
  ofereça rodar o plan de novo se a decisão reabrir o problema.
- Tudo claro → siga. Não rediscuta o que foi decidido.

## Passo 4 — Revise com a decisão travada (somente leitura)

- **Incorpore as respostas** na abordagem.
- **Diga o que mudou** — se uma resposta invalida um passo da proposta, registre em "O que mudou".
- **Confirme os `arquivo:linha`** no código atual (o código pode ter mudado desde o plano).
- **Leve adiante os testes** e os **passos irreversíveis** de `config.md` que se aplicam.
<!-- SE MEMORIA -->
- **Decisão estrutural?** Inclua no plano decidido o passo "registrar ADR em `{{PASTA_ADR}}/`" com
  o título da decisão. Quem escreve o arquivo é a implement.
<!-- FIM SE -->
- **Não é código neste repo** → o plano decidido diz isso, nomeia o responsável e o próximo passo.

## Passo 5 — Escreva o plano decidido

Monte o bloco de `formato-plano.md` § O plano decidido, em {{IDIOMA_TASK}}, e grave-o na descrição
entre `<!-- {{PREFIXO}}-DECIDIDO:START -->` e `<!-- {{PREFIXO}}-DECIDIDO:END -->`. Mantenha o bloco do
plano original (é o histórico das opções); não apague o texto do humano. Com `--dry-run`, só mostre no
chat.

## Passo 6 — Relate no chat

A escolha, o que mudou, os passos irreversíveis e o link da task. Próximo passo:
`/{{PREFIXO}}-implement <id>`.

## Regras

- **Somente leitura no repositório.** A única escrita é na task.
- **Não decida pelo humano.** Se faltar decisão, pergunte.
- **Não mova o status** — quem move para {{STATUS_EM_PROGRESSO}} é a implement.
