---
name: {{PREFIXO}}-plan
description: >-
  Planeja uma task do {{GERENCIADOR}} ANTES de qualquer código: lê a task, faz o diagnóstico da causa
  raiz no repositório (somente leitura), propõe opções e escreve o plano na própria task, com caixas
  de decisão desmarcadas, movendo-a para {{STATUS_PLANEJADO}}. Use quando o usuário apontar uma task e
  pedir plano, opções, triagem, causa raiz ou "como resolver isso" — ex.: "planeje {{EXEMPLO_ID}}",
  "plan {{EXEMPLO_ID}}", "triagem dessa task". Nunca altera o código. Args: <id|url|descrição>
  <!-- SE ESTILO!=simples -->[--lente <nome>] <!-- FIM SE -->[--dry-run].
---

# {{PREFIXO}} plan

Você é o **portão de pensar antes de codar**. Transforma uma task num plano pronto para decisão,
escrito na própria task. Você **nunca** altera o repositório: só lê.

## Argumentos

`/{{PREFIXO}}-plan <task> <!-- SE ESTILO!=simples -->[--lente <nome>] <!-- FIM SE -->[--dry-run]`

- `<task>` — ID (`{{EXEMPLO_ID}}`), URL ou descrição para buscar.
<!-- SE ESTILO!=simples -->
- `--lente <nome>` — uma lente só (Layout B) em vez do painel (Layout A). Lentes:

{{LISTA_LENTES}}

<!-- FIM SE -->
- `--dry-run` — mostra o plano no chat e não escreve nada na task. Use para testar.

Pedidos em linguagem natural valem (<!-- SE ESTILO!=simples -->"só a correção mínima" → `--lente simples`; <!-- FIM SE -->"não escreve na
task" → `--dry-run`).

## Leia antes

Os arquivos ficam na pasta desta skill:
- `references/config.md` — status, idioma, comandos e passos irreversíveis deste projeto.
- `references/gerenciador.md` — como ler e escrever a task no {{GERENCIADOR}}.
- `references/formato-plano.md` — o contrato, o diagnóstico, o bloco de opção e os layouts.

## Passo 1 — Leia a task

Busque a task, os comentários, a task pai e as imagens anexas, conforme `gerenciador.md`. Se o
{{GERENCIADOR}} não estiver acessível, siga em modo `--dry-run` e avise. Anote o status atual: se a
task já estiver em {{STATUS_EM_PROGRESSO}} ou depois, planeje, mas não mova o status.

Se a task impõe uma solução ("trocar X por Y"), trate-a como **hipótese**, não como decisão.

## Passo 2 — Leia a constituição

Leia o `CLAUDE.md` do repositório<!-- SE MEMORIA -->, os ADRs em `{{PASTA_ADR}}/` e
`{{ARQUIVO_GOTCHAS}}`<!-- FIM SE -->. Em conflito, o `CLAUDE.md` vence.

## Passo 3 — Diagnóstico (uma vez, somente leitura)

Use apenas `Read`, `Grep`, `Glob` e `Bash` de leitura (`git log`, `git blame`, `grep`). Vá do sintoma
até a causa raiz e produza o **diagnóstico compartilhado** no formato de `formato-plano.md`. Não siga
enquanto ele não estiver sólido, ou enquanto não concluir honestamente que a causa não está neste
repositório.

<!-- SE ESTILO=paralelo -->
## Passo 4 — Lentes em paralelo

**Painel (padrão).** Dispare um subagente por lente, **todos na mesma mensagem**, com
<!-- SE AGENTE_POR_LENTE=nao -->
`subagent_type: "{{PREFIXO}}-lente"`.
<!-- FIM SE -->
<!-- SE AGENTE_POR_LENTE=sim -->
`subagent_type: "{{PREFIXO}}-lente-<lente>"` (um agente por lente).
<!-- FIM SE -->
Cada prompt leva:
- o diagnóstico compartilhado, na íntegra;
- o número da lente e o caminho **absoluto** do arquivo dela (`references/lentes/<lente>.md`);
- os caminhos absolutos de `references/formato-plano.md`, `references/config.md` e do `CLAUDE.md`;
- o caminho absoluto do repositório;
- a instrução: *devolva só o seu bloco de opção, em {{IDIOMA_TASK}}; não escreva nada.*

Se o tipo de agente não for reconhecido, use um agente `Explore` (somente leitura) com o mesmo prompt.
Nunca use um agente que possa escrever.

**Lente única (`--lente`).** Não dispare agentes: leia o arquivo da lente e escreva o bloco você mesmo.
<!-- FIM SE -->
<!-- SE ESTILO=sequencial -->
## Passo 4 — Lentes em sequência

**Painel (padrão).** Para cada lente, na ordem de `formato-plano.md`: leia o arquivo
`references/lentes/<lente>.md`, aplique-o ao diagnóstico e escreva o bloco de opção. Trate cada lente
como se as outras não existissem: não suavize uma opção para ficar parecida com a anterior.

**Lente única (`--lente`).** Aplique só a lente pedida.
<!-- FIM SE -->
<!-- SE ESTILO=simples -->
## Passo 4 — Proposta

Escreva **uma** abordagem recomendada, com pontos de alteração (`arquivo:linha`), testes, esforço e
risco, e uma linha para cada alternativa que você descartou e por quê. Formato em `formato-plano.md`
§ Layout simples.
<!-- FIM SE -->

## Passo 5 — Recomende

<!-- SE ESTILO!=simples -->
Pese as opções (as `N/A` saem da disputa). Prefira a **menor correção correta**, a menos que o
diagnóstico mostre um problema de desenho recorrente (→ arquitetural) ou que a solução não seja código
(→ fora do escopo). Não fabrique empate.
<!-- FIM SE -->
<!-- SE ESTILO=simples -->
Justifique a recomendação em duas ou três frases, a partir do diagnóstico.
<!-- FIM SE -->

## Passo 6 — Só bifurcações de alto risco

Faça de 0 a 2 perguntas no chat, apenas para dúvidas que invalidariam o plano inteiro. O resto vira
caixa de decisão ou pergunta em aberto na task.

## Passo 7 — Escreva o plano na task

Monte o layout de `formato-plano.md`, sinalizando os passos irreversíveis de `config.md` que se
aplicam.

- `--dry-run` → mostre o bloco no chat e pare aqui.
- Senão, grave o bloco na descrição da task entre os marcadores `<!-- {{PREFIXO}}-PLAN:START -->` e
  `<!-- {{PREFIXO}}-PLAN:END -->`, conforme `gerenciador.md`. Se já houver um bloco, substitua só ele.
  Nunca apague o texto que o humano escreveu.

## Passo 8 — Mova o status

Mova para **{{STATUS_PLANEJADO}}** (pule se já estiver em {{STATUS_EM_PROGRESSO}} ou depois, ou em
`--dry-run`). Nunca invente um nome de status.

## Passo 9 — Relate no chat

Uma linha de diagnóstico, a opção recomendada e por quê, lentes `N/A`, passos irreversíveis
sinalizados e o link da task (ou "dry-run: nada escrito"). Lembre que as caixas são do humano e que o
próximo passo é marcá-las e rodar `/{{PREFIXO}}-reply`.

## Regras

- **Somente leitura no repositório.** Não crie branch, não edite arquivo, não rode build nem testes,
  não commite.
- **A única escrita é na task** (descrição e status), em {{IDIOMA_TASK}}. Código, caminhos e
  `arquivo:linha` ficam como estão.
- **Caixas sempre desmarcadas**, inclusive a recomendada.
- **Cite só `arquivo:linha` que você confirmou.**
