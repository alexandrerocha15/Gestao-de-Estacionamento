---
name: Generate PRD
description: Gera um documento de requisitos de produto (PRD) completo e pronto para revisão a partir de um prompt inicial, ideia, entrevista ou documento existente. Conduz uma entrevista de clarificação com o sistema de questões do ambiente antes de finalizar o artefato. Use quando o usuário pedir um PRD, documento de requisitos de produto, especificação de produto, ou quiser transformar uma ideia em requisitos estruturados salvos em docs/prd/.
---

# Generate PRD

Produza um **Product Requirements Document (PRD)** completo, testável e pronto para revisão, a partir do prompt inicial do usuário e do contexto real do projeto.

**Fronteira de atuação:** esta skill escreve apenas o documento de PRD. Não implemente código, não altere código-fonte, configuração ou specs do OpenSpec. O pedido que acionou a skill autoriza somente a criação/atualização do PRD em `docs/prd/`. Ao terminar, pare e apresente o resultado.

## Entrada

O prompt inicial pode ser:
- Uma ideia curta ou vaga (ex.: "quero um app de agendamento de quadras")
- Uma funcionalidade específica de um produto existente
- Um caminho de documento ou entrevista (ex.: `docs/entrevista.md`)
- Um rascunho de requisitos já parcialmente escrito
- Nada (o comando foi chamado sem argumentos)

Se a entrada estiver vazia, use a ferramenta `question` para perguntar de forma aberta:

> "Qual produto, funcionalidade ou problema o PRD deve cobrir? Descreva a ideia, o público e o resultado esperado."

**Não prossiga sem entender minimamente o que deve ser documentado.**

## Passo 1 - Levantar contexto do projeto (somente leitura)

Inspecione, quando existirem e forem relevantes:

- `README.md` e arquivos em `docs/` (entrevistas, notas, pesquisas)
- `openspec/specs/` (capacidades já existentes) e `openspec/changes/` (trabalho em andamento)
- `openspec/config.yaml` para restrições e convenções do projeto
- Estrutura do código, stack e integrações já presentes (para manter o PRD compatível com a realidade técnica)

Regras:
- Não copie o contexto do projeto para dentro do PRD; use-o para fundamentar o documento.
- Distinga o que existe hoje do que é proposto.
- Se não houver código/documentação suficiente, diga isso no PRD na seção de premissas e limitações, e pergunte apenas o que for bloqueante.

## Passo 2 - Entrevista de requisitos (obrigatória)

Esta skill **conduz uma entrevista antes de escrever o PRD**. O prompt inicial raramente contém tudo o que é necessário. Entreviste o usuário para eliminar ambiguidades materiais e só finalize o artefato depois que as respostas estiverem refletidas no documento.

### 2.1 Preparar a entrevista

1. Extraia do prompt: problema, público, proposta de valor, funcionalidades citadas, restrições e resultados esperados.
2. Use o contexto levantado no Passo 1 **antes** de perguntar. Nunca pergunte algo que já está escrito em `README.md`, `docs/`, specs ou código.
3. Monte uma lista de lacunas. Classifique cada uma como:
   - **Material:** muda escopo, comportamento observável, compatibilidade, regra de negócio, segurança ou critério de aceitação. **Deve** ser perguntada.
   - **Menor:** pode ser resolvida com um padrão razoável. Pode virar premissa, desde que registrada.
4. Para cada lacuna material, formule uma pergunta focada, com o motivo e a decisão que ela desbloqueia.

### 2.2 Usar o sistema de questões do ambiente

Use **sempre a ferramenta `question`** para as perguntas de clarificação. Ela é o sistema de questões do ambiente e permite que o usuário escolha uma opção ou digite a própria resposta.

Regras de uso:

- Agrupe decisões relacionadas em uma única chamada, com no máximo **5 perguntas por rodada**.
- Cada pergunta deve ter **2 a 4 opções concretas**, curtas e mutuamente exclusivas. Quando a evidência sustentar uma recomendação, coloque-a como primeira opção e marque "(Recomendado)" na descrição/label.
- Não inclua uma opção de resposta livre: a ferramenta já oferece "digitar a própria resposta".
- Use `multiple: true` apenas quando o usuário puder escolher mais de uma opção ao mesmo tempo (ex.: funcionalidades do MVP).
- Explique em uma frase por que a resposta importa e o que ela desbloqueia.
- Se a ferramenta `question` não estiver disponível no ambiente, faça as mesmas perguntas em texto e registre que a clarificação foi feita por conversa.

### 2.3 Conduzir as rodadas

1. Faça a primeira rodada com as lacunas materiais mais bloqueantes (problema, público, escopo, sucesso).
2. A cada resposta, atualize o entendimento e verifique se surgiram novas ambiguidades dependentes.
3. Faça novas rodadas somente para o que continuar materialmente ambíguo. Pare quando o PRD puder ser escrito sem suposições relevantes.
4. Não faça perguntas de detalhe que não mudariam o documento. Entrevista não é interrogatório; respeite quando o usuário disser que algo está fora de escopo ou "a definir".
5. Se o usuário pedir explicitamente para pular a entrevista:
   - Avise que o PRD ficará com lacunas.
   - Registre cada lacuna como **questão aberta** e marque o status como `Rascunho - pendente de clarificação`.
   - Nunca preencha a lacuna com um fato inventado.

### 2.4 Registrar as decisões

- Separe claramente: **fatos observados** (documentos/código), **decisões confirmadas na entrevista** e **premissas**.
- Registre cada pergunta material e a resposta em `Anexo A - Registro da entrevista` no PRD.
- Uma resposta do usuário vale como decisão confirmada; silêncio ou opção não respondida **não** é aceitação.
- Não inicie a escrita final do PRD (Passo 4) antes de concluir a entrevista.

## Passo 3 - Definir destino do arquivo

1. Destino padrão: `docs/prd/prd.md`.
2. Use outro nome ou caminho **somente** se o usuário solicitar isso explicitamente.
3. Se o arquivo já existir:
   - Leia a versão atual.
   - Atualize o `prd.md` existente por padrão, incrementando a versão no cabeçalho e registrando a mudança no changelog.
   - Só crie um novo arquivo se o usuário pedir explicitamente; nesse caso, pergunte o nome/caminho desejado.
   - Nunca sobrescreva silenciosamente: informe claramente que o arquivo existente foi atualizado.

## Passo 4 - Escrever o PRD

**Pré-requisito:** conclua a entrevista do Passo 2. Se ainda houver ambiguidade material sem resposta, volte ao Passo 2 e use a ferramenta `question` antes de escrever.

Leia `references/prd-template.md` (relativo a esta skill) e use-o como estrutura obrigatória. Preencha **todas** as seções; quando uma seção não se aplicar, escreva "Não aplicável" com uma justificativa curta, em vez de removê-la.

Regras de qualidade:

- **Idioma:** português do Brasil por padrão. Use outro idioma apenas se o usuário pedir.
- **Rastreabilidade:** numere requisitos funcionais (`RF-001`), não funcionais (`RNF-001`) e regras de negócio (`RN-001`). Referencie esses IDs em jornadas, critérios de aceitação, riscos e fases quando fizer sentido.
- **Testabilidade:** cada requisito deve ter critério de aceitação verificável, preferencialmente no formato "Dado / Quando / Então".
- **Sem ambiguidade:** evite "rápido", "amigável", "moderno", "robusto" sem uma métrica ou condição objetiva.
- **Prioridade:** classifique requisitos com MoSCoW (Essencial, Importante, Desejável, Fora do escopo).
- **Escopo explícito:** sempre inclua o que está **fora do escopo**.
- **Sem design prematuro:** descreva o **quê** e o **porquê**; detalhes de implementação técnica só quando forem restrição real ou decisão já confirmada.
- **Segurança e privacidade:** trate autenticação, autorização, dados pessoais e LGPD quando o produto lidar com pessoas, veículos, pagamentos ou localização.
- **Métricas:** para cada objetivo, defina métrica, meta e forma de medição. Se a meta não foi fornecida, marque como "a definir" e registre a pergunta.
- **Consistência:** o PRD não pode contradizer specs existentes no OpenSpec; se houver conflito, destaque-o na seção de questões abertas ou riscos.
- **Entrevista registrada:** toda pergunta material feita com a ferramenta `question` e sua resposta devem constar no `Anexo A - Registro da entrevista`, com o efeito no documento.

## Passo 5 - Validar antes de salvar

Confira o checklist:

- [ ] A entrevista de requisitos foi realizada com a ferramenta `question` antes da escrita?
- [ ] Nenhuma ambiguidade material ficou sem pergunta ou sem registro como questão aberta?
- [ ] As decisões confirmadas na entrevista estão refletidas nos requisitos e no Anexo A?
- [ ] Problema, público e proposta de valor estão claros?
- [ ] Objetivos têm métricas e metas (ou "a definir" explícito)?
- [ ] Escopo dentro/fora está inequívoco?
- [ ] Todo requisito tem ID, prioridade e critério de aceitação verificável?
- [ ] Requisitos não funcionais cobrem desempenho, disponibilidade, segurança, usabilidade e acessibilidade quando aplicável?
- [ ] Regras de negócio e validações estão explícitas?
- [ ] Fluxos principais e alternativos estão descritos, incluindo erros e exceções?
- [ ] Riscos têm mitigação ou plano de contingência?
- [ ] Premissas e questões abertas estão registradas e não disfarçadas de fatos?
- [ ] O documento está em português (salvo pedido contrário) e sem conteúdo de instruções internas da skill?

## Passo 6 - Salvar e reportar

1. Crie os diretórios necessários e escreva o arquivo no destino definido.
2. Verifique que o arquivo existe e está completo.
3. Responda de forma breve com:
   - Caminho do PRD criado/atualizado
   - Versão e status
   - Contagem de requisitos funcionais e não funcionais
   - Perguntas de clarificação realizadas e decisões confirmadas na entrevista
   - Principais premissas adotadas
   - Questões abertas que precisam de decisão
   - Próximo passo sugerido (revisar o PRD; depois, se o projeto usar OpenSpec, transformar em mudança com `/opsx-propose`)

## Guardrails

- **Entreviste antes de finalizar.** Não escreva a versão final do PRD com ambiguidade material sem antes usar a ferramenta `question`. A entrevista não é opcional.
- Use a ferramenta `question` para clarificações estruturadas, com opções concretas e a resposta livre automática; não substitua por uma lista de perguntas abertas em texto quando a ferramenta estiver disponível.
- Não implemente nada e não altere arquivos fora de `docs/prd/` (ou do caminho explicitamente autorizado).
- Não fabrique dados, pesquisas, métricas ou declarações de usuários. Marque tudo que for suposição.
- Não copie instruções, checklists ou textos internos desta skill para o PRD.
- Não crie múltiplos PRDs para o mesmo escopo sem confirmar com o usuário.
- Se o usuário pedir implementação junto com o PRD, entregue apenas o PRD e explique que a implementação é um passo separado.
