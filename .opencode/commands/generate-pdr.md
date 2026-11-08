---
description: "Gera um PRD completo a partir de um prompt inicial, com entrevista de clarificação antes de finalizar"
---

Carregue a skill `generate-pdr` (ferramenta `skill`, id exato `generate-pdr`) e siga as instruções dela à risca para produzir um PRD completo e pronto para revisão.

A skill conduz uma **entrevista de clarificação obrigatória** usando a ferramenta `question` (sistema de questões do ambiente). Especificações ambíguas devem ser esclarecidas com o usuário antes de finalizar o artefato.

Prompt inicial e contexto fornecido pelo usuário:

$ARGUMENTS

Se o prompt acima estiver vazio, não invente um produto: use a skill para perguntar ao usuário qual ideia, necessidade ou funcionalidade o PRD deve cobrir.
