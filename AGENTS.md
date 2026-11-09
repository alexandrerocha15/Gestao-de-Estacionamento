# Instruções para agents

## Geral

- Sempre utilizar o sistema de questões do ambiente (harness) ao invés de fazer perguntas na resposta.

## Fluxo de trabalho obrigatório

Toda mudança deve seguir, obrigatoriamente, esta ordem:

1. **PRD (Product Requirements Document):** definir e aprovar o problema, os objetivos, o escopo e os requisitos do produto.
2. **TDD (Technical Design Document):** definir e aprovar as decisões técnicas, a arquitetura e o roadmap de implementação com base no PRD.
3. **SDD (Spec-Driven Development com OpenSpec):** criar ou atualizar, revisar e validar as especificações e tarefas de implementação no OpenSpec, mantendo rastreabilidade explícita com os requisitos do PRD e as decisões do TDD. A implementação só pode começar depois que essas especificações e tarefas estiverem completas, coerentes e aprovadas.
4. **Pull Request:** após a aprovação do PRD e do TDD e a validação do SDD, implementar as tarefas especificadas e abrir o PR usando `.github/pull_request_template.md`.

Regras:

- Não inverter nem pular etapas.
- Cada artefato deve referenciar os documentos anteriores aplicáveis.
- Antes de criar um artefato, verificar se já existe um documento ou uma especificação correspondente; quando existir, reutilizá-lo e atualizá-lo em vez de criar uma nova versão ou arquivo desnecessariamente.
- Na etapa SDD, nenhuma implementação deve começar antes da criação ou atualização, revisão e validação das especificações e tarefas rastreáveis ao PRD e ao TDD.
- Alterações de escopo ou decisões que invalidem um documento aprovado devem atualizar esse documento antes de avançar.
- O Pull Request deve demonstrar a rastreabilidade entre requisitos, decisões, design, implementação e testes.
