# Configuração do projeto

Valores que as skills {{PREFIXO}}-plan, {{PREFIXO}}-reply e {{PREFIXO}}-implement usam. Edite aqui
quando o fluxo mudar; as skills leem este arquivo a cada execução. Campo com `PREENCHER` ainda não foi
definido: a skill avisa e pergunta antes de depender dele.

## Gerenciador
- Ferramenta: {{GERENCIADOR}}
- Exemplo de ID: `{{EXEMPLO_ID}}`
- Magic word no commit: `{{MAGIC_WORD}}` (troque `<ID>` pelo ID da task)

## Status
| Momento | Status |
| --- | --- |
| Depois do plan | {{STATUS_PLANEJADO}} |
| Implement começa (lock) | {{STATUS_EM_PROGRESSO}} |
| Implement termina | {{STATUS_REVISAO}} |

## Idioma
- Texto escrito na task: {{IDIOMA_TASK}}
- Chat: o idioma do usuário

## Repositório
- Branch padrão: `{{BRANCH_PADRAO}}`
- Build: `{{COMANDO_BUILD}}`
- Testes: `{{COMANDO_TESTES}}`
<!-- SE MEMORIA -->
- ADRs: `{{PASTA_ADR}}/`
- Gotchas: `{{ARQUIVO_GOTCHAS}}`
<!-- FIM SE -->

## Passos irreversíveis (sinalizar e parar)
{{PASSOS_IRREVERSIVEIS}}
