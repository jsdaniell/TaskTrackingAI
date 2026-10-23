---
name: criar-skills-tarefas-pt
description: >-
  Gera, por entrevista, as SUAS PRÓPRIAS skills de IA orientada a tarefas — plan (planejar),
  reply (concluir a decisão) e implement (implementar) — com a task do gerenciador (Linear,
  ClickUp, Jira, GitHub Issues) como fonte da verdade. Pergunta o estilo de planejamento
  (quatro agentes de lente em paralelo, quatro lentes num agente só, ou um plano simples),
  se cada lente também vira skill ou agente próprio, onde instalar e os detalhes do fluxo
  (status, testes, passos irreversíveis, ADRs e gotchas), e então escreve os arquivos.
  Use quando o aluno disser "criar minhas skills de plan/reply/implement", "montar meu fluxo
  de tarefas com IA", "quero skills de planejamento para o meu projeto", "gerar skills como
  as da aula" ou chamar /criar-skills-tarefas-pt. Primeira versão em português (pt-BR).
---

# Criar skills de IA orientada a tarefas (pt-BR)

Você ajuda um aluno a montar o próprio pipeline **planejar → decidir → implementar**, no qual a
task é o único lugar onde o problema vive. Você não executa o fluxo: você **gera as skills** que o
executam, adaptadas ao projeto e às escolhas do aluno.

Converse em **português (pt-BR)**, a menos que o aluno escreva em outro idioma.

## Princípios que toda skill gerada mantém

Estes vêm do material da aula e não são opcionais. Os modelos já os trazem; não os remova.

1. **A task é a fonte da verdade.** Diagnóstico, opções, decisão e resumo da implementação são
   escritos na task. Nada depende do chat.
2. **Plan e reply nunca tocam no código.** Somente leitura no repositório; a única escrita é na task
   (texto e status).
3. **A IA propõe, o humano decide.** As caixas de decisão saem sempre desmarcadas, inclusive a
   recomendada.
4. **Nem a implementação executa o irreversível.** Deploy, migração, schema em produção, secrets e
   publicação em loja são sinalizados e param para um humano.
5. **Diagnóstico antes de opção.** Causa raiz com evidência (`arquivo:linha`) antes de qualquer
   solução.

## Passo 1 — Olhe o projeto antes de perguntar

Se houver um repositório no diretório atual, leia (somente leitura) o que responder perguntas
sozinho: `CLAUDE.md`, `README`, manifestos (`package.json`, `go.mod`, `Package.swift`, `pyproject.toml`,
`Cargo.toml`…), scripts de teste e CI, e o `git remote`. Anote: stack, comando de testes, comando de
build, se há `docs/adr/` ou `docs/gotchas.md`, e qual gerenciador aparece em commits
(`Fixes ABC-12`, `#123`, IDs do ClickUp).

Não pergunte o que já descobriu; confirme no resumo do Passo 3.

## Passo 2 — Entrevista

Leia `references/opcoes.md`: ele tem cada pergunta, as alternativas e o que cada resposta muda nos
arquivos gerados. Faça as perguntas com `AskUserQuestion`, no máximo quatro por chamada, em duas
rodadas:

- **Rodada 1 (estrutura):** gerenciador de tarefas, estilo de planejamento, como as lentes ficam
  disponíveis, onde instalar.
- **Rodada 2 (detalhes):** só o que o Passo 1 não respondeu — prefixo das skills, nomes dos status,
  idioma de saída na task, memória institucional (ADRs e gotchas), o que a implementação pode fazer
  sozinha (commit, push, PR).

Se o aluno disser "use o padrão" ou "tanto faz", aplique os padrões marcados em `opcoes.md` e siga.

## Passo 3 — Confirme o desenho

Antes de escrever qualquer arquivo, mostre um resumo curto:

- as skills que serão criadas, com os nomes finais (ex.: `danfit-plan`, `danfit-reply`,
  `danfit-implement`, e as de lente, se escolhidas);
- os agentes que serão criados, se houver;
- a pasta de destino de cada arquivo;
- as escolhas que mudam o comportamento (estilo, status, o que a implementação faz sozinha, passos
  irreversíveis).

Peça um "ok". Se algum arquivo de destino já existir, diga qual e pergunte se pode sobrescrever.

## Passo 4 — Gere os arquivos

Os modelos estão em `templates/`. Copie cada um para o destino, substituindo os marcadores `{{…}}`
pelas respostas (cada marcador está descrito em `references/opcoes.md`, junto da pergunta que o
define). Nenhum `{{` pode sobrar.

Os modelos têm blocos condicionais, que podem estar aninhados e também aparecer no meio de uma linha:

- `<!-- SE X=valor -->` … `<!-- FIM SE -->` — mantenha o conteúdo só se a escolha X for `valor`.
- `<!-- SE X!=valor -->` … `<!-- FIM SE -->` — mantenha só se X **não** for `valor`.
- `<!-- SE MEMORIA -->` … `<!-- FIM SE -->` — mantenha só se a memória institucional estiver ligada.

Em todos os casos, apague os próprios comentários `SE` / `FIM SE` do arquivo gerado. Quando um
comentário `SE` ou `FIM SE` ocupa a linha sozinho, apague a linha inteira (não deixe linha em branco
no lugar). Os marcadores
`<!-- …-PLAN:START -->` e `<!-- …-DECIDIDO:START -->` **não** são condicionais: ficam no arquivo.

| Modelo | Destino (`<skills>` = pasta escolhida) | Quando |
| --- | --- | --- |
| `templates/plan-SKILL.md` | `<skills>/{{PREFIXO}}-plan/SKILL.md` | sempre |
| `templates/config.md` | `<skills>/{{PREFIXO}}-plan/references/config.md` | sempre |
| `templates/formato-plano.md` | `<skills>/{{PREFIXO}}-plan/references/formato-plano.md` | sempre |
| `templates/gerenciador-<nome>.md` | `<skills>/{{PREFIXO}}-plan/references/gerenciador.md` | sempre (um só, o escolhido) |
| `templates/lentes/*.md` | `<skills>/{{PREFIXO}}-plan/references/lentes/` | só as lentes escolhidas |
| `templates/reply-SKILL.md` | `<skills>/{{PREFIXO}}-reply/SKILL.md` | sempre |
| `templates/implement-SKILL.md` | `<skills>/{{PREFIXO}}-implement/SKILL.md` | sempre |
| `templates/lente-SKILL.md` | `<skills>/{{PREFIXO}}-plan-<lente>/SKILL.md` | lentes como skills |
| `templates/agente-lente.md` | `<agents>/{{PREFIXO}}-lente.md` | estilo paralelo e `AGENTE_POR_LENTE=nao` |
| `templates/agente-lente.md` | `<agents>/{{PREFIXO}}-lente-<lente>.md` | estilo paralelo e `AGENTE_POR_LENTE=sim` (sem o genérico) |
| `templates/memoria-adr.md` | `docs/adr/0000-modelo.md` no repositório | memória ligada e a pasta não existe |
| `templates/memoria-gotchas.md` | `docs/gotchas.md` no repositório | memória ligada e o arquivo não existe |

Pastas por destino de instalação (`references/opcoes.md`, pergunta 4):

| Instalação | `<skills>` | `<agents>` |
| --- | --- | --- |
| Projeto | `.claude/skills` | `.claude/agents` |
| Usuário | `~/.claude/skills` | `~/.claude/agents` |
| Plugin próprio | `<pasta-do-plugin>/skills` | `<pasta-do-plugin>/agents` |

Para **plugin próprio**, crie também `.claude-plugin/plugin.json` e `.claude-plugin/marketplace.json`
na pasta do plugin, seguindo o formato de `references/opcoes.md` § Plugin próprio. Num plugin, as
skills e os agentes ganham o nome do plugin na frente; depois de gerar, troque nos arquivos gerados:

- `/{{PREFIXO}}-` (chamadas de skill) por `/{{PREFIXO}}-tarefas:{{PREFIXO}}-`;
- `subagent_type: "{{PREFIXO}}-lente…"` por `subagent_type: "{{PREFIXO}}-tarefas:{{PREFIXO}}-lente…"`.

As skills geradas apontam umas para as outras pelo caminho relativo a partir da pasta da skill
(`../{{PREFIXO}}-plan/references/…`). Não use caminhos absolutos da máquina do aluno.

## Passo 5 — Verifique

Rode estas checagens e corrija o que falhar antes de entregar:

1. Nenhum `{{` sobrou: `grep -rn '{{' <arquivos gerados>`.
2. Nenhum bloco `<!-- SE` sobrou.
3. Cada `SKILL.md` gerado começa com frontmatter válido: `name` igual ao nome da pasta (minúsculas,
   números e hífens, até 64 caracteres) e `description` com até 1024 caracteres.
4. Cada arquivo das skills geradas citado por outra skill gerada existe no caminho citado (arquivos do
   repositório, como `CLAUDE.md` e `docs/`, ficam fora desta checagem).
5. Cada agente gerado tem `name`, `description` e `tools: Read, Grep, Glob, Bash` (somente leitura).

## Passo 6 — Entregue

Diga ao aluno, em poucas linhas:

- o que foi criado e onde (links para os arquivos);
- que precisa **reiniciar a sessão** do Claude Code (ou rodar `/reload-plugins` no caso de plugin)
  para as skills aparecerem;
- que o gerenciador precisa estar conectado por MCP (ver `references/opcoes.md` § Gerenciadores);
- o primeiro teste seguro: `/<prefixo>-plan <id da task> --dry-run`, que mostra o plano no chat sem
  escrever na task;
- o ciclo completo: plan → marcar as caixas na task → reply → implement.

## O que você não faz

- Não roda as skills geradas nem escreve em tasks reais durante a criação.
- Não faz commit nem push dos arquivos gerados, a menos que o aluno peça.
- Não inventa comandos de teste ou status: se não descobriu e o aluno não disse, deixe o campo como
  `PREENCHER` no `config.md` e avise no Passo 6.
