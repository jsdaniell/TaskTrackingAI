# Opções da entrevista

Cada pergunta traz as alternativas, o padrão (⭐) e o que a resposta muda nos arquivos. Use os textos
das alternativas como `label`/`description` do `AskUserQuestion`, com a padrão em primeiro lugar e
"(Recomendado)" no fim do label.

---

## Rodada 1 — estrutura

### 1. Gerenciador de tarefas

| Alternativa | Modelo usado | Observação |
| --- | --- | --- |
| Linear ⭐ | `templates/gerenciador-linear.md` | Magic word `Fixes ABC-12` fecha a issue no merge. |
| ClickUp | `templates/gerenciador-clickup.md` | Pede o ID do workspace → `{{ID_WORKSPACE}}`. |
| Jira | `templates/gerenciador-jira.md` | Pede a chave do projeto (`ABC`) → `{{CHAVE_PROJETO}}`. |
| GitHub Issues | `templates/gerenciador-github.md` | Usa `gh`; `Fixes #12` fecha a issue. |

"Outro" (texto livre): use `templates/gerenciador-github.md` como base e troque as operações pelas
ferramentas MCP que o aluno disser que tem. Se ele não tiver MCP nem CLI para o gerenciador, avise que
as skills vão funcionar só em `--dry-run` (plano no chat) até conectar.

Marcadores: `{{GERENCIADOR}}` (nome), `{{EXEMPLO_ID}}` (só para exemplos e gatilhos; ex.: `DAN-42`,
`86aftvp9c`, `ABC-7`, `#42`).

### 2. Estilo de planejamento

| Alternativa | O que a skill plan faz | Custo |
| --- | --- | --- |
| Quatro agentes em paralelo ⭐ | Diagnóstico uma vez; depois dispara um subagente por lente, todos na mesma mensagem, e junta as opções. | Mais tokens, mais rápido, opções mais independentes. |
| Quatro lentes num agente só | Mesmo painel de opções, mas a própria skill aplica cada lente em sequência. | Mais barato; as opções tendem a se parecer mais. |
| Plano simples | Um diagnóstico, uma recomendação e uma linha por alternativa descartada. Sem lentes. | O mais barato. Bom para começar ("planejamento simples primeiro"). |

Marcador: `{{ESTILO}}` = `paralelo` | `sequencial` | `simples`. No `plan-SKILL.md`, mantenha só o
bloco `<!-- SE ESTILO=… -->` correspondente. Com `simples`, não gere `templates/lentes/`, as skills de
lente nem os agentes, e pule as perguntas 3 e 6.

### 3. Como as lentes ficam disponíveis (multiSelect)

| Alternativa | Gera |
| --- | --- |
| Opção `--lente` na skill plan ⭐ | Sempre incluída. `/<prefixo>-plan <task> --lente simples` roda uma lente só. |
| Cada lente como skill própria | `<prefixo>-plan-simples`, `<prefixo>-plan-arquitetural`, … (atalhos que chamam a plan com `--lente`). Aparecem no menu `/`. |
| Cada lente como agente próprio | Um arquivo de agente por lente (`<prefixo>-lente-simples`, …), visíveis em `/agents` e editáveis um a um. Só faz sentido com estilo paralelo. |

Se não marcar "agente próprio" e o estilo for paralelo, gere **um** agente genérico
(`<prefixo>-lente`) que recebe o arquivo da lente no prompt.

Marcadores: `{{AGENTE_POR_LENTE}}` = `sim` | `nao`.

Marcadores por lente, usados em `lente-SKILL.md` e `agente-lente.md` (um arquivo gerado por lente):

| Marcador | Valor | Exemplo |
| --- | --- | --- |
| `{{LENTE}}` | nome do arquivo da lente, sem `.md` | `fora-do-escopo` |
| `{{NOME_LENTE}}` | coluna "Nome na task" | `Fora do escopo (não é código)` |
| `{{RESUMO_LENTE}}` | a linha `> Resumo:` do arquivo da lente | `quando a resposta é processo…` |
| `{{GATILHO_LENTE}}` | a primeira frase de `> Gatilho:`, sem as aspas | `isso é mesmo bug?` |
| `{{NOME_AGENTE}}` | `<prefixo>-lente` (genérico) ou `<prefixo>-lente-<lente>` | `danfit-lente-simples` |
| `{{NOME_LENTE_AGENTE}}` | `do painel` (genérico) ou o "Nome na task" | `Simples / cirúrgica` |

Com um agente por lente, acrescente ao fim do agente gerado: "Sua lente é sempre `<lente>`. O
orquestrador passa o caminho absoluto do arquivo dela no prompt."

### 4. Onde instalar

| Alternativa | Pastas | Para quem |
| --- | --- | --- |
| Neste projeto ⭐ | `.claude/skills`, `.claude/agents` | Vai junto com o repo; o time inteiro usa. |
| No meu usuário | `~/.claude/skills`, `~/.claude/agents` | Vale em todos os projetos da máquina. |
| Plugin próprio | uma pasta nova (pergunte o caminho) | Para publicar no GitHub e outras pessoas instalarem. |

---

## Rodada 2 — detalhes (só o que o Passo 1 não respondeu)

### 5. Prefixo das skills
Texto curto, minúsculas e hífens (ex.: nome do projeto: `danfit`, `pontogo`). Padrão ⭐: o nome da pasta
do repositório, normalizado. Marcador: `{{PREFIXO}}`. Com o prefixo, os nomes finais são
`{{PREFIXO}}-plan`, `{{PREFIXO}}-reply`, `{{PREFIXO}}-implement`.

### 6. Quais lentes (multiSelect)

| Lente | Arquivo | Nome na task |
| --- | --- | --- |
| Simples ⭐ | `lentes/simples.md` | Simples / cirúrgica |
| Arquitetural ⭐ | `lentes/arquitetural.md` | Arquitetural |
| Fora do escopo ⭐ | `lentes/fora-do-escopo.md` | Fora do escopo (não é código) |
| Balanceada ⭐ | `lentes/balanceada.md` | Balanceada / pragmática |

O aluno pode descrever uma lente nova em texto livre (ex.: "segurança", "performance"). Crie
`lentes/<nome>.md` no mesmo formato das outras, a partir da descrição dele. Numere as lentes na ordem
da tabela, com as novas no fim. O nome da lente em todo lugar (marcadores, caixas, título do arquivo) é
a coluna **Nome na task**.

- `{{LISTA_LENTES}}` — lista numerada, sem recuo, uma linha por lente:
  `N. **<Nome na task>** — \`--lente <arquivo sem .md>\` — arquivo \`references/lentes/<arquivo>\``
  (caminho relativo à pasta da skill plan).
- `{{CAIXAS_LENTES}}` — uma linha `- [ ] Opção N — <Nome na task>` por lente.

### 7. Status do fluxo
Pergunte os nomes exatos dos status no gerenciador. Padrão ⭐ (Linear): `Planned` → `In Progress` →
`In Review`. Marcadores: `{{STATUS_PLANEJADO}}` (para onde a plan move), `{{STATUS_EM_PROGRESSO}}`
(a implement move ao começar), `{{STATUS_REVISAO}}` (a implement move ao terminar).

### 8. Idioma de saída na task
Português (pt-BR) ⭐ ou inglês. Marcador: `{{IDIOMA_TASK}}` = `português (pt-BR)` | `inglês (en)`. O chat segue o idioma do usuário.

### 9. Memória institucional
| Alternativa | Efeito |
| --- | --- |
| ADRs e gotchas ⭐ | reply registra ADR quando a decisão é estrutural; implement registra gotcha quando descobre causa não óbvia. |
| Só ADRs | mantém os blocos `<!-- SE MEMORIA -->`, mas tira deles as menções a `{{ARQUIVO_GOTCHAS}}` e o item de gotcha da implement; não cria `docs/gotchas.md`. |
| Nenhuma | remove os blocos `<!-- SE MEMORIA -->`. |

Marcadores: `{{PASTA_ADR}}` (padrão `docs/adr`), `{{ARQUIVO_GOTCHAS}}` (padrão `docs/gotchas.md`).

### 10. O que a implement faz sozinha
| Alternativa | Efeito |
| --- | --- |
| Commit local com a task linkada ⭐ | Cria branch, commita com `{{MAGIC_WORD}}`, não faz push. |
| Commit + push + PR | Também faz push da branch e abre o PR (nunca faz merge). |
| Só altera os arquivos | Não commita; deixa o diff para o aluno revisar. |

Marcador: `{{NIVEL_GIT}}` = `commit` | `pr` | `arquivos`. `{{MAGIC_WORD}}` é um **padrão** com `<ID>`,
que vem do gerenciador: `Fixes <ID>` (Linear, Jira), `Fixes #<ID>` (GitHub), `<ID>` (ClickUp). Nunca
fixe um ID real nele.

### Descobertos do projeto (Passo 1) ou perguntados
- `{{COMANDO_TESTES}}` — ex.: `npm test`, `go test ./...`, `xcodebuild test …`.
- `{{COMANDO_BUILD}}` — ou `nenhum`.
- `{{PASSOS_IRREVERSIVEIS}}` — lista do que a implement sinaliza e não executa. Sempre inclua: deploy,
  migração ou mudança de schema em produção, drop de índice ou backfill, alteração de secrets ou
  permissões, publicação em loja. Acrescente o que o aluno citar.
- `{{BRANCH_PADRAO}}` — padrão `main`.
- `{{AUTOR}}` — só para plugin próprio: `git config user.name`; se vazio, pergunte.

---

## Gerenciadores — o que o aluno precisa ter conectado

| Gerenciador | Conexão |
| --- | --- |
| Linear | Conector/MCP oficial do Linear. |
| ClickUp | Conector/MCP oficial do ClickUp. |
| Jira | Conector Atlassian (Jira) por MCP. |
| GitHub Issues | `gh` autenticado (`gh auth status`). |

Sem conexão, as skills geradas funcionam só com `--dry-run`.

---

## Plugin próprio

`<pasta>/.claude-plugin/plugin.json`:

```json
{
  "name": "{{PREFIXO}}-tarefas",
  "version": "0.1.0",
  "description": "Skills de plan, reply e implement de {{PREFIXO}}.",
  "author": { "name": "{{AUTOR}}" }
}
```

`<pasta>/.claude-plugin/marketplace.json`:

```json
{
  "name": "{{PREFIXO}}-marketplace",
  "owner": { "name": "{{AUTOR}}" },
  "plugins": [
    { "name": "{{PREFIXO}}-tarefas", "source": "./", "description": "Skills de plan, reply e implement." }
  ]
}
```

Depois de publicar a pasta num repositório GitHub, a instalação é:

```
/plugin marketplace add <usuario>/<repositorio>
/plugin install {{PREFIXO}}-tarefas@{{PREFIXO}}-marketplace
```

Num plugin, as skills ficam com o nome do plugin na frente (`/{{PREFIXO}}-tarefas:{{PREFIXO}}-plan`)
e os agentes também (`{{PREFIXO}}-tarefas:{{PREFIXO}}-lente…`). O `SKILL.md` (Passo 4) diz o que
trocar nos arquivos gerados.
