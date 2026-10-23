# Contrato e formato do plano

As regras que todo planejamento segue e os formatos exatos que ele escreve na task.

## O contrato

1. **Somente leitura no repositório.** Investigar e planejar. Nunca editar arquivo, criar branch,
   commitar, rodar build ou testes, nem fazer deploy.
2. **A única escrita é na task:** um bloco delimitado na descrição e o status.
3. **Diagnóstico uma vez, até a causa raiz, com evidência**, antes de qualquer solução.
4. **Honestidade sobre o encaixe.** Uma lente que não se aplica diz `N/A` e por quê, em uma linha.
5. **O `CLAUDE.md` do repositório é a constituição.** Em conflito com estes arquivos, ele vence.

> **Idioma.** Tudo o que vai para a task fica em {{IDIOMA_TASK}}. Código, caminhos, `arquivo:linha`,
> identificadores e os marcadores HTML não são traduzidos. O chat segue o idioma do usuário.

## O diagnóstico compartilhado

```
### Diagnóstico
- **Sintoma:** <o que se observa, e onde (tela, endpoint, comando)>
- **Causa raiz:** <o defeito real, em uma ou duas frases>
- **Evidências:**
  - `<caminho>:LINHA` — <o que esta linha faz de errado>
- **Raio de impacto:** <o que mais toca o mesmo código ou dado>
- **Confiança:** <alta | média | baixa> — <o que aumentaria, se não for alta>
```

Se a causa não está neste repositório (outro repo, infra, decisão de produto) ou ainda não dá para
saber, diga isso claramente. Nunca invente `arquivo:linha`.

<!-- SE ESTILO!=simples -->
## O bloco de opção

Cada lente escreve exatamente um bloco:

```
### Opção <N> — <nome da lente> ⟨aplica-se | N/A⟩
**Veredito:** <uma linha: por que esta lente serve aqui, ou por que não>
**Abordagem:** <a correção concreta, nos termos e padrões do repo>
**Pontos de alteração:**
- `<caminho>:LINHA` — <o que muda aqui>
- <os testes a criar ou alterar>
**Esforço / risco:** esforço <P/M/G>, risco <baixo/médio/alto> — <por quê, em uma linha>
**Passos irreversíveis:** <nenhum | os de config.md que se aplicam>
**Trade-off vs. as outras:** <a frase que o revisor precisa para escolher>
```

Uma lente `N/A` ainda escreve o bloco: `**Veredito:** N/A — <por quê>` e o resto em uma linha
("Não se aplica; ver Opção X.").

Ordem fixa:

{{LISTA_LENTES}}

## Layout A — painel de opções (padrão)

```
<!-- {{PREFIXO}}-PLAN:START -->
## 🧭 Plano (somente leitura — nenhum código alterado)
_Gerado por {{PREFIXO}}-plan · <id da task>_

<diagnóstico>

### Opções
<um bloco por lente, na ordem fixa>

### Recomendação
**⭐ Opção <N> — <nome>.** <2–3 frases a partir do diagnóstico.>

### Perguntas em aberto
- <pergunta> → R:

## Decisão (marque uma — caixas desmarcadas de propósito)
{{CAIXAS_LENTES}}
- [ ] Nenhuma destas — precisa de discussão (comente na task)

### ⚠️ Passos irreversíveis sinalizados
<nenhum, ou os de config.md que se aplicam — para um humano executar>
<!-- {{PREFIXO}}-PLAN:END -->
```

Marque também o título da opção recomendada com `⭐ RECOMENDADA`.

## Layout B — lente única (`--lente`)

```
<!-- {{PREFIXO}}-PLAN:START -->
## 🧭 Plano (somente leitura — nenhum código alterado)
_Gerado por {{PREFIXO}}-plan --lente <lente> · <id da task>_

<diagnóstico>

### Recomendação — <nome da lente>
<o bloco de opção>

**Outras lentes:**
- <lente>: <uma linha de por que não, ou "se aplica → rode o painel completo">

## Decisão (marque uma — desmarcada de propósito)
- [ ] Seguir com esta opção
- [ ] Prefiro o painel completo (rodar sem --lente)
- [ ] Nenhuma destas — precisa de discussão (comente na task)

### ⚠️ Passos irreversíveis sinalizados
<nenhum, ou os que se aplicam>
<!-- {{PREFIXO}}-PLAN:END -->
```
<!-- FIM SE -->
<!-- SE ESTILO=simples -->
## Layout simples

```
<!-- {{PREFIXO}}-PLAN:START -->
## 🧭 Plano (somente leitura — nenhum código alterado)
_Gerado por {{PREFIXO}}-plan · <id da task>_

<diagnóstico>

### Abordagem recomendada
<a correção concreta>
**Pontos de alteração:**
- `<caminho>:LINHA` — <o que muda>
- <testes a criar ou alterar>
**Esforço / risco:** esforço <P/M/G>, risco <baixo/médio/alto>

### Alternativas descartadas
- <alternativa>: <por que não, em uma linha>

### Perguntas em aberto
- <pergunta> → R:

## Decisão (desmarcada de propósito)
- [ ] Seguir com a abordagem recomendada
- [ ] Prefiro uma alternativa (diga qual num comentário)
- [ ] Precisa de discussão

### ⚠️ Passos irreversíveis sinalizados
<nenhum, ou os que se aplicam>
<!-- {{PREFIXO}}-PLAN:END -->
```
<!-- FIM SE -->

## Caixas de decisão

- Todas saem **desmarcadas**, inclusive a recomendada. Marcar é um ato humano.
- Sempre inclua a saída "Nenhuma destas / precisa de discussão".

## O plano decidido (escrito pela reply)

```
<!-- {{PREFIXO}}-DECIDIDO:START -->
## ✅ Plano decidido
_Concluído por {{PREFIXO}}-reply · <data>_

**Escolha:** <opção marcada>
**Abordagem:** <especificada por completo, com as respostas incorporadas>

### O que mudou em relação às opções
- <o que as respostas alteraram; "nada" se nada>

### Passos de implementação
1. `<caminho>:LINHA` — <o que fazer>

### Testes a criar
- <cenário>

### ⚠️ Passos irreversíveis (não executar automaticamente)
- <passo> — responsável: <humano>
<!-- {{PREFIXO}}-DECIDIDO:END -->
```
