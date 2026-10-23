# TaskTrackingAI

**IA orientada a tarefas:** a task do gerenciador (Linear, ClickUp, Jira, GitHub Issues) é a fonte da
verdade, do diagnóstico ao deploy. O agente planeja e registra na task, o humano decide, e só então o
código é escrito.

Este repositório reúne o material da apresentação e um plugin do Claude Code com a skill
`criar-skills-tarefas-pt`, que gera as **suas próprias** skills de `plan`, `reply` e `implement` para o
seu projeto. Ela pergunta o gerenciador, o estilo de planejamento (quatro agentes de lente em paralelo,
quatro lentes num agente só ou um plano simples) e se cada lente também vira skill ou agente.

## Material

- [A task como fonte da verdade](A%20task%20como%20fonte%20da%20verdade.pdf) — apresentação
- [Guia de criação de tarefas](guia-criacao-de-tarefas.pdf) — versão completa
- [Guia de criação de tarefas (resumo)](guia-criacao-de-tarefas-short.pdf) — duas páginas

## Instalar a skill

No Claude Code:

```
/plugin marketplace add jsdaniell/TaskTrackingAI
/plugin install tarefas-ia@tasktracking-ai
```

Depois, no repositório do seu projeto: `/tarefas-ia:criar-skills-tarefas-pt`

## Repositório

<img src="assets/qrcode-repositorio.png" alt="QR code para https://github.com/jsdaniell/TaskTrackingAI" width="200">

https://github.com/jsdaniell/TaskTrackingAI
