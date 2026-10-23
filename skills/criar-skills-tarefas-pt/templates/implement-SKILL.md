---
name: {{PREFIXO}}-implement
description: >-
  Implementa o plano decidido de uma task do {{GERENCIADOR}}: lê o bloco "Plano decidido" escrito pelo
  {{PREFIXO}}-reply, escreve o código e os testes seguindo o CLAUDE.md, roda build e testes até ficar
  verde, linka o trabalho à task, comenta o resumo na task e move o status. Use quando o plano estiver
  decidido e o usuário pedir para construir — "implementar {{EXEMPLO_ID}}", "implement {{EXEMPLO_ID}}",
  "codar o plano decidido". Sinaliza e para antes de qualquer passo irreversível (deploy, migração,
  secrets). Não serve para planejar ({{PREFIXO}}-plan) nem para concluir um plano sem decisão
  ({{PREFIXO}}-reply).
---

# {{PREFIXO}} implement

Só aqui o agente escreve código. Você segue o plano decidido; não o reinventa.

## Leia antes

Na pasta da skill plan (`../{{PREFIXO}}-plan/references/`): `config.md` e `gerenciador.md`. No
repositório: `CLAUDE.md`<!-- SE MEMORIA -->, os ADRs citados no plano e `{{ARQUIVO_GOTCHAS}}`<!-- FIM SE -->.

## Passo 1 — Busque o plano decidido

Busque a task e ache `<!-- {{PREFIXO}}-DECIDIDO:START -->`.
- **Sem plano decidido** → pare. Se houver só o plano com caixas, indique `/{{PREFIXO}}-reply`; se não
  houver nada, `/{{PREFIXO}}-plan`.
- **O plano decidido diz que não é código neste repo** → pare e relate.

## Passo 2 — Prepare o terreno

- Confira que a árvore de trabalho está limpa (`git status`). Se não estiver, pergunte antes de
  seguir.
<!-- SE NIVEL_GIT!=arquivos -->
- Crie uma branch a partir de `{{BRANCH_PADRAO}}` com o ID da task no nome, em minúsculas e sem `#`
  (ex.: `<id>-resumo-curto`).
<!-- FIM SE -->
- Mova a task para **{{STATUS_EM_PROGRESSO}}** e atribua-a a quem pediu. Esse status é o "lock":
  outra sessão ou outro agente não pega uma task que já está nele.

## Passo 3 — Implemente

Siga os passos do plano decidido na ordem. Use os padrões do `CLAUDE.md`. Se o código atual não bater
com o plano (arquivo mudou, função sumiu), adapte o mínimo necessário e registre isso no resumo. Se a
divergência mudar a abordagem, **pare** e pergunte.

## Passo 4 — Testes

Escreva os testes listados em "Testes a criar". Rode:

- build: `{{COMANDO_BUILD}}`
- testes: `{{COMANDO_TESTES}}`

Repita até ficar verde. Não desative nem apague teste para passar. Se não conseguir deixar verde,
pare e relate o que falha, com a saída.

<!-- SE MEMORIA -->
## Passo 5 — Memória institucional

- O plano decidido pede ADR → crie `{{PASTA_ADR}}/NNNN-<titulo>.md` (próximo número livre), com
  Status, Contexto, Decisão, Consequências e Task.
- Você descobriu uma causa não óbvia (sintoma enganoso, armadilha de API, configuração) → acrescente
  uma entrada em `{{ARQUIVO_GOTCHAS}}` com Sintoma, Causa e Task.
<!-- FIM SE -->

## Passo 6 — Linke o trabalho à task

<!-- SE NIVEL_GIT=commit -->
Commite na branch com uma mensagem que termine com `{{MAGIC_WORD}}`, trocando `<ID>` pelo ID desta
task. **Não faça push** — diga ao
usuário que a branch está pronta para revisão.
<!-- FIM SE -->
<!-- SE NIVEL_GIT=pr -->
Commite na branch com uma mensagem que termine com `{{MAGIC_WORD}}` (`<ID>` = ID desta task), faça push da branch e abra o PR
com o link da task na descrição. **Nunca faça merge** — o merge é do humano.
<!-- FIM SE -->
<!-- SE NIVEL_GIT=arquivos -->
Não commite. Deixe as alterações na árvore de trabalho para o usuário revisar e diga que o commit deve
terminar com `{{MAGIC_WORD}}` (`<ID>` = ID desta task) para linkar a task.
<!-- FIM SE -->

## Passo 7 — Comente e mova

Comente na task, em {{IDIOMA_TASK}}: o que foi feito, arquivos alterados, testes criados e o
resultado, divergências do plano, o link do commit/PR, e os passos irreversíveis que ficaram para um
humano. Mova para **{{STATUS_REVISAO}}**.

## Passo 8 — Relate no chat

Resumo curto, resultado dos testes, branch/PR, e a lista de passos irreversíveis pendentes.

## Nunca execute (sinalize e pare)

{{PASSOS_IRREVERSIVEIS}}

Se um passo do plano cair nesta lista, implemente o código que o prepara, mas não o execute: registre
no comentário da task quem precisa executá-lo e como.

## Regras

- Siga o plano decidido; não abra escopo novo. Achou outro problema? Sugira uma task nova no
  comentário.
- Uma task por vez, numa branch própria.
- Não faça merge na `{{BRANCH_PADRAO}}` nem deploy.
