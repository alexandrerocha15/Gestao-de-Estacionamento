# Banco de perguntas para o TDD

Use como referência, não como questionário obrigatório. Pergunte apenas o que
o repositório e as respostas anteriores ainda não esclareceram. Agrupe no
máximo 5 perguntas por rodada e coloque a opção recomendada primeiro.

## 1. Escopo e restrições

- Qual é o escopo deste TDD: sistema inteiro, um módulo, uma mudança?
- Quais requisitos do PRD são arquiteturalmente significativos?
- Existe prazo, tamanho de equipe ou orçamento que limite a solução?
- Há restrições de infraestrutura (nuvem, on-premises, rede, Windows/Linux)?
- Existem normas ou políticas obrigatórias (LGPD, PCI, acessibilidade, auditoria)?
- O sistema precisa conviver com legado ou substituí-lo?
- Qual o volume esperado: usuários simultâneos, veículos/dia, dados históricos?

## 2. Arquitetura e estrutura

- Estilo arquitetural: monolito modular, camadas, Clean Architecture, DDD,
  CQRS, microsserviços, serverless? Por quê?
- O sistema é um único deploy ou vários? Quais limites de módulo/bounded context?
- Monorepo ou repositórios separados? Como o frontend e o backend se organizam?
- Quais padrões são obrigatórios: Repository, Unit of Work, Mediator, Event
  Sourcing, Outbox, background jobs?
- Como as camadas se comunicam e quais dependências são proibidas?
- Onde ficam regras de negócio, validações e orquestração?
- Como erros, resultados e transações atravessam as camadas?

## 3. Linguagem, runtime e frameworks

- Versão da linguagem e do runtime (ex.: .NET 8/9, Node 22, TypeScript 5).
- Framework web/API (ex.: ASP.NET Core Minimal APIs ou Controllers).
- Framework de UI (ex.: Angular standalone, React, Blazor) e versão.
- Estratégia de renderização/estado no frontend (SSR, SPA, Signals, RxJS,
  gerenciamento de estado).
- Estilo de estilos/componentes (ex.: Tailwind CSS, design system, Lucide).
- Ferramentas de build e empacotamento.
- Política de atualização de dependências e suporte de versão.

## 4. Bibliotecas e dependências

Para cada biblioteca, confirme: finalidade, versão, alternativas, licença e
custo de manutenção.

- Validação (ex.: FluentValidation, Zod).
- Mapeamento (ex.: AutoMapper ou mapeamento manual).
- ORM e acesso a dados (ex.: EF Core, Dapper).
- Logs, métricas e tracing (ex.: Serilog, OpenTelemetry).
- Testes (xUnit/NUnit, Vitest/Jest, Playwright/Cypress, Testcontainers).
- Autenticação/autorização e criptografia.
- Geração de PDF, e-mail, filas, cache, agendamento.
- Biblioteca de componentes e ícones do frontend.

## 5. Dados e persistência

- Banco principal e versão (ex.: SQL Server 2022) e justificativa.
- Estratégia de modelagem: relacional, documental, híbrida.
- ORM e abordagem de migrações (code-first, database-first, versionadas).
- Convenções de nomenclatura, chaves, índices e restrições.
- Como garantir unicidade e concorrência (ex.: vaga ocupada, pagamento).
- Auditoria: tabela dedicada, eventos, soft delete, who/when/what.
- Histórico e retenção de dados; anonimização/LGPD.
- Cache, read models e jobs assíncronos; consistência aceitável.
- Estratégia de seed e dados de teste.

## 6. Interfaces e integrações

- Estilo de API: REST/OpenAPI, GraphQL, gRPC; versionamento e erros.
- Contratos síncronos vs. eventos/filas; idempotência.
- Integrações externas: pagamento, notificação, câmera, OCR de placa,
  impressão, ERP/financeiro.
- Tratamento de indisponibilidade, retry, timeout e circuit breaker.
- Webhooks e autenticação entre serviços.
- Limites de payload, paginação, filtros e ordenação.

## 7. Segurança

- Autenticação: ASP.NET Core Identity, OIDC, provedor externo?
- Autorização: perfis/papéis, policies, permissões por recurso.
- Multi-tenancy: necessário? Como isolar dados?
- Gestão de segredos e configuração por ambiente.
- Sessão, tokens, expiração, refresh e logout.
- Proteção de dados sensíveis, criptografia e mascaramento.
- Trilha de auditoria e requisitos de não repúdio.
- Política de senhas, bloqueio e recuperação.

## 8. Operação, qualidade e entrega

- Ambientes (dev, teste, homologação, produção) e paridade.
- CI/CD: ferramenta, gates, migrações automáticas ou manuais.
- Containerização e orquestração; requisitos de infraestrutura.
- Observabilidade: logs estruturados, correlação, métricas, alertas, health checks.
- Backup, restauração, RPO/RTO e continuidade.
- Estratégia de testes por camada e cobertura mínima dos fluxos críticos.
- Dados de teste, testes de carga e critérios de performance.
- Acessibilidade, responsividade e navegadores suportados.
- Versionamento do produto, changelog e estratégia de rollout.

## 9. Perguntas de fechamento

- Qual decisão é mais arriscada e precisa de spike/prova de conceito?
- O que pode ficar para depois sem comprometer a arquitetura?
- Quem aprova as decisões técnicas?
- Qual é o critério para considerar este TDD concluído?
