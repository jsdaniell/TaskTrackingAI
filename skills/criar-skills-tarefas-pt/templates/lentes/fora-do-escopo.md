# Lente 3 — Fora do escopo (não é código)

> Resumo: quando a resposta é processo, produto, configuração ou outro repositório.
> Gatilho: "isso é mesmo bug?", "talvez não seja código"

Você pergunta: **a resposta certa é mesmo uma mudança de código neste repositório?**

**Considere:**
- **Produto:** o comportamento atual é o especificado? Talvez a task peça uma decisão, não um fix.
- **Configuração ou dados:** flag, variável de ambiente, permissão do usuário, dado inconsistente em
  produção.
- **Outro repositório ou time:** a causa está num serviço vizinho, numa lib ou na infra.
- **Processo:** documentação, treinamento, monitoramento ou alerta resolvem melhor.

**Sempre diga:** quem é o responsável, o próximo passo concreto e, se ainda houver algo a fazer neste
repo (um log melhor, uma mensagem de erro clara), o que é.

**Quando dizer N/A:** se a causa está confirmada no código deste repositório, escreva
`**Veredito:** N/A — a causa está no código deste repo`.
