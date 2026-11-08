---
name: Generate TDD
description: Gera um Technical Design Document (TDD) detalhado e pronto para implementação a partir de um PRD, ideia, requisito ou documento existente. Conduz uma entrevista de decisões arquiteturais e estruturais (linguagem, framework, bibliotecas, dados, integrações, segurança, deploy) e centraliza as decisões técnicas em docs/tdd/. Use quando o usuário pedir um TDD, documento de design técnico, design de arquitetura, decisões técnicas ou plano técnico.
---

# Generate TDD

Produza um **Technical Design Document (TDD)** detalhado, consistente com o contexto real do projeto e pronto para orientar a implementação. O TDD é a fonte única das decisões técnicas: toda escolha de arquitetura, linguagem, framework, biblioteca, dados, integração e operação deve estar registrada e justificada nele.

**Fronteira de atuação:** esta skill escreve apenas o documento de TDD. Não implemente código, não altere código-fonte, configuração ou specs do OpenSpec. O pedido que acionou a skill autoriza somente a criação/atualização do TDD em `docs/tdd/`. Ao terminar, pare e apresente o resultado.

## Entrada

O prompt inicial pode ser:
- Uma ideia, funcionalidade ou mudança técnica (ex.: "quero definir a arquitetura do módulo de pagamentos")
- Um caminho de PRD, entrevista ou documento (ex.: `docs/prd/prd.md`)
- Um rascunho de decisões técnicas já parcialmente escrito
- Um repositório com código existente a ser documentado
- Nada (o comando foi chamado sem argumentos)

Se a entrada estiver vazia, use a ferramenta `question` para perguntar de forma aberta:

> "Qual sistema, módulo ou mudança técnica o TDD deve cobrir? Descreva o objetivo, o contexto e o que já está decidido."

**Não prossiga sem entender minimamente o que deve ser documentado.**

## Passo 1 - Levantar contexto do projeto (somente leitura)

Inspecione, quando existirem e forem relevantes:

- `docs/prd/` e `docs/entrevista.md` (requisitos, regras de negócio, RNFs)
- `README.md` (visão do produto e stack declarada)
- `openspec/specs/`, `openspec/changes/` e `openspec/config.yaml` (capacidades, restrições e convenções)
- Código e configurações existentes: manifestos (`*.csproj`, `package.json`, `angular.json`), `Dockerfile`, `appsettings*.json`, `opencode.json`, migrations e contratos
- TDDs anteriores em `docs/tdd/`

Regras:
- Não copie o contexto do projeto para dentro do TDD; use-o para fundamentar as decisões.
- Distinga o que existe hoje do que é proposto.
- Trate a stack já declarada como **restrição padrão**: confirme apenas o que estiver ambíguo e só proponha divergência com justificativa explícita.
- Extraia os requisitos arquiteturalmente significativos (RF/RNF) que exigem decisão técnica.
- Se não houver documentação/código suficiente, diga isso nas premissas e limitações, e pergunte apenas o que for bloqueante.

## Passo 2 - Entrevista de decisões técnicas (obrigatória)

Esta skill **conduz uma entrevista antes de escrever o TDD**. O prompt inicial raramente contém todas as decisões necessárias. Entreviste o usuário para eliminar ambiguidades materiais e só finalize o artefato depois que as respostas estiverem refletidas no documento.

### 2.1 Preparar a entrevista

1. Extraia do prompt e do contexto: escopo, restrições, stack declarada, requisitos e decisões já tomadas.
2. Use o contexto levantado no Passo 1 **antes** de perguntar. Nunca pergunte algo que já está escrito em `README.md`, `docs/`, specs, configurações ou código.
3. Monte uma lista de lacunas. Classifique cada uma como:
   - **Material:** muda arquitetura, estrutura, contratos, segurança, operação, custo de manutenção ou compatibilidade. **Deve** ser perguntada.
   - **Menor:** pode ser resolvida com um padrão razoável. Pode virar premissa, desde que registrada.
4. Para cada lacuna material, formule uma pergunta focada, com o motivo e a decisão que ela desbloqueia.

Cubra, no mínimo, os temas abaixo (banco detalhado em `references/question-bank.md`):

| Tema | O que decidir |
| --- | --- |
| Escopo e restrições | Abrangência do TDD, prazo, equipe, infraestrutura, conformidade, volume |
| Arquitetura e estrutura | Estilo arquitetural, camadas, módulos, monorepo, padrões, dependências permitidas |
| Linguagem e runtime | Versão da linguagem, runtime, compatibilidade e política de atualização |
| Frameworks e bibliotecas | Web/API, UI, validação, mapeamento, estado, build, testes e justificativa de cada dependência |
| Dados e persistência | Banco, ORM, modelagem, migrações, concorrência, auditoria, cache, jobs |
| Interfaces e integrações | Estilo de API, contratos, versionamento, erros, idempotência, serviços externos |
| Segurança | Autenticação, autorização, multi-tenancy, segredos, LGPD, auditoria |
| Operação e qualidade | Ambientes, CI/CD, observabilidade, backup, estratégia de testes, performance |

### 2.2 Usar o sistema de questões do ambiente

Use **sempre a ferramenta `question`** para as perguntas de decisão. Ela é o sistema de questões do ambiente e permite que o usuário escolha uma opção ou digite a própria resposta.

Regras de uso:

- Agrupe decisões relacionadas em uma única chamada, com no máximo **5 perguntas por rodada**.
- Cada pergunta deve ter **2 a 4 opções concretas**, curtas e mutuamente exclusivas. Quando a evidência sustentar uma recomendação, coloque-a como primeira opção e marque "(Recomendado)" na descrição/label.
- Não inclua uma opção de resposta livre: a ferramenta já oferece "digitar a própria resposta".
- Use `multiple: true` apenas quando o usuário puder escolher mais de uma opção ao mesmo tempo (ex.: bibliotecas desejadas).
- Explique em uma frase por que a resposta importa e o que ela desbloqueia.
- Se a ferramenta `question` não estiver disponível no ambiente, faça as mesmas perguntas em texto e registre que a entrevista foi feita por conversa.

### 2.3 Conduzir as rodadas

1. Comece pelas lacunas mais bloqueantes: escopo, arquitetura, linguagem/runtime e frameworks.
2. A cada resposta, atualize o entendimento e verifique se surgiram novas ambiguidades dependentes.
3. Faça novas rodadas somente para o que continuar materialmente ambíguo. Pare quando o TDD puder ser escrito sem suposições relevantes.
4. Não pergunte detalhes que não mudariam o documento. Entrevista não é interrogatório; respeite quando o usuário disser que algo está fora de escopo ou "a definir".
5. Se o usuário pedir explicitamente para pular a entrevista:
   - Avise que o TDD ficará com decisões em aberto.
   - Registre cada lacuna como **decisão em aberto** (`DEC-XXX` com status `Em aberto`) e marque o status do documento como `Rascunho - pendente de decisão`.
   - Nunca preencha a lacuna com uma escolha inventada.

### 2.4 Registrar as decisões

- Separe claramente: **fatos observados** (documentos/código), **decisões confirmadas na entrevista** e **premissas**.
- Registre cada pergunta material e a resposta em `Anexo A - Registro da entrevista` no TDD.
- Uma resposta do usuário vale como decisão confirmada; silêncio ou opção não respondida **não** é aceitação.
- Antes de escrever o documento, apresente um resumo (`ID | Tema | Decisão | Status`) e peça confirmação. Corrija o que for apontado.
- Não inicie a escrita final do TDD (Passo 4) antes de concluir a entrevista e confirmar o resumo.

## Passo 3 - Definir destino do arquivo

1. Destino padrão: `docs/tdd/tdd.md`.
2. Use outro nome ou caminho **somente** se o usuário solicitar isso explicitamente.
3. Se o arquivo já existir:
   - Leia a versão atual.
   - Atualize o `tdd.md` existente por padrão, incrementando a versão no cabeçalho e registrando a mudança no changelog.
   - Só crie um novo arquivo se o usuário pedir explicitamente; nesse caso, pergunte o nome/caminho desejado.
   - Nunca sobrescreva silenciosamente: informe claramente que o arquivo existente foi atualizado.
4. Não crie múltiplos TDDs para o mesmo escopo sem pedido explícito do usuário.

## Passo 4 - Escrever o TDD

**Pré-requisito:** conclua a entrevista do Passo 2 e confirme o resumo das decisões. Se ainda houver ambiguidade material sem resposta, volte ao Passo 2 e use a ferramenta `question` antes de escrever.

Leia `references/tdd-template.md` e `references/mermaid-guide.md` (relativos a esta skill) e use-os como base. Preencha **todas** as seções; quando uma seção não se aplicar, escreva "Não aplicável" com uma justificativa curta, em vez de removê-la.

Regras de qualidade:

- **Idioma:** português do Brasil por padrão. Use outro idioma apenas se o usuário pedir.
- **Centralização das decisões:** cada decisão técnica recebe um ID estável `DEC-001`, `DEC-002`, ... com contexto, decisão, alternativas consideradas, justificativa, consequências, status e requisitos relacionados. O restante do documento referencia esses IDs em vez de repetir justificativas.
- **Status das decisões:** `Aceita`, `Proposta`, `Em aberto` ou `Substituída por DEC-XXX`. Uma nova decisão que contradiga outra deve marcar a anterior como substituída, nunca apagar o histórico.
- **Trade-offs explícitos:** toda escolha relevante deve listar a alternativa descartada e o motivo. Evite "boas práticas" genéricas sem consequência concreta.
- **Especificidade:** informe versões, pacotes, caminhos, contratos, limites e configurações. Não use `TBD` solto.
- **Rastreabilidade:** referencie os IDs do PRD (`RF-001`, `RNF-001`, `RN-001`) e preencha a matriz PRD → TDD. Todo requisito arquiteturalmente significativo deve apontar para uma seção ou decisão.
- **Consistência:** o TDD não pode contradizer o PRD nem as specs do OpenSpec; se houver conflito, destaque-o nas questões abertas ou riscos.
- **Diagramas Mermaid:** sempre procure oportunidades de explicar visualmente relacionamentos, fluxos, estados e implantação. Siga a subseção 4.1 e `references/mermaid-guide.md`.
- **Dados e concorrência:** explicite modelo, índices, restrições, transações, idempotência e política de auditoria/retenção.
- **Segurança e privacidade:** trate autenticação, autorização, segredos, dados pessoais e LGPD quando o produto lidar com pessoas, veículos, pagamentos ou localização.
- **Dependências justificadas:** toda biblioteca/framework deve ter finalidade, versão, justificativa e decisão associada.
- **Sem implementação prematura:** descreva arquitetura, contratos, schemas e fluxos; trechos de código apenas quando forem normativos e curtos.
- **Entrevista registrada:** toda pergunta material feita com a ferramenta `question` e sua resposta devem constar no `Anexo A - Registro da entrevista`, com o efeito no documento.

### 4.1 Diagramas Mermaid

Diagramas são parte do TDD, não decoração. **Sempre avalie** se um relacionamento, fluxo ou estrutura ficaria mais claro visualmente e inclua o diagrama correspondente. No mínimo, procure cobrir:

| Necessidade | Tipo Mermaid sugerida | Onde usar no TDD |
| --- | --- | --- |
| Atores, sistemas externos e fronteiras | `flowchart` | Visão de contexto |
| Containers, componentes e dependências | `flowchart` com `subgraph` | Arquitetura da solução |
| Relacionamentos entre entidades | `erDiagram` ou `classDiagram` | Modelo de dados / domínio |
| Fluxos críticos e integrações | `sequenceDiagram` | Fluxos principais |
| Ciclo de vida de entidades | `stateDiagram-v2` | Estados de veículo, estadia, pagamento etc. |
| Ambientes e infraestrutura | `flowchart` | Build, deploy e operação |

Regras:

- Escreva diagramas **válidos** e legíveis; prefira poucos nós (até ~15 por diagrama) e divida os grandes por assunto.
- Todo diagrama deve ter um título ou legenda e um parágrafo curto explicando o que ele mostra e por que importa.
- Referencie decisões (`DEC-XXX`) e requisitos (`RF/RNF`) na explicação do diagrama quando fizer sentido.
- Use rótulos claros e consistentes com o restante do documento; evite abreviações não definidas.
- Não repita no diagrama tudo o que já está em tabelas: use-o para mostrar relações, ordem, dependências e estados.
- Não use imagens externas; mantenha os diagramas como código Mermaid versionável dentro do Markdown.
- Se um diagrama não puder ser validado, simplifique-o em vez de entregá-lo quebrado.

Consulte `references/mermaid-guide.md` para exemplos e boas práticas.

## Passo 5 - Validar antes de salvar

Confira o checklist:

- [ ] O contexto do projeto foi levantado antes das perguntas?
- [ ] A entrevista de decisões foi realizada com a ferramenta `question` antes da escrita?
- [ ] O resumo das decisões foi confirmado pelo usuário?
- [ ] Nenhuma ambiguidade material ficou sem pergunta ou sem registro como decisão em aberto?
- [ ] As decisões confirmadas estão refletidas na arquitetura, nas dependências e no Anexo A?
- [ ] Arquitetura, camadas, módulos e dependências permitidas estão claros?
- [ ] Linguagem, runtime, frameworks e versões estão especificados?
- [ ] Toda biblioteca tem finalidade, versão e justificativa?
- [ ] O modelo de dados cobre entidades, relacionamentos, índices, restrições e migrações?
- [ ] Há diagramas Mermaid para contexto, arquitetura, relacionamentos e fluxos críticos, com legendas e sem duplicação desnecessária?
- [ ] Concorrência, idempotência e auditoria estão tratadas nos fluxos críticos?
- [ ] Contratos de API, erros, versionamento e integrações estão definidos?
- [ ] Segurança, autorização, segredos e LGPD estão tratados quando aplicável?
- [ ] Todo RNF tem estratégia de atendimento e forma de verificação?
- [ ] A matriz de rastreabilidade PRD → TDD está completa?
- [ ] Riscos têm mitigação ou plano de contingência?
- [ ] Premissas e decisões em aberto estão registradas e não disfarçadas de fatos?
- [ ] O documento está em português (salvo pedido contrário) e sem conteúdo de instruções internas da skill?

## Passo 6 - Salvar e reportar

1. Crie os diretórios necessários e escreva o arquivo no destino definido.
2. Verifique que o arquivo existe e está completo.
3. Responda de forma breve com:
   - Caminho do TDD criado/atualizado
   - Versão e status
   - Contagem de decisões aceitas e em aberto
   - Diagramas Mermaid incluídos e o que cada um explica
   - Entrevista realizada e decisões confirmadas
   - Principais riscos técnicos
   - Questões abertas que precisam de decisão
   - Primeira fase recomendada de implementação

## Guardrails

- **Entreviste antes de finalizar.** Não escreva a versão final do TDD com ambiguidade material sem antes usar a ferramenta `question`. A entrevista não é opcional.
- Use a ferramenta `question` para clarificações estruturadas, com opções concretas e a resposta livre automática; não substitua por uma lista de perguntas abertas em texto quando a ferramenta estiver disponível.
- Centralize as decisões no TDD. Não deixe escolhas técnicas espalhadas apenas em conversa.
- Não implemente nada e não altere arquivos fora de `docs/tdd/` (ou do caminho explicitamente autorizado).
- Não fabrique requisitos, métricas, restrições ou declarações de usuários. Marque tudo que for suposição.
- Não reabra silenciosamente a stack declarada no projeto; divergências exigem decisão explícita e justificada.
- Não copie instruções, checklists ou textos internos desta skill para o TDD.
- Se o usuário pedir implementação junto com o TDD, entregue apenas o TDD e explique que a implementação é um passo separado.
