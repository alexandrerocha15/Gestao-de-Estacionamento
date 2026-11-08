# PRD - <Nome do Produto ou Funcionalidade>

| Campo | Valor |
| --- | --- |
| Versão | 1.0 |
| Status | Rascunho / Rascunho - pendente de clarificação / Em revisão / Aprovado |
| Autor | <nome ou "Agente OpenCode"> |
| Data | <AAAA-MM-DD> |
| Stakeholders | <áreas/pessoas envolvidas> |
| Links | <entrevistas, protótipos, specs, issues> |

## 1. Resumo executivo

Em 3 a 6 linhas: o que é, para quem, qual problema resolve e qual resultado de negócio é esperado.

## 2. Contexto e problema

- **Situação atual:** como o problema é resolvido hoje.
- **Problema:** dores concretas, evidências e impactos.
- **Oportunidade:** por que agir agora.
- **Hipótese de valor:** "Se <solução>, então <público> conseguirá <resultado>, medido por <métrica>."

## 3. Objetivos e métricas de sucesso

| ID | Objetivo | Métrica | Baseline | Meta | Como medir |
| --- | --- | --- | --- | --- | --- |
| OBJ-001 | | | | | |

Inclua objetivos de negócio, de usuário e, quando aplicável, técnicos.

## 4. Público-alvo e personas

| Persona | Perfil | Necessidades | Dores | Contexto de uso |
| --- | --- | --- | --- | --- |
| | | | | |

Liste também perfis secundários e quem **não** é público-alvo.

## 5. Escopo

### 5.1 Dentro do escopo

- ...

### 5.2 Fora do escopo

- ...

### 5.3 Premissas

- ...

### 5.4 Restrições

- Técnicas, legais, operacionais, orçamentárias e de prazo.

## 6. Jornadas e fluxos principais

Descreva a jornada ponta a ponta de cada persona e os fluxos alternativos/erros.

```text
[Estado inicial] --> [Ação] --> [Resultado esperado]
                       |
                       +--> [Erro/exceção] --> [Tratamento]
```

## 7. Requisitos funcionais

| ID | Requisito | Prioridade | Persona | Critérios de aceitação |
| --- | --- | --- | --- | --- |
| RF-001 | | Essencial | | Dado ... quando ... então ... |

Regras:
- Um requisito por linha, com comportamento observável.
- Use MoSCoW: Essencial, Importante, Desejável, Fora do escopo.
- Critérios de aceitação devem ser verificáveis por teste ou inspeção.

## 8. Requisitos não funcionais

| ID | Categoria | Requisito | Métrica/Critério | Prioridade |
| --- | --- | --- | --- | --- |
| RNF-001 | Desempenho | | | |

Cubra, quando aplicável: desempenho, escalabilidade, disponibilidade, confiabilidade, segurança, privacidade, acessibilidade, usabilidade, compatibilidade, observabilidade, manutenibilidade e limites operacionais.

## 9. Regras de negócio

| ID | Regra | Condição | Comportamento | Exceções |
| --- | --- | --- | --- | --- |
| RN-001 | | | | |

## 10. Modelo de dados conceitual

Entidades, atributos principais, relacionamentos e ciclo de vida. Sem esquema físico de banco, salvo decisão já confirmada.

```text
[Entidade A] 1 --- N [Entidade B]
```

## 11. Integrações e dependências

| Integração/Dependência | Tipo | Direção | Dados trocados | Criticidade | Observações |
| --- | --- | --- | --- | --- | --- |
| | | | | | |

## 12. Segurança, privacidade e conformidade

- Autenticação e autorização por perfil.
- Dados pessoais coletados, finalidade, retenção e descarte.
- Requisitos de LGPD e consentimento, quando aplicável.
- Auditoria e rastreabilidade de operações sensíveis.
- Tratamento de dados sensíveis (localização, veículos, pagamentos, imagens).

## 13. UX e requisitos de interface (alto nível)

- Princípios de experiência.
- Telas/áreas principais e objetivo de cada uma.
- Estados vazios, carregamento, erro e sucesso.
- Requisitos de acessibilidade e responsividade.
- Não prescreva detalhes visuais além do necessário para os requisitos.

## 14. Riscos e mitigações

| ID | Risco | Probabilidade | Impacto | Mitigação | Plano de contingência |
| --- | --- | --- | --- | --- | --- |
| R-001 | | | | | |

## 15. Fases de entrega

| Fase | Escopo (IDs de requisitos) | Resultado esperado | Dependências |
| --- | --- | --- | --- |
| MVP | | | |

## 16. Critérios de sucesso e Definition of Done

- Critérios de aceitação globais do produto.
- O que precisa estar verdadeiro para considerar o PRD entregue.
- Como será feita a validação com usuários ou em produção.

## 17. Questões abertas

| ID | Questão | Impacto | Responsável | Prazo |
| --- | --- | --- | --- | --- |
| QA-001 | | | | |

## 18. Glossário

| Termo | Definição |
| --- | --- |
| | |

## 19. Anexo A - Registro da entrevista

Registre as perguntas de clarificação feitas com a ferramenta `question` (sistema de questões do ambiente) e as decisões resultantes. Se a entrevista não foi realizada, registre o motivo e as lacunas pendentes.

| ID | Pergunta | Resposta / Decisão | Requisitos impactados | Status |
| --- | --- | --- | --- | --- |
| ENT-001 | | | | Confirmada / Pendente |

## 20. Changelog

| Versão | Data | Autor | Mudança |
| --- | --- | --- | --- |
| 1.0 | <AAAA-MM-DD> | | Criação do documento |
