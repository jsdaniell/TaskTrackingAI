# Gerenciador — Jira

Use as ferramentas do conector/MCP da Atlassian (Jira) disponíveis na sessão. Se nenhuma existir,
trabalhe em `--dry-run` e avise. Projeto: `{{CHAVE_PROJETO}}`.

## Ler a task
- Busque a issue pela chave (`{{EXEMPLO_ID}}`) ou pela URL. Leia resumo, descrição, status,
  responsável, épico/pai, subtarefas, comentários e anexos.

## Escrever o bloco
1. Leia a descrição **atual** logo antes de escrever.
2. Se os marcadores do bloco já existem, substitua só o trecho entre eles; senão, acrescente no fim.
3. Grave a descrição. Nunca apague o texto do humano.

> O Jira pode converter markdown para o formato dele e perder comentários HTML e caixas. Depois de
> gravar, leia de novo. Se os marcadores sumiram, use títulos fixos como delimitador; se as caixas
> sumiram, escreva as opções como lista numerada e peça a decisão num comentário ("Decisão: 2").

## Status, comentário e link
- Mude o status pela **transição** cujo destino tem o nome de `config.md`. Se não houver, liste as
  transições disponíveis e pergunte.
- Comentários: use a ferramenta de comentário da issue.
- Link de código: a chave da issue na branch e no commit (`{{MAGIC_WORD}}` (com o ID da task)) liga o commit à issue
  quando a integração com o repositório está ativa.
