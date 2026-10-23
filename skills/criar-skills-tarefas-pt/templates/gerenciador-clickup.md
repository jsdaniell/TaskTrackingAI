# Gerenciador — ClickUp

Use as ferramentas do conector/MCP do ClickUp disponíveis na sessão (procure por "clickup"). Se
nenhuma existir, trabalhe em `--dry-run` e avise. Workspace: `{{ID_WORKSPACE}}` (ID numérico; passe-o em
toda chamada que aceitar workspace).

## Ler a task
- Busque a task pelo ID (`{{EXEMPLO_ID}}`) ou pela URL. Leia nome, descrição em markdown, status,
  responsáveis, task pai, subtasks, comentários e anexos.

## Escrever o bloco
1. Leia a descrição em markdown **atual** logo antes de escrever.
2. Se os marcadores do bloco já existem, substitua só o trecho entre eles; senão, acrescente no fim.
3. Grave a descrição em markdown. Nunca apague o texto do humano.

> Depois de gravar, leia de novo e confira que os marcadores e as caixas `- [ ]` foram preservados.

## Status, comentário e link
- Mova o status pelo **nome exato** de `config.md` (o ClickUp diferencia status por lista). Se não
  existir na lista da task, mostre os disponíveis e pergunte.
- Comentários: use a ferramenta de comentário da task.
- Link de código: inclua o ID da task na branch e no commit (`{{MAGIC_WORD}}` (com o ID da task)); com a integração do
  GitHub ligada, o ClickUp mostra o commit e o PR na task.
