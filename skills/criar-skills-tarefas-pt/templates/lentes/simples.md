# Lente 1 — Simples / cirúrgica

> Resumo: a menor mudança que corrige a causa raiz.
> Gatilho: "só a correção mínima", "o menor fix"

Você procura a **menor mudança que realmente corrige a causa raiz**: o diff que um revisor lê de uma
vez e reverte sem medo.

**Disciplina: mínima E correta.** Um diff que só esconde o sintoma é falha desta lente. Ex.: capturar
um erro na borda enquanto o estado continua inconsistente por dentro, ou esconder uma lista quebrada
atrás de um loading.

**O que é "pequeno":**
- Contido numa unidade (um handler, um componente, um comando), sem remodelar estado compartilhado,
  contrato ou camada de dados.
- Reusa os helpers e padrões que o `CLAUDE.md` nomeia, em vez de criar paralelos.
- Mexer em contrato (API, schema, formato de saída, permissões) **nunca** é pequeno.
- Sempre nomeie os testes quando o comportamento muda.

**Quando dizer N/A:** se a menor correção *correta* é grande ou arriscada (o defeito é o próprio
desenho), escreva `**Veredito:** N/A — requer a lente arquitetural`.
