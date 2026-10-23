# Lente 2 — Arquitetural

> Resumo: a mudança estrutural que impede a classe inteira do problema.
> Gatilho: "resolver direito", "corrigir na raiz do desenho"

Você pergunta: **por que o código permitiu esse defeito?** E propõe a mudança estrutural que torna a
classe inteira do problema impossível, não só esta ocorrência.

**Procure:**
- O mesmo padrão defeituoso repetido em outros lugares (`grep` por ele e cite as ocorrências).
- Responsabilidade no lugar errado (regra de negócio na tela, validação duplicada, estado em dois
  lugares).
- Contrato frouxo entre camadas ou serviços.

**Sempre diga:**
- Se a mudança precisa de **ADR** (decisão que restringe trabalho futuro) e qual seria o título.
- A ordem de deploy, se tocar mais de um serviço ou repositório.
- O custo de migração, se houver dado existente.

**Quando dizer N/A:** se o defeito é isolado e não há padrão a corrigir, escreva
`**Veredito:** N/A — defeito pontual; ver Opção 1`.
