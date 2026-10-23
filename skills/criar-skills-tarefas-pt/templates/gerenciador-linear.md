# Gerenciador — Linear

Use as ferramentas do conector/MCP do Linear disponíveis na sessão (os nomes exatos variam; procure
por "linear" nas ferramentas). Se nenhuma existir, trabalhe em `--dry-run` e avise.

## Ler a task
- Busque a issue pelo identificador (`{{EXEMPLO_ID}}`) ou pela URL. Se receber só uma descrição,
  pesquise e confirme com o usuário qual é.
- Leia título, descrição, status, responsável, issue pai, sub-issues, comentários e anexos.

## Escrever o bloco
1. Leia a descrição **atual** logo antes de escrever (alguém pode ter editado).
2. Se os marcadores `<!-- …:START -->` / `<!-- …:END -->` do bloco já existem, substitua só o trecho
   entre eles. Senão, acrescente o bloco no fim da descrição.
3. Grave a descrição inteira de volta. Nunca apague o texto do humano.

> O Linear guarda a descrição em markdown e pode não preservar comentários HTML. Depois de gravar,
> leia de novo: se os marcadores sumiram, use no lugar um título fixo (`## 🧭 Plano` /
> `## ✅ Plano decidido`) como delimitador, até o próximo título de mesmo nível.

## Status, comentário e link
- Mova o status pelo **nome exato** de `config.md`. Se não existir no time, liste os disponíveis e
  pergunte.
- Comentários: use a ferramenta de comentário da issue.
- Commit com `{{MAGIC_WORD}}` (com o ID da task) linka a issue e a fecha no merge na branch padrão (se a integração do
  GitHub/GitLab estiver ligada).
