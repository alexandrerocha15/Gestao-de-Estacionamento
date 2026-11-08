---
description: "Gera um Technical Design Document (TDD) com entrevista de decisões arquiteturais, linguagem, frameworks e bibliotecas, incluindo roadmap de implementação com fases"
---

Carregue a skill `generate-tdd` (ferramenta `skill`, id exato `generate-tdd`) e siga as instruções dela à risca para produzir um TDD completo e pronto para revisão.

A skill conduz uma **entrevista de decisões técnicas obrigatória** usando a ferramenta `question` (sistema de questões do ambiente). Decisões de arquitetura/estrutura, linguagem, runtime, frameworks, bibliotecas, dados, integrações, segurança, testes e deploy devem ser esclarecidas com o usuário antes de finalizar o artefato. Todas as decisões ficam centralizadas em um registro `DEC-XXX` no documento.

Inclua **diagramas Mermaid** sempre que ajudarem a explicar relacionamentos, fluxos ou estados: contexto, containers/componentes, entidades (ER ou classes), sequência dos fluxos críticos, estados e implantação. Cada diagrama deve ter legenda e uma explicação curta, seguindo `references/mermaid-guide.md` da skill.

Inclua **obrigatoriamente a seção de Roadmap de implementação** com as fases da implementação, cada fase com entregável, dependências e critérios de aceite verificáveis. O roadmap é parte integrante do TDD, não um apêndice opcional, e deve derivar-se das decisões (`DEC-XXX`) e requisitos (`RF/RNF`) registrados.

Prompt inicial e contexto fornecido pelo usuário:

$ARGUMENTS

Se o prompt acima estiver vazio, não invente um sistema: use a skill para perguntar ao usuário qual sistema, módulo ou mudança técnica o TDD deve cobrir.

Não implemente código nesta tarefa; o entregável é apenas o TDD, por padrão em `docs/tdd/tdd.md`.
