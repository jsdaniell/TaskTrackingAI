# Gerenciador — GitHub Issues

Use o `gh` (confira `gh auth status`; se falhar, trabalhe em `--dry-run` e avise).

## Ler a task
- `gh issue view <número> --json number,title,body,state,labels,assignees,comments,url`
- Se receber só uma descrição: `gh issue list --search "<texto>"` e confirme com o usuário.

## Escrever o bloco
1. Leia o corpo **atual** logo antes de escrever.
2. Se os marcadores do bloco já existem, substitua só o trecho entre eles; senão, acrescente no fim.
3. Grave com `gh issue edit <número> --body-file <arquivo temporário>`. Nunca apague o texto do humano.

## Status, comentário e link
- O GitHub Issues não tem status além de aberta/fechada. Use **labels** com os nomes de `config.md`
  (`gh issue edit <n> --add-label "<status>" --remove-label "<anterior>"`) ou um campo de status do
  GitHub Projects, se o projeto usar. Se a label não existir, pergunte antes de criar.
- Comentários: `gh issue comment <número> --body-file <arquivo>`.
- `{{MAGIC_WORD}}` (com o ID da task) no commit ou na descrição do PR fecha a issue no merge na branch padrão.
