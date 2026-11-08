# PRD - Plataforma de Gestão de Estacionamento e Valet

| Campo | Valor |
| --- | --- |
| Versão | 1.1 |
| Status | Aprovado |
| Autor | Agente OpenCode |
| Data | 2026-11-08 |
| Stakeholders | Alexandre Rech (proprietário); equipe de operação (atendentes e manobristas); gestão/financeiro; Tiago Santini (levantamento de requisitos) |
| Links | [Entrevista](../entrevista.md) · [README](../../README.md) · OpenSpec (`openspec/`) |

## 1. Resumo executivo

A Plataforma de Gestão de Estacionamento e Valet é um sistema web de uso interno para centralizar a operação de um estacionamento com serviço de valet. Ela substitui tickets de papel, controles manuais e planilhas por registros digitais de entrada, ocupação de vagas, solicitações de retirada, cálculo de diárias, pagamentos, fechamento de caixa e relatórios gerenciais.

O produto atende atendentes, manobristas, gestores/financeiro e administradores da operação. O resultado esperado é reduzir erros de cobrança e tempo de espera, dar visibilidade confiável da ocupação e dos valores recebidos, permitir auditoria das ações sensíveis e sustentar o crescimento do negócio sem aumentar proporcionalmente a complexidade administrativa.

## 2. Contexto e problema

### 2.1 Situação atual

- O cliente chega, um funcionário recebe o carro, entrega um ticket de papel numerado e procura uma vaga.
- A identificação do veículo é feita em controle interno com placa, modelo e horário de entrada.
- A localização do veículo depende da memória da equipe e de conferência física das vagas.
- A retirada é solicitada no balcão e coordenada verbalmente entre os funcionários.
- A cobrança é calculada manualmente, por diárias, a partir do horário de entrada.
- O fechamento do caixa é consolidado em planilhas.
- O acesso aos registros é praticamente irrestrito entre os funcionários.

### 2.2 Problema

- Perda de tickets gera demora na localização do registro e preocupação de segurança na comprovação de propriedade.
- A equipe não sabe em tempo real quantas vagas estão livres nem onde cada veículo está.
- Pedidos de retirada podem ser esquecidos ou demorar mais que o esperado, especialmente em picos.
- Cálculos manuais de diárias geram divergências de cobrança e tratamento desigual entre clientes.
- O controle financeiro manual dificulta saber o que foi pago, o que está pendente e quanto entrou por forma de pagamento.
- Reclamações de danos não têm histórico confiável das condições do veículo na entrada e na saída.
- A falta de trilha de auditoria impede identificar quem registrou, cobrou ou entregou cada veículo.
- Planilhas e registros paralelos aumentam o esforço de consolidação e o risco de erro.

### 2.3 Oportunidade

A operação já reconhece as dores e pretende crescer. Centralizar a operação em uma plataforma única, com regras de cobrança padronizadas e dados confiáveis, reduz perdas financeiras, melhora a experiência do cliente e cria base para decisões gerenciais. O repositório atual contém apenas documentação e a stack declarada; não há, no repositório, sistema implementado a ser migrado.

### 2.4 Hipótese de valor

> Se a operação de valet for centralizada em uma plataforma web que registra entradas, vagas, retiradas e cobranças automaticamente, então a equipe conseguirá atender com menos erros e mais rapidez, medido pela redução do tempo entre solicitação e entrega e pela eliminação de divergências não justificadas no fechamento de caixa.

## 3. Objetivos e métricas de sucesso

| ID | Objetivo | Métrica | Baseline | Meta | Como medir |
| --- | --- | --- | --- | --- | --- |
| OBJ-001 | Padronizar a cobrança de diárias | Percentual de estadias cobradas com cálculo do sistema | 0% (cálculo manual) | 100% | Registros de cobrança gerados pela plataforma |
| OBJ-002 | Reduzir o tempo de entrega do veículo | Mediana do tempo entre solicitação e entrega, em minutos | Não medido | A definir (QA-001) | Timestamps de solicitação e entrega (RF-027) |
| OBJ-003 | Dar visibilidade confiável da ocupação | Percentual de estadias ativas vinculadas a uma vaga válida | Não medido | 100% | Relatório de consistência de vagas |
| OBJ-004 | Organizar a fila de retiradas | Percentual de solicitações com status e tempos registrados | 0% (controle verbal) | 100% | Fila e histórico de retiradas |
| OBJ-005 | Reduzir divergências de caixa | Divergência não justificada no fechamento diário sobre a receita do dia | Não medido | 0% | Fechamentos diários (RF-038) |
| OBJ-006 | Garantir rastreabilidade das operações | Percentual de ações sensíveis com auditoria completa | 0% | 100% | Trilha de auditoria (RF-004) |
| OBJ-007 | Agilizar o acompanhamento gerencial | Percentual de dias com fechamento e relatórios disponíveis até o dia seguinte | Planilhas manuais | 100% | Data/hora do fechamento e dos relatórios |
| OBJ-008 | Melhorar a experiência do cliente | Reclamações formais por mês sobre demora e cobrança | Não medido | A definir (QA-001) | Registro externo de reclamações |
| OBJ-009 | Tornar o check-in rápido para a equipe | Tempo médio de check-in por atendente treinado | Não medido | A definir (proposta: até 1 minuto) | Logs de operação e teste de usabilidade |
| OBJ-010 | Manter a operação disponível | Disponibilidade mensal no horário de operação | Não aplicável | A definir (proposta: 99,5%) | Monitoramento de disponibilidade (QA-002) |

## 4. Público-alvo e personas

| Persona | Perfil | Necessidades | Dores | Contexto de uso |
| --- | --- | --- | --- | --- |
| Administrador (Alexandre Rech) | Proprietário da operação | Visão completa da operação e das finanças, controle de usuários, tarifas e auditoria | Dependência de planilhas e decisões sem dados confiáveis | Escritório e visitas ao balcão; desktop e celular |
| Gestor/Financeiro | Responsável pela conferência financeira e autorizações | Fechamento de caixa, relatórios, aprovação de descontos, estornos e ticket perdido | Divergências de cobrança, consolidação manual, falta de rastreabilidade | Desktop no balcão ou escritório |
| Atendente/Caixa | Recebe veículos, consulta estadias, cobra e entrega tickets | Check-in rápido, busca por placa ou ticket, cálculo automático, comprovante | Cálculo manual, ticket perdido, fila verbal | Balcão, desktop e tablet |
| Manobrista | Move e localiza veículos no pátio | Saber vaga e prioridade de retirada, atualizar status sem burocracia | Não saber onde o carro está, pedidos esquecidos | Celular ou tablet, em movimento |
| Cliente do valet (secundário) | Cliente do serviço, não usuário do sistema no MVP | Rapidez, transparência do valor e segurança do veículo | Espera, cobrança divergente, danos sem comprovação | Balcão de atendimento |

Perfis secundários: equipe de TI/fornecedor responsável por manutenção e suporte.

Não são público-alvo do MVP: clientes como usuários de aplicativo, operadores de outros estacionamentos (modelo SaaS), clientes de estacionamento sem valet e fornecedores de cancela, leitor de placas ou câmeras.

## 5. Escopo

### 5.1 Dentro do escopo

- Operação de uma única unidade de estacionamento com valet.
- Cadastro de usuários, quatro perfis fixos e permissões por módulo.
- Configuração da unidade, da capacidade e das vagas.
- Cadastro de tarifa única de diária com vigência e histórico.
- Check-in com placa, modelo, nome e documento do responsável, horário, vaga e condição do veículo em texto.
- Emissão e reimpressão de ticket com identificador único e QR/código, com impressão em impressora térmica.
- Mapa/lista de vagas com estados livre e ocupada, contadores e atualização quase em tempo real.
- Busca de veículos ativos por placa, código do ticket, nome ou documento.
- Solicitação, fila, atribuição e acompanhamento de retiradas com status e tempos.
- Movimentação de veículos entre vagas e cancelamento de solicitação de retirada, com auditoria.
- Tratamento de ticket perdido com conferência de documento e autorização de gestor.
- Registro obrigatório das condições do veículo na entrada e na saída, em texto.
- Cálculo automático de permanência, diárias e total.
- Registro de pagamento integral por dinheiro, débito, crédito ou Pix, sem integração com meios de pagamento.
- Desconto, isenção, estorno e cancelamento com autorização e auditoria.
- Comprovante simples de pagamento, imprimível ou em PDF.
- Fechamento diário de caixa com conferência por forma de pagamento e reabertura controlada.
- Painel operacional e relatórios por dia, semana e mês, com exportação CSV.
- Registro de operações de contingência após queda de internet, com aprovação.
- Trilha de auditoria das ações sensíveis.
- Retenção de dados conforme os prazos definidos na seção 12.

### 5.2 Fora do escopo

- Aplicativo ou autoatendimento para o cliente final solicitar o carro, consultar status ou pagar.
- Integração com adquirente, maquininha de cartão, gateway de pagamento ou Pix automático.
- Emissão de nota fiscal de serviço (NFS-e) ou integração fiscal.
- Fotografias de danos ou qualquer captura de imagem no MVP.
- Múltiplas unidades, filiais ou modelo SaaS multiempresa.
- Operação offline com sincronização automática.
- Aplicativo nativo para Android ou iOS.
- Leitura automática de placas (LPR), cancela automática, sensores de vaga ou integração com CFTV.
- Reserva de vagas, planos mensais, fidelidade, mensalistas ou tarifas dinâmicas.
- Lavagem, manutenção ou outros serviços sobre o veículo.
- Integração contábil, ERP ou conciliação bancária.
- Notificações por WhatsApp, SMS ou e-mail ao cliente.
- Controle de acesso físico, biometria ou reconhecimento facial.
- Migração automática de planilhas ou registros históricos.

### 5.3 Premissas

- A operação é de uma única unidade, com um único pátio de valet.
- Todos os veículos atendidos passam pelo check-in; não há clientes de autoatendimento no MVP.
- Existe uma tarifa única de diária, igual para todos os tipos de veículo, com vigência.
- A cobrança mínima é de uma diária para qualquer permanência, conforme decisão da entrevista.
- Os meios de pagamento do MVP são dinheiro, débito, crédito e Pix, registrados manualmente.
- Não há pagamento dividido nem pagamento parcial no MVP.
- O documento do responsável é CPF ou RG, armazenado com tipo e número.
- Existe impressora térmica de 80 mm no balcão; se ela falhar, o ticket pode ser exibido na tela.
- O sistema é web responsivo, acessado por navegador em desktop, tablet e celular.
- Há conexão de internet durante a operação; quedas são tratadas por contingência manual.
- O número e os identificadores das vagas serão informados antes da configuração inicial.
- Não há integração com sistemas externos no MVP.
- O sistema será novo e não está prevista migração automática de dados históricos; caso seja necessária, será tratada como escopo adicional (QA-008).
- O horário oficial de referência é America/Sao_Paulo.

### 5.4 Restrições

- **Técnicas:** stack declarada no README com .NET, SQL Server, Entity Framework Core, ASP.NET Core Identity, Angular, Tailwind CSS, Lucide Icons e OpenAPI/Swagger. O PRD deve permanecer compatível com essa stack.
- **Arquiteturais:** sistema single-tenant, para uma unidade, sem preparação para múltiplos tenants no MVP.
- **Legais:** tratamento de dados pessoais conforme a LGPD; retenção de 5 anos para registros financeiros e 2 anos para registros operacionais, com anonimização após o prazo operacional.
- **Operacionais:** dependência de internet e de impressora térmica; contingência manual exige aprovação e rastreabilidade.
- **De escopo:** não há fotografias, integração de pagamento, nota fiscal, autoatendimento do cliente nem operação offline no MVP.
- **De prazo e orçamento:** não definidos até a aprovação deste PRD; devem ser tratados no planejamento da entrega.
- **De dados:** não há baseline histórico confiável de tempos, reclamações ou divergências; as metas dependentes de baseline estão marcadas como a definir.
- **Metas técnicas propostas:** disponibilidade, recuperação e volumetria são propostas iniciais, sujeitas a validação no planejamento (QA-002 e QA-012).

## 6. Jornadas e fluxos principais

### 6.1 Jornada de check-in

```text
[Cliente chega]
   --> [Atendente informa placa, modelo, responsável e documento]
   --> [Sistema valida placa e duplicidade]
   --> [Atendente seleciona vaga livre]
   --> [Atendente registra condição de entrada em texto]
   --> [Sistema calcula tarifa vigente e gera ticket único]
   --> [Ticket impresso e entregue ao cliente]
   --> [Estadia ativa e vaga ocupada]

Exceções:
[Placa inválida ou já ativa] --> [Bloquear e exibir estadia existente]
[Sem vaga livre] --> [Bloquear check-in e informar lotação]
[Vaga ocupada por outro usuário] --> [Erro de concorrência e nova seleção]
[Impressora indisponível] --> [Exibir código/QR na tela e permitir reimpressão]
```

### 6.2 Jornada de retirada e entrega

```text
[Cliente solicita o carro]
   --> [Atendente localiza por ticket ou placa]
   --> [Sistema cria solicitação Solicitada com horário]
   --> [Atendente ou gestor atribui manobrista]
   --> [Manobrista marca Em busca]
   --> [Atendente calcula e cobra a estadia]
   --> [Pagamento integral ou isenção autorizada]
   --> [Atendente registra condição de saída]
   --> [Entrega confirmada com horário]
   --> [Vaga liberada automaticamente]

Exceções:
[Cliente perdeu o ticket] --> [Busca por placa, conferência de documento e autorização]
[Pagamento pendente] --> [Bloquear entrega até pagamento ou isenção]
[Condição de saída vazia] --> [Bloquear entrega]
[Solicitação cancelada] --> [Estadia volta a Aberta e vaga permanece ocupada]
```

### 6.3 Jornada de cobrança

```text
[Atendente abre o checkout]
   --> [Sistema calcula duração e diárias]
   --> [Exibe composição do cálculo]
   --> [Há desconto ou isenção?]
        --> [Não] --> [Registra pagamento integral]
        --> [Sim] --> [Gestor autoriza com motivo e auditoria] --> [Registra pagamento ou isenção]
   --> [Emite comprovante simples]
   --> [Cobrança Paga ou Isenta]
   --> [Entrega liberada]
```

### 6.4 Jornada de fechamento diário

```text
[Fim do dia]
   --> [Sistema apura pagamentos, estornos, descontos e valores em aberto]
   --> [Operador informa valores conferidos por forma de pagamento]
   --> [Sistema calcula divergências]
   --> [Fechamento concluído e auditado]
   --> [Resumo disponível para impressão ou CSV]

Exceções:
[Pagamento lançado após o fechamento] --> [Reabertura pelo Administrador com justificativa]
[Divergência identificada] --> [Registrar justificativa e tratar antes de encerrar]
```

### 6.5 Jornada de ticket perdido

```text
[Cliente informa perda do ticket]
   --> [Atendente busca pela placa]
   --> [Confere documento com o cadastro da entrada]
   --> [Documento confere?]
        --> [Não] --> [Bloquear e escalar para gestor]
        --> [Sim] --> [Gestor ou Administrador autoriza com motivo]
   --> [Ticket marcado como perdido]
   --> [Cobrança permanece a mesma]
   --> [Entrega segue o fluxo normal]
```

### 6.6 Jornada de contingência por falta de internet

```text
[Internet indisponível]
   --> [Equipe registra a operação em controle manual temporário]
   --> [Conexão retorna]
   --> [Usuário autorizado lança entradas, saídas e pagamentos com horários reais]
   --> [Registro marcado como contingência e justificado]
   --> [Gestor ou Administrador aprova]
   --> [Registro entra nos relatórios e no fechamento]
```

### 6.7 Jornada gerencial

```text
[Gestor acessa o painel]
   --> [Consulta veículos no pátio, vagas, fila e valores em aberto]
   --> [Seleciona período: dia, semana ou mês]
   --> [Analisa faturamento, atendimentos, picos, ocupação e tempos]
   --> [Exporta CSV quando necessário]
```

## 7. Requisitos funcionais

| ID | Requisito | Prioridade | Persona | Critérios de aceitação |
| --- | --- | --- | --- | --- |
| RF-001 | Autenticar usuários com e-mail e senha. | Essencial | Todos | Dado um usuário ativo com credenciais válidas, quando informar e-mail e senha, então o sistema autentica e abre o painel conforme o perfil; credenciais inválidas exibem mensagem genérica sem revelar se o e-mail existe. |
| RF-002 | Aplicar os quatro perfis fixos e suas permissões. | Essencial | Todos | Dado um usuário com perfil Manobrista, quando acessar o sistema, então não visualiza valores financeiros nem áreas administrativas; tentativa de acesso direto por URL é negada. |
| RF-003 | Permitir que o Administrador gerencie usuários. | Essencial | Administrador | Dado um Administrador, quando cadastrar um usuário com perfil e senha temporária, então o usuário deve trocar a senha no primeiro acesso; usuários desativados não conseguem autenticar. |
| RF-004 | Registrar auditoria das ações sensíveis. | Essencial | Administrador, Gestor/Financeiro | Dado um desconto, estorno, cancelamento, reabertura de caixa ou ticket perdido, quando a ação for confirmada, então o sistema registra usuário, data, hora, ação, entidade, motivo e valores antes/depois em trilha não editável. |
| RF-005 | Configurar dados da unidade e capacidade de vagas. | Essencial | Administrador | Dado um Administrador, quando cadastrar vagas com identificadores únicos, então a capacidade passa a ser o número de vagas ativas e identificadores duplicados são rejeitados. |
| RF-006 | Ativar e desativar vagas. | Essencial | Administrador | Dado uma vaga ocupada, quando tentar desativá-la, então o sistema bloqueia; vaga inativa não aparece como disponível para check-in. |
| RF-007 | Cadastrar tarifa única de diária com vigência. | Essencial | Administrador | Dado um Administrador, quando cadastrar valor e início de vigência, então apenas uma tarifa fica vigente por data, o histórico é preservado e sobreposições são rejeitadas. |
| RF-008 | Registrar check-in do veículo. | Essencial | Atendente/Caixa | Dado um atendente autenticado, quando informar placa, modelo, nome e documento do responsável, vaga livre e condição de entrada, então o sistema cria a estadia com horário do servidor e ticket; campo obrigatório vazio bloqueia a gravação. |
| RF-009 | Validar placa e impedir duplicidade de estadia ativa. | Essencial | Atendente/Caixa | Dado uma placa válida já com estadia ativa, quando tentar novo check-in, então o sistema bloqueia e exibe o ticket ativo; placa fora dos padrões aceitos é rejeitada. |
| RF-010 | Gerar ticket único e imprimir. | Essencial | Atendente/Caixa | Dado um check-in concluído, quando o ticket for emitido, então ele recebe identificador único não reutilizável e é enviado à impressora térmica; se a impressão falhar, código e QR ficam visíveis na tela. |
| RF-011 | Reimprimir ticket existente. | Importante | Atendente/Caixa | Dado um ticket existente, quando um usuário autorizado reimprimir, então o mesmo identificador é impresso e a reimpressão é registrada em auditoria com usuário, data e hora. |
| RF-012 | Garantir ocupação exclusiva da vaga. | Essencial | Atendente/Caixa | Dado dois atendentes selecionando a mesma vaga livre, quando ambos confirmarem, então apenas um check-in é gravado; o outro recebe erro de vaga ocupada e deve escolher outra. |
| RF-013 | Bloquear check-in quando não houver vaga livre. | Essencial | Atendente/Caixa | Dado que não há vagas ativas livres, quando tentar check-in, então o sistema bloqueia a operação e informa a lotação, sem gerar ticket. |
| RF-014 | Buscar veículos ativos. | Essencial | Atendente/Caixa, Gestor/Financeiro, Administrador, Manobrista | Dado um termo de placa ou código do ticket, quando o usuário pesquisar, então o sistema lista estadias ativas com vaga, entrada e indicador de pagamento; busca por nome ou documento é restrita a perfis autorizados e o documento é mascarado para quem não tem permissão financeira. |
| RF-015 | Exibir detalhe e histórico da estadia. | Essencial | Atendente/Caixa, Gestor/Financeiro | Dado uma estadia, quando abrir o detalhe, então o sistema exibe veículo, responsável, vaga, condições de entrada e saída, tarifa, cobrança e linha do tempo; estadia entregue é somente leitura. |
| RF-016 | Cancelar entrada com autorização. | Essencial | Gestor/Financeiro, Administrador | Dado uma estadia aberta, sem pagamento e não entregue, quando um Gestor ou Administrador autorizar o cancelamento com motivo, então a estadia fica cancelada, a vaga é liberada e a ação é auditada; após pagamento ou entrega o cancelamento é bloqueado. |
| RF-017 | Registrar obrigatoriamente a condição de entrada. | Essencial | Atendente/Caixa | Dado um check-in, quando a condição de entrada estiver vazia, então o sistema bloqueia a gravação; texto, usuário e horário ficam vinculados à estadia. |
| RF-018 | Mover veículo entre vagas. | Desejável | Manobrista, Gestor/Financeiro | Dado um veículo em vaga ocupada, quando um usuário autorizado mover para outra vaga livre, então a origem é liberada, o destino é ocupado e o histórico registra origem, destino, usuário e horário. |
| RF-019 | Exibir mapa ou lista de vagas com ocupação. | Essencial | Todos | Dado o painel de vagas, quando houver mudança de ocupação, então o mapa ou lista reflete o novo estado em até 10 segundos e exibe contadores de livres e ocupadas; vagas inativas não aparecem como disponíveis. |
| RF-020 | Liberar a vaga automaticamente. | Essencial | Todos | Dado uma estadia entregue ou cancelada, quando a operação for concluída, então a vaga vinculada volta a livre automaticamente, sem ação manual. |
| RF-021 | Registrar solicitação de retirada. | Essencial | Atendente/Caixa | Dado uma estadia ativa, quando o atendente solicitar a retirada por código ou placa, então o sistema cria uma solicitação com status Solicitada e horário; solicitação ativa duplicada para a mesma estadia é bloqueada. |
| RF-022 | Exibir fila de retiradas. | Essencial | Atendente/Caixa, Manobrista | Dado o painel de retiradas, quando houver solicitações, então o sistema lista por ordem de chegada com placa, vaga, status, tempo de espera e responsável; filtros por status são aplicáveis. |
| RF-023 | Atualizar status da retirada. | Essencial | Manobrista, Atendente/Caixa | Dado uma solicitação Solicitada, quando o manobrista iniciar a busca e o atendente confirmar a entrega, então os status Em busca e Entregue são registrados com horário e usuário; saltos de status e reversão são bloqueados. |
| RF-024 | Atribuir manobrista à solicitação. | Importante | Atendente/Caixa, Gestor/Financeiro | Dado uma solicitação, quando o atendente atribuir um manobrista ativo, então a fila exibe o responsável e a atribuição é registrada. |
| RF-025 | Bloquear entrega sem pagamento integral ou isenção. | Essencial | Atendente/Caixa | Dado uma estadia com cobrança em aberto e sem isenção, quando tentar registrar a entrega, então o sistema bloqueia e informa pagamento pendente; após pagamento integral ou isenção autorizada, a entrega é permitida. |
| RF-026 | Registrar obrigatoriamente a condição de saída. | Essencial | Atendente/Caixa, Manobrista | Dado a confirmação de entrega, quando a condição de saída estiver vazia, então o sistema bloqueia a conclusão; texto, usuário e horário ficam registrados. |
| RF-027 | Calcular o tempo de retirada. | Essencial | Atendente/Caixa, Gestor/Financeiro | Dado uma solicitação entregue, quando o sistema calcular o tempo, então exibe a duração entre solicitação e entrega em minutos e inclui o valor nas médias por período. |
| RF-028 | Cancelar solicitação de retirada. | Desejável | Atendente/Caixa, Gestor/Financeiro | Dado uma solicitação não entregue, quando o atendente cancelar com motivo, então a solicitação fica Cancelada, a estadia volta a Aberta e o evento é auditado. |
| RF-029 | Tratar ticket perdido com conferência e autorização. | Essencial | Atendente/Caixa, Gestor/Financeiro | Dado um cliente sem ticket, quando o atendente buscar pela placa, conferir o documento com o cadastro e um Gestor ou Administrador autorizar com motivo, então o ticket é marcado como perdido, a liberação é permitida e a ação é auditada; documento divergente bloqueia o fluxo. |
| RF-030 | Calcular permanência e diárias automaticamente. | Essencial | Atendente/Caixa | Dado uma estadia com entrada e saída, quando o sistema calcular, então aplica diárias iguais ao maior valor inteiro entre 1 e o teto de (duração em horas dividida por 24); 24 horas exatas geram 1 diária e 24 horas e 1 segundo geram 2. |
| RF-031 | Exibir a composição do cálculo. | Essencial | Atendente/Caixa | Dado o checkout, quando o cálculo for exibido, então mostra entrada, saída ou horário atual, duração, quantidade de diárias, valor unitário, subtotal, desconto e total; o valor unitário é o da tarifa vigente na entrada. |
| RF-032 | Registrar pagamento integral. | Essencial | Atendente/Caixa | Dado uma cobrança em aberto, quando o atendente registrar o valor integral com forma dinheiro, débito, crédito ou Pix, então a cobrança fica Paga com data, hora e operador; valor diferente do total sem desconto autorizado é rejeitado. |
| RF-033 | Impedir pagamento duplicado. | Essencial | Atendente/Caixa | Dado uma cobrança já paga, quando tentar registrar novo pagamento, então o sistema bloqueia; após estorno autorizado, um novo pagamento pode ser registrado como evento distinto. |
| RF-034 | Emitir comprovante simples de pagamento ou isenção. | Essencial | Atendente/Caixa | Dado um pagamento ou isenção, quando o comprovante for emitido, então contém número sequencial, dados do veículo, período, diárias, valor, desconto, forma de pagamento e operador, com opção de impressão ou PDF; reimpressão mantém o número. |
| RF-035 | Aplicar desconto ou isenção com autorização. | Essencial | Gestor/Financeiro, Administrador | Dado uma cobrança em aberto, quando um Gestor ou Administrador aplicar desconto ou isenção com motivo, então o total é recalculado sem ficar negativo, a autorização é auditada e a isenção total permite entrega sem pagamento. |
| RF-036 | Registrar estorno de pagamento com autorização. | Essencial | Gestor/Financeiro, Administrador | Dado um pagamento registrado, quando um Gestor ou Administrador autorizar estorno integral com motivo, então o pagamento fica Estornado, o valor sai do faturamento líquido e o evento é auditado; a entrega já realizada não é revertida. |
| RF-037 | Consultar cobranças pagas, em aberto, isentas e estornadas. | Essencial | Atendente/Caixa, Gestor/Financeiro | Dado um período, placa ou status, quando consultar cobranças, então o sistema lista os registros com totais por forma de pagamento e destaca veículos ativos com valor em aberto. |
| RF-038 | Registrar o fechamento diário de caixa. | Essencial | Atendente/Caixa, Gestor/Financeiro | Dado o fim do dia, quando o operador registrar o fechamento com valores informados por forma de pagamento, então o sistema compara com os pagamentos do dia, calcula divergências, grava o fechamento e permite imprimir ou exportar o resumo; só existe um fechamento por data. |
| RF-039 | Reabrir fechamento de caixa. | Importante | Administrador | Dado um fechamento concluído, quando um Administrador reabrir com justificativa, então a versão anterior é preservada, o dia volta a aceitar ajustes e a reabertura é auditada. |
| RF-040 | Exibir painel operacional. | Essencial | Todos | Dado o painel operacional, quando aberto, então exibe veículos no pátio, vagas livres e ocupadas, fila por status, indicador de pendência e, para perfis com permissão financeira, valores em aberto; os dados refletem alterações em até 10 segundos. |
| RF-041 | Gerar relatórios por dia, semana e mês. | Essencial | Gestor/Financeiro, Administrador | Dado um filtro de período, quando gerar relatórios, então exibe faturamento líquido, veículos atendidos, permanência média, horários de pico, taxa de ocupação, tempo médio de retirada, descontos, estornos, valores em aberto e divergências de caixa, conforme as regras de negócio. |
| RF-042 | Exportar relatórios em CSV. | Essencial | Gestor/Financeiro, Administrador | Dado um relatório filtrado, quando exportar em CSV, então o arquivo é gerado em UTF-8 com separador ponto e vírgula, contém as colunas e totais exibidos, respeita as permissões do usuário e neutraliza fórmulas maliciosas. |
| RF-043 | Registrar operação de contingência após queda de internet. | Essencial | Atendente/Caixa, Gestor/Financeiro | Dado uma indisponibilidade de internet, quando a conexão retornar, então um usuário autorizado registra a operação com horários reais, justificativa e marcação de contingência, sujeita à aprovação de Gestor ou Administrador; só após aprovação o registro entra nos relatórios e no fechamento. |
| RF-044 | Consultar histórico de entregas e tempos. | Importante | Gestor/Financeiro | Dado um período, quando consultar o histórico de entregas, então o sistema lista estadias entregues com horários de solicitação e entrega, duração, manobrista e operador; filtros por período e placa são aplicáveis. |

## 8. Requisitos não funcionais

| ID | Categoria | Requisito | Métrica/Critério | Prioridade |
| --- | --- | --- | --- | --- |
| RNF-001 | Desempenho | O check-in e a busca de veículos devem responder rapidamente em condições normais de uso. | Percentil 95 do check-in e da busca em até 2 segundos com até 20 usuários simultâneos. | Essencial |
| RNF-002 | Desempenho | Os relatórios devem responder em prazos previsíveis. | Até 5 segundos para períodos de até 12 meses e até 15 segundos para até 5 anos. | Essencial |
| RNF-003 | Concorrência | O sistema deve suportar operação simultânea sem perder atualizações. | Pelo menos 20 usuários simultâneos e nenhuma dupla ocupação de vaga em teste de concorrência. | Essencial |
| RNF-004 | Disponibilidade | O sistema deve estar disponível durante o horário de operação. | Pelo menos 99,5% ao mês, excluídas janelas de manutenção programada; horário a confirmar em QA-002. | Essencial |
| RNF-005 | Recuperação | Os dados devem ser recuperáveis após falha. | Backup completo diário e incremental a cada 1 hora, RPO de até 1 hora, RTO de até 4 horas e teste de restauração trimestral. | Essencial |
| RNF-006 | Segurança | O acesso deve ser autenticado e protegido. | TLS 1.2 ou superior, senhas com hash, bloqueio após 5 tentativas por 15 minutos, sessão expirada após 30 minutos de inatividade e nenhum dado sensível em logs. | Essencial |
| RNF-007 | Autorização | As permissões devem ser aplicadas no servidor. | Toda requisição é validada por perfil e permissão; acesso negado por padrão; a interface não é a única barreira. | Essencial |
| RNF-008 | Auditoria | A trilha de auditoria deve ser íntegra e consultável. | Registros somente de acréscimo, sem edição ou exclusão pela aplicação, retidos por 5 anos, com filtros por usuário, data, ação e entidade. | Essencial |
| RNF-009 | Privacidade | Tratar dados pessoais com minimização e proteção. | Documento mascarado em listagens para perfis sem permissão, criptografia em repouso e em trânsito, nenhum dado de cartão armazenado. | Essencial |
| RNF-010 | Retenção | Cumprir os prazos de retenção definidos. | Registros financeiros por 5 anos; operacionais por 2 anos, seguidos de anonimização; auditoria por 5 anos. | Essencial |
| RNF-011 | Usabilidade | A operação principal deve ser rápida e aprender fácil. | Check-in concluído em até 60 segundos por usuário treinado; treinamento inicial de até 30 minutos; mensagens de erro acionáveis. | Essencial |
| RNF-012 | Acessibilidade | As telas principais devem ser acessíveis. | Conformidade com WCAG 2.1 nível AA nos fluxos de check-in, retirada, cobrança e fechamento; navegação por teclado e contraste adequado. | Importante |
| RNF-013 | Compatibilidade | O sistema deve funcionar nos dispositivos da operação. | Últimas versões de Chrome, Edge, Firefox e Safari; Android e iOS; layout responsivo a partir de 360 px de largura. | Essencial |
| RNF-014 | Observabilidade | Falhas devem ser detectáveis e diagnosticáveis. | Logs estruturados com correlação, health check, métricas de erro e alertas para indisponibilidade e falha de impressão. | Importante |
| RNF-015 | Manutenibilidade | O sistema deve ser evolutivo e testável. | Contrato OpenAPI atualizado, testes automatizados nos fluxos críticos e migrações versionadas de banco. | Importante |
| RNF-016 | Impressão | O ticket deve ser impresso com fallback. | Suporte a impressora térmica de 80 mm, reimpressão pelo mesmo código e exibição de código/QR na tela quando a impressão falhar. | Essencial |
| RNF-017 | Contingência | A operação manual deve ser conciliável. | Registros de contingência lançados em até 24 horas e antes do fechamento seguinte, com justificativa e aprovação. | Essencial |
| RNF-018 | Escalabilidade | O sistema deve suportar crescimento inicial sem redesenho. | Pelo menos 500 vagas, 5.000 estadias por mês e 50 usuários cadastrados, com margem para expansão; dimensionamento proposto a validar em QA-012. | Importante |
| RNF-019 | Localização | O sistema deve operar no contexto brasileiro. | Interface em português do Brasil, moeda BRL, fuso America/Sao_Paulo e armazenamento de horários em UTC. | Essencial |
| RNF-020 | Exportação | Os arquivos exportados devem ser compatíveis com planilhas. | CSV em UTF-8 com BOM, separador ponto e vírgula, colunas estáveis e proteção contra injeção de fórmulas. | Essencial |
| RNF-021 | Conformidade fiscal | O comprovante não deve ser confundido com documento fiscal. | Comprovante identificado como não fiscal; emissão de NFS-e fora do escopo do MVP. | Essencial |

## 9. Regras de negócio

| ID | Regra | Condição | Comportamento | Exceções |
| --- | --- | --- | --- | --- |
| RN-001 | Cálculo de diárias | Toda estadia com duração maior que zero | Diárias = maior valor inteiro entre 1 e o teto da duração em horas dividida por 24; 24 horas exatas = 1 diária; 24 horas e 1 segundo = 2 diárias. | Nenhuma no MVP. |
| RN-002 | Congelamento de tarifa | Check-in concluído | A tarifa aplicada é a vigente no horário de entrada e não muda se o valor for alterado depois. | Registro de contingência usa a tarifa vigente no horário real informado, após aprovação. |
| RN-003 | Pagamento integral | Cobrança em aberto | O pagamento deve ser único e integral; não há pagamento parcial ou dividido. | Isenção total autorizada equivale a cobrança quitada. |
| RN-004 | Liberação do veículo | Tentativa de entrega | A entrega exige cobrança paga ou isenta, condição de saída registrada e solicitação em status Em busca. | Nenhuma. |
| RN-005 | Unicidade de estadia ativa | Check-in | Uma placa normalizada pode ter apenas uma estadia ativa; a normalização remove espaços e hífens e converte para maiúsculas. | Após entrega ou cancelamento, a placa pode gerar nova estadia. |
| RN-006 | Ocupação de vaga | Check-in | Cada estadia ativa ocupa exatamente uma vaga ativa; apenas vagas livres podem ser selecionadas; uma vaga não pode ter dois veículos ativos. | Movimentação autorizada entre vagas. |
| RN-007 | Lotação | Check-in | Com zero vagas ativas livres, o check-in é bloqueado. | Nenhuma. |
| RN-008 | Ticket perdido | Cliente sem ticket | Exige busca por placa, conferência do documento com o cadastro, autorização de Gestor ou Administrador e motivo registrado; a cobrança não muda. | Documento divergente bloqueia o fluxo e deve ser escalado. |
| RN-009 | Ações sensíveis | Desconto, isenção, estorno, cancelamento, reabertura e ticket perdido | Exigem perfil Gestor/Financeiro ou Administrador, motivo obrigatório e registro em auditoria. | Nenhuma. |
| RN-010 | Estorno | Pagamento já registrado | O estorno do MVP é integral; o valor deixa o faturamento líquido; a entrega já realizada permanece registrada. | Estorno parcial fica fora do MVP. |
| RN-011 | Transições de status | Estadia e solicitação de retirada | Estadia: Aberta, Retirada solicitada, Em busca, Entregue ou Cancelada. Solicitação: Solicitada, Em busca, Entregue ou Cancelada. Não há salto nem reversão. | Cancelamento de solicitação devolve a estadia para Aberta. |
| RN-012 | Identificador de ticket | Emissão e reimpressão | O código do ticket é único e não reutilizável; a reimpressão mantém o mesmo código. | Nenhuma. |
| RN-013 | Fechamento diário | Encerramento do dia | Existe um fechamento por data; considera pagamentos e estornos pela data do evento; pode ser concluído com veículos ativos, pois estes ainda não são receita. | Novo pagamento na data fechada exige reabertura. |
| RN-014 | Reabertura de caixa | Fechamento já concluído | Somente Administrador reabre, com justificativa; a versão anterior é preservada e a reabertura é auditada. | Nenhuma. |
| RN-015 | Taxa de ocupação | Cálculo de relatório | Ocupação = soma, em horas-vaga, do tempo em que cada vaga esteve ocupada no período, dividida pelo produto entre vagas ativas e duração do período. | Vagas inativas são excluídas do denominador. |
| RN-016 | Horário de pico | Cálculo de relatório | Contagem de check-ins por hora cheia no período, ordenada do maior para o menor. | Nenhuma. |
| RN-017 | Faturamento líquido | Cálculo de relatório | Faturamento líquido = pagamentos menos estornos no período; descontos são exibidos separadamente; valores em aberto não compõem receita. | Isenções não geram receita. |
| RN-018 | Médias operacionais | Cálculo de relatório | Permanência média usa estadias entregues pela data de saída; tempo médio de retirada usa solicitações entregues pela data de entrega. | Nenhuma. |
| RN-019 | Horários e fuso | Todo registro | Horários são armazenados em UTC e exibidos em America/Sao_Paulo. | Nenhuma. |
| RN-020 | Contingência | Operação lançada após queda de internet | O registro usa horários reais, recebe marcação de contingência e justificativa, exige aprovação e só então entra em relatórios e fechamento. | Registros não aprovados ficam pendentes e não podem ser usados no fechamento. |
| RN-021 | Horário de entrada | Check-in normal | O horário é o timestamp do servidor e não pode ser editado pelo atendente. | Contingência permite informar horário real com aprovação. |
| RN-022 | Movimentação de vaga | Mudança de vaga | A vaga de destino deve estar livre; origem, destino, usuário e horário são registrados. | Vaga de destino igual à origem é rejeitada. |
| RN-023 | Vaga inativa | Configuração de vagas | Vaga inativa não pode ser usada nem desativada enquanto estiver ocupada. | Nenhuma. |
| RN-024 | Retenção e descarte | Prazo de retenção atingido | Registros financeiros ficam 5 anos; registros operacionais ficam 2 anos e depois têm nome e documento anonimizados, preservando-se nos registros financeiros apenas os dados necessários à obrigação legal; auditoria fica 5 anos. | Obrigação legal ou solicitação de autoridade pode estender o prazo. |
| RN-025 | Numeração de comprovante | Emissão de comprovante | A numeração é sequencial e não reutilizável; comprovantes de cobranças estornadas mantêm o número e registram o estorno. | Nenhuma. |

## 10. Modelo de dados conceitual

### 10.1 Entidades e relacionamentos

```text
[Unidade] 1 --- N [Vaga]
[Unidade] 1 --- N [Tarifa]
[Perfil] 1 --- N [Usuario]
[Veiculo] 1 --- N [Estadia]
[Vaga] 1 --- N [Estadia]              (histórico; no máximo uma ativa por vaga)
[Estadia] 1 --- N [RegistroCondicao]  (entrada e saída)
[Estadia] 1 --- N [SolicitacaoRetirada]
[Estadia] 1 --- 0..1 [Cobranca]
[Cobranca] 1 --- N [EventoFinanceiro] (desconto, isenção, estorno)
[Cobranca] 1 --- 0..1 [Pagamento]
[Cobranca] 1 --- 0..1 [Comprovante]
[FechamentoCaixa] 1 --- N [Pagamento] (por data do evento)
[Usuario] 1 --- N [Auditoria]
```

### 10.2 Entidades principais

| Entidade | Atributos principais | Observações |
| --- | --- | --- |
| Unidade | Nome, identificação, capacidade, configurações | Uma única unidade no MVP. |
| Vaga | Identificador, estado, ativo | Estado livre ou ocupada; histórico de ocupação. |
| Tarifa | Valor da diária, início de vigência, usuário, data de criação | Vigência sem sobreposição; histórico imutável. |
| Usuario | Nome, e-mail, perfil, estado, senha | Autenticação pelo ASP.NET Core Identity. |
| Perfil | Nome, permissões | Quatro perfis fixos. |
| Veiculo | Placa normalizada, modelo | Pode ter várias estadias ao longo do tempo. |
| Estadia | Placa, modelo, nome e documento do responsável, entrada, saída, vaga, status, tarifa congelada | Registro central da operação. |
| RegistroCondicao | Tipo (entrada/saída), texto, usuário, horário | Substitui fotografias no MVP. |
| SolicitacaoRetirada | Status, horário da solicitação, horário de início da busca, horário de entrega, manobrista, motivo de cancelamento | Uma estadia pode ter solicitações canceladas e uma nova. |
| Cobranca | Duração, diárias, valor unitário, subtotal, desconto, total, status | Status: Em aberto, Paga, Isenta, Estornada. |
| Pagamento | Valor, forma, data e hora, operador, status | Sem dados de cartão. |
| EventoFinanceiro | Tipo, valor, motivo, autorizador, data e hora | Desconto, isenção e estorno. |
| Comprovante | Número sequencial, dados impressos, data de emissão | Não fiscal. |
| FechamentoCaixa | Data, valores esperados e informados por forma, divergência, status, justificativa | Um por data, com versionamento na reabertura. |
| Auditoria | Usuário, ação, entidade, valores antes/depois, motivo, data e hora | Somente acréscimo. |

### 10.3 Ciclo de vida

```text
Estadia:
Aberta --> Retirada solicitada --> Em busca --> Entregue
Aberta --> Cancelada
Retirada solicitada --> Aberta (solicitação cancelada)

Cobrança:
Em aberto --> Paga --> Estornada
Em aberto --> Isenta

Fechamento:
Aberto --> Fechado --> Reaberto --> Fechado
```

## 11. Integrações e dependências

| Integração/Dependência | Tipo | Direção | Dados trocados | Criticidade | Observações |
| --- | --- | --- | --- | --- | --- |
| Impressora térmica de 80 mm | Hardware local | Saída | Ticket e comprovante | Alta | Falha de impressão não pode bloquear o check-in; código/QR na tela é o fallback. |
| ASP.NET Core Identity | Componente interno | Ambos | Credenciais, perfis e sessão | Alta | Já previsto na stack do projeto. |
| SQL Server | Persistência | Ambos | Todos os dados operacionais e financeiros | Alta | Backup e restauração conforme RNF-005. |
| Navegadores e dispositivos | Cliente | Entrada | Interface web | Alta | Desktop no balcão e celular/tablet para manobristas. |
| Internet | Infraestrutura | Ambos | Tráfego da aplicação | Alta | Queda tratada por contingência manual. |
| Exportação CSV | Arquivo | Saída | Relatórios e fechamentos | Média | UTF-8 com BOM e separador ponto e vírgula. |
| OpenAPI/Swagger | Documentação | Saída | Contratos da API | Média | Base para futuras integrações. |
| Gateway de pagamento | Não previsto | — | — | — | Fora do escopo; pagamentos apenas registrados. |
| Emissor de NFS-e | Não previsto | — | — | — | Fora do escopo; comprovante não fiscal. |
| LPR, cancela ou sensores | Não previsto | — | — | — | Fora do escopo do MVP. |

## 12. Segurança, privacidade e conformidade

### 12.1 Autenticação e autorização

- Autenticação por e-mail e senha com ASP.NET Core Identity, senha com hash, bloqueio temporário após tentativas inválidas e expiração de sessão por inatividade.
- Autorização no servidor por perfil e permissão, com negação por padrão.
- Quatro perfis fixos: Administrador, Gestor/Financeiro, Atendente/Caixa e Manobrista.

| Módulo | Administrador | Gestor/Financeiro | Atendente/Caixa | Manobrista |
| --- | --- | --- | --- | --- |
| Painel operacional | Ver | Ver | Ver | Ver, sem valores financeiros |
| Check-in | Ver e operar | Ver | Operar | Não |
| Veículos e vagas | Ver e operar | Ver | Ver e operar | Ver e atualizar status |
| Retiradas | Ver e operar | Ver e autorizar | Ver e operar | Ver e atualizar |
| Cobrança e pagamentos | Ver e operar | Ver e autorizar | Operar | Ver apenas indicador pago/pendente |
| Descontos e estornos | Autorizar | Autorizar | Não autoriza | Não |
| Fechamento de caixa | Ver, fechar e reabrir | Ver e fechar | Ver e fechar | Não |
| Relatórios | Ver e exportar | Ver e exportar | Não | Não |
| Tarifas e vagas | Configurar | Ver | Ver | Ver |
| Usuários | Gerenciar | Não | Não | Não |
| Auditoria | Ver | Ver | Não | Não |

### 12.2 Dados pessoais e LGPD

| Dado | Finalidade | Base legal indicada | Retenção | Acesso |
| --- | --- | --- | --- | --- |
| Nome do responsável | Identificar o cliente e comprovar a retirada | Execução de contrato | 2 anos para o vínculo operacional, com anonimização após o prazo; nos registros financeiros, apenas o necessário para a obrigação legal de 5 anos | Atendente, Gestor, Administrador |
| Documento do responsável | Comprovar propriedade, inclusive em ticket perdido | Execução de contrato | 2 anos para o vínculo operacional, com anonimização após o prazo; nos registros financeiros, apenas o necessário para a obrigação legal de 5 anos | Atendente, Gestor, Administrador; mascarado em listagens |
| Placa e modelo do veículo | Operação, localização e histórico | Execução de contrato | 2 anos para o vínculo operacional; 5 anos para registros financeiros | Operação e gestão |
| Horários de entrada, saída e retirada | Operação, cálculo e indicadores | Execução de contrato | 2 anos | Operação e gestão |
| Condições do veículo em texto | Comprovação de estado e tratamento de reclamações | Legítimo interesse, com avaliação de impacto | 2 anos | Operação e gestão |
| Pagamentos: valor, forma e data | Controle financeiro e obrigações legais | Execução de contrato e obrigação legal | 5 anos | Atendente, Gestor e Administrador |
| Auditoria de ações | Segurança e rastreabilidade | Legítimo interesse e obrigação legal | 5 anos | Administrador e Gestor |

- Não são coletadas fotografias, dados de cartão, biometria ou localização em tempo real no MVP.
- Dados pessoais não devem aparecer em logs de aplicação; documentos são mascarados em listagens e relatórios para perfis sem permissão.
- O titular pode solicitar acesso, correção e informações sobre tratamento; exclusão fica limitada pelos prazos legais e contratuais.
- A base legal e a política de retenção devem ser validadas juridicamente antes do go-live (QA-007).

### 12.3 Auditoria e tratamento de incidentes

- Ações sensíveis geram registro com usuário, data e hora, entidade, motivo e valores antes/depois.
- A trilha é somente de acréscimo e não pode ser alterada pela aplicação.
- Acesso à auditoria é restrito a Administrador e Gestor/Financeiro.
- Incidentes de segurança devem ter plano de resposta, comunicação ao encarregado de dados e registro de evidências.
- Backups devem ser protegidos e testados periodicamente.

## 13. UX e requisitos de interface (alto nível)

### 13.1 Princípios

- Priorizar velocidade no balcão: check-in e cobrança com o menor número de passos possível.
- Tornar o estado da operação evidente: vaga ocupada, veículo aguardando, pagamento pendente e entrega bloqueada devem ser visíveis sem ambiguidade.
- Prevenir erros: validação de placa, confirmação de ações irreversíveis e bloqueio de entrega sem pagamento.
- Não esconder regras: a composição do cálculo deve ser compreensível para o atendente explicar ao cliente.
- Funcionar bem em desktop, tablet e celular, com alvos de toque adequados.

### 13.2 Telas e áreas principais

| Tela/Área | Objetivo | Estados principais |
| --- | --- | --- |
| Login | Autenticar e direcionar ao painel do perfil | Vazio, carregando, credencial inválida, bloqueado |
| Painel operacional | Mostrar pátio, vagas, fila, pendências e atalhos, com valores financeiros apenas para perfis autorizados | Carregando, sem veículos, operação normal, erro de atualização |
| Check-in | Registrar veículo, responsável, vaga e condição | Formulário vazio, validação, vaga ocupada, lotação, impressão falhou, sucesso |
| Pátio e vagas | Visualizar mapa/lista e localizar veículos | Todas livres, parcialmente ocupado, lotado, vaga inativa |
| Busca de veículos | Localizar estadia por placa, ticket, nome ou documento | Sem resultados, um resultado, múltiplos resultados, documento mascarado |
| Detalhe da estadia | Ver dados, condições, cobrança e linha do tempo | Aberta, aguardando retirada, em busca, entregue, cancelada |
| Fila de retiradas | Priorizar e acompanhar pedidos | Vazia, com solicitações, atrasada, entregue |
| Checkout | Calcular, aplicar desconto, registrar pagamento e emitir comprovante | Em aberto, desconto pendente de autorização, pago, isento, estornado |
| Ticket perdido | Conferir documento e autorizar liberação | Documento confere, divergente, aguardando autorização, autorizado |
| Fechamento de caixa | Conferir valores e encerrar o dia | Aberto, divergência, fechado, reaberto |
| Relatórios | Analisar operação e finanças por período | Carregando, sem dados, com dados, exportando |
| Administração | Gerenciar tarifa, vagas, usuários e auditoria | Lista vazia, formulário, conflito de vigência, usuário desativado |

### 13.3 Conteúdo de ticket e comprovante

- **Ticket de entrada:** nome da unidade, código único, QR, placa, modelo, vaga, data e hora de entrada, condição registrada e orientação para não perder o ticket.
- **Comprovante:** número sequencial, identificação de não fiscal, placa, período, quantidade de diárias, valor unitário, subtotal, desconto, total, forma de pagamento, operador, data e hora.

### 13.4 Estados, erros e acessibilidade

- Estados vazios devem orientar a próxima ação, sem telas em branco.
- Carregamentos longos devem exibir indicador e impedir dupla submissão.
- Erros devem informar o que aconteceu e como corrigir, sem expor dados sensíveis.
- Ações irreversíveis exigem confirmação e, quando aplicável, autorização.
- Fluxos principais devem ser navegáveis por teclado, com rótulos, foco visível e contraste adequado.
- O layout deve se adaptar a telas a partir de 360 px e a uso em movimento no pátio.

## 14. Riscos e mitigações

| ID | Risco | Probabilidade | Impacto | Mitigação | Plano de contingência |
| --- | --- | --- | --- | --- | --- |
| R-001 | Queda de internet interrompe o atendimento | Média | Alto | Contingência manual documentada, treinamento e lançamento com aprovação | Registrar em controle manual e conciliar antes do fechamento seguinte |
| R-002 | Falha da impressora térmica | Média | Médio | Teste periódico, fallback de código/QR na tela e reimpressão | Entregar ticket com código na tela e reimprimir depois |
| R-003 | Divergência entre vaga registrada e vaga real | Média | Alto | Vaga obrigatória, movimentação auditada e conferência periódica | Corrigir com movimentação autorizada e registrar ocorrência |
| R-004 | Tarifa configurada incorretamente | Baixa | Alto | Apenas Administrador altera, vigência obrigatória, prévia do valor e auditoria | Corrigir vigência e revisar cobranças afetadas antes do fechamento |
| R-005 | Fraude ou uso indevido de descontos, isenções e estornos | Média | Alto | Autorização de gestor, motivo obrigatório, auditoria e relatório de eventos | Revisar eventos, estornar indevidos e aplicar medida administrativa |
| R-006 | Erros nos lançamentos de contingência | Alta | Médio | Formulário com horários reais, justificativa e aprovação obrigatória | Reconciliar com controle manual e corrigir antes do fechamento |
| R-007 | Baixa adesão da equipe | Média | Alto | Interface simples, treinamento curto, piloto e suporte no go-live | Manter contingência manual temporária e reforçar treinamento |
| R-008 | Perda de dados por falha técnica | Baixa | Alto | Backup diário, monitoramento e teste de restauração | Restaurar backup e reprocessar operações do período |
| R-009 | Incidente de privacidade ou vazamento | Baixa | Alto | Minimização, mascaramento, criptografia, controle de acesso e logs | Acionar plano de incidentes e comunicação conforme LGPD |
| R-010 | Aumento de escopo durante o MVP | Alta | Médio | Fases explícitas, critérios de aceitação e controle de mudanças | Reavaliar escopo e priorizar apenas itens essenciais |
| R-011 | Falta de baseline para medir ganhos | Alta | Médio | Instrumentar o sistema desde o primeiro dia e medir duas semanas de operação manual | Usar período inicial como linha de base e ajustar metas |
| R-012 | Dupla ocupação de vaga em concorrência | Média | Alto | Transação e restrição de unicidade no banco, teste de concorrência | Corrigir vaga manualmente com auditoria e investigar causa |
| R-013 | Dependência de um único servidor | Média | Alto | Monitoramento, backup e plano de recuperação | Restaurar em ambiente alternativo conforme RTO |
| R-014 | Código de ticket previsível e usado indevidamente | Baixa | Médio | Código não sequencial ou validação por documento, além da conferência no ticket perdido | Bloquear liberação, conferir documento e autorizar com auditoria |
| R-015 | Metas de tempo e reclamações sem dados confiáveis | Alta | Médio | Registrar baseline e revisar metas após o piloto | Manter metas como faixa de acompanhamento até dados suficientes |

## 15. Fases de entrega

| Fase | Escopo (IDs de requisitos) | Resultado esperado | Dependências |
| --- | --- | --- | --- |
| MVP | RF-001 a RF-010; RF-012 a RF-017; RF-019 a RF-023; RF-025 a RF-027; RF-029 a RF-038; RF-040 a RF-043 | Operação completa de check-in, vagas, retirada, cobrança, pagamento, fechamento diário, painel, relatórios e auditoria, com perfis e contingência | Configuração de vagas e tarifa; impressora térmica; validação jurídica; baseline inicial |
| Fase 2 | RF-011; RF-024; RF-039; RF-044 | Reimpressão, atribuição de manobrista, reabertura de caixa e histórico de entregas | MVP em operação e feedback da equipe |
| Fase 3 | RF-018; RF-028 | Movimentação de veículos entre vagas e cancelamento de solicitação de retirada | Fase 2 concluída |
| Futuro (fora do escopo atual) | Autoatendimento do cliente, integração de pagamento, NFS-e, fotografias, múltiplas unidades, offline e LPR | Evolução conforme prioridade do negócio | Novo PRD e validação específica |

## 16. Critérios de sucesso e Definition of Done

- Todos os requisitos Essenciais do MVP implementados e aceitos com critérios verificáveis.
- Teste de aceitação com os quatro perfis, cobrindo check-in, retirada, ticket perdido, pagamento, desconto, estorno e fechamento.
- Piloto em operação real por pelo menos duas semanas, com contingência manual disponível.
- Nenhum defeito crítico ou bloqueante aberto no fim do piloto.
- Tarifa, vagas, usuários e permissões configurados e validados pelo negócio.
- Impressora térmica homologada e fallback de ticket em tela testado.
- Backup e restauração testados; RPO e RTO atendidos.
- Política de retenção e anonimização configurada e revisada juridicamente.
- Trilha de auditoria validada para todas as ações sensíveis.
- Relatórios e exportação CSV conferidos contra o fechamento manual de um período de referência.
- Equipe treinada e material de apoio disponível.
- Métricas de tempo de retirada, divergência de caixa, volume de atendimentos e ocupação sendo coletadas desde o primeiro dia.
- Critério de validação: o gestor consegue explicar a composição de uma cobrança, localizar um veículo e fechar o caixa sem depender de planilhas paralelas.

## 17. Questões abertas

| ID | Questão | Impacto | Responsável | Prazo |
| --- | --- | --- | --- | --- |
| QA-001 | Quais são as metas de tempo médio de retirada e de redução de reclamações, e qual o baseline atual? | Metas de OBJ-002 e OBJ-008 | Alexandre Rech e operação | Antes do piloto |
| QA-002 | Qual é o horário oficial de operação para medir disponibilidade? | Meta de OBJ-010 e RNF-004 | Alexandre Rech | Antes do MVP |
| QA-003 | Quantas vagas existem hoje e quais identificadores serão usados? | Configuração inicial e capacidade | Operação | Antes do MVP |
| QA-004 | Qual é o modelo da impressora térmica e ela é compatível com 80 mm? | RF-010, RNF-016 e risco R-002 | Operação e TI | Antes do MVP |
| QA-005 | As formas de pagamento são apenas dinheiro, débito, crédito e Pix? Há necessidade de pagamento dividido? | RF-032 e RN-003 | Alexandre Rech | Antes do MVP |
| QA-006 | Quais limites de desconto cada gestor pode aprovar? | RF-035 e risco R-005 | Alexandre Rech | Antes do MVP |
| QA-007 | A retenção de 5 anos financeiro e 2 anos operacional está correta juridicamente? Como anonimizar sem perder o histórico financeiro? | RNF-010, RN-024 e seção 12 | Jurídico e Alexandre Rech | Antes do go-live |
| QA-008 | Existe necessidade de migrar planilhas ou registros históricos? | Escopo de implantação | Alexandre Rech | Antes do MVP |
| QA-009 | Estorno parcial será necessário no futuro? | RN-010 e RF-036 | Gestão/financeiro | Fase 2 |
| QA-010 | Há intenção de emitir NFS-e futuramente? | Integrações e RNF-021 | Alexandre Rech | Fase 2 |
| QA-011 | Quem aprovará registros de contingência: Gestor/Financeiro ou Administrador? | RF-043 e RN-020 | Alexandre Rech | Antes do MVP |
| QA-012 | Qual o volume esperado de estadias por mês e de usuários simultâneos? | RNF-003 e RNF-018 | Operação | Antes do MVP |
| QA-013 | O comprovante precisa de assinatura ou identificação adicional? | RF-034 | Gestão/financeiro | Antes do MVP |
| QA-014 | Há requisitos de integração contábil ou conciliação bancária no futuro? | Seção 11 e roadmap | Gestão/financeiro | Fase 2 |
| QA-015 | Qual é o prazo e o orçamento disponíveis para o MVP? | Planejamento das fases | Alexandre Rech | Antes do início do MVP |

## 18. Glossário

| Termo | Definição |
| --- | --- |
| Valet | Serviço em que um funcionário recebe, estaciona e devolve o veículo ao cliente. |
| Check-in | Registro de entrada do veículo, responsável, vaga e condição inicial. |
| Checkout | Etapa de cálculo, cobrança e liberação do veículo para entrega. |
| Estadia | Período entre a entrada e a entrega ou cancelamento de um veículo. |
| Diária | Unidade de cobrança correspondente a cada período iniciado de 24 horas contado da entrada. |
| Tarifa | Valor configurável de uma diária, com vigência. |
| Vaga | Posição física do pátio onde um veículo é estacionado. |
| Ticket | Identificação única entregue ao cliente na entrada, com código e QR. |
| Solicitação de retirada | Pedido do cliente para receber o veículo, com status e horários. |
| Manobrista | Funcionário que localiza, move e entrega os veículos. |
| Atendente/Caixa | Funcionário que recebe o cliente, registra operações e cobra. |
| Gestor/Financeiro | Perfil responsável por autorizações, conferência e relatórios. |
| Fechamento de caixa | Conferência diária dos valores recebidos por forma de pagamento. |
| Divergência | Diferença entre o valor esperado pelo sistema e o valor informado na conferência. |
| Contingência | Registro manual temporário usado quando a internet está indisponível. |
| Isenção | Desconto de 100% autorizado que dispensa o pagamento e permite a entrega; no MVP, a isenção que libera a entrega é total. |
| Estorno | Devolução integral de um pagamento registrado. |
| LGPD | Lei Geral de Proteção de Dados Pessoais. |
| RPO | Recovery Point Objective: perda máxima aceitável de dados. |
| RTO | Recovery Time Objective: tempo máximo aceitável de recuperação. |
| CSV | Formato de arquivo de texto separado por delimitadores, usado para exportação. |
| MoSCoW | Classificação de prioridade: Essencial, Importante, Desejável e Fora do escopo. |

## 19. Anexo A - Registro da entrevista

As perguntas abaixo foram feitas com a ferramenta `question` do ambiente. A primeira apresentação da rodada 1 foi dispensada pelo usuário e reapresentada na sequência; todas as 20 perguntas foram respondidas e as decisões estão refletidas no documento.

| ID | Pergunta | Resposta / Decisão | Requisitos impactados | Status |
| --- | --- | --- | --- | --- |
| ENT-001 | O produto será usado em uma única unidade ou deve nascer preparado para múltiplas unidades/empresas? | Uma unidade própria. | RF-005, RF-040, seção 5.4 | Confirmada |
| ENT-002 | Quem usa o sistema no MVP? | Somente equipe interna; cliente não acessa o sistema. | RF-002, RF-003, seção 5.2 | Confirmada |
| ENT-003 | Como calcular o valor quando o carro passa de um dia? | Cada período iniciado de 24 horas, contado da entrada, gera uma diária completa; preço configurável. | RF-030, RF-031, RN-001, RN-002 | Confirmada |
| ENT-004 | O sistema deve apenas registrar pagamentos ou também processá-los? | Apenas registrar os pagamentos recebidos. | RF-032, RF-037, seção 11 | Confirmada |
| ENT-005 | Quais perfis de acesso o MVP deve ter? | Quatro perfis fixos: Administrador, Gestor/Financeiro, Atendente/Caixa e Manobrista. | RF-002, RF-003, seção 12.1 | Confirmada |
| ENT-006 | Quais dados mínimos devem ser registrados na entrada? | Placa, modelo, nome e documento do responsável, horário de entrada e ticket com identificador único. | RF-008, RF-009, RF-014 | Confirmada |
| ENT-007 | Como tratar a retirada quando o cliente perde o ticket? | Busca por placa, conferência de documento e autorização de gestor. | RF-029, RN-008 | Confirmada |
| ENT-008 | O sistema deve registrar em qual vaga cada veículo está? | Vaga obrigatória no check-in, com controle de livre/ocupada e liberação na saída. | RF-012, RF-019, RF-020, RN-006 | Confirmada |
| ENT-009 | Quando e como documentar as condições do veículo? | Registro obrigatório em texto na entrada e na saída; sem fotografias no MVP. | RF-017, RF-026, seção 5.2 | Confirmada |
| ENT-010 | O veículo pode ser entregue sem pagamento? | Não; pagamento integral obrigatório para liberar a entrega. | RF-025, RN-004 | Confirmada |
| ENT-011 | O preço da diária será um valor único ou uma tabela? | Valor único com vigência e histórico de alterações. | RF-007, RN-002 | Confirmada |
| ENT-012 | Descontos e isenções devem existir no MVP? | Sim, somente com autorização de gestor, motivo e registro. | RF-035, RN-009 | Confirmada |
| ENT-013 | O fechamento de caixa deve ser uma funcionalidade do MVP? | Sim, fechamento diário. | RF-038, RF-039, RN-013 | Confirmada |
| ENT-014 | Em quais dispositivos a equipe vai operar? | Balcão em desktop e celular/tablet para manobristas, com interface responsiva. | RNF-013, seção 13 | Confirmada |
| ENT-015 | O que deve acontecer se a internet cair? | Operação online com contingência manual lançada depois. | RF-043, RNF-017, RN-020 | Confirmada |
| ENT-016 | Qual o alcance dos relatórios no MVP? | Dashboard operacional e financeiro com filtros de dia, semana e mês e exportação CSV. | RF-040, RF-041, RF-042 | Confirmada |
| ENT-017 | Por quanto tempo os registros devem ser mantidos? | Registros financeiros por 5 anos e operacionais por 2 anos, com descarte após o prazo. | RNF-010, RN-024, seção 12.2 | Confirmada |
| ENT-018 | O cliente deve receber comprovante no pagamento? | Sim, comprovante simples imprimível ou em PDF. | RF-034, RNF-021 | Confirmada |
| ENT-019 | Como o ticket de entrada deve ser entregue? | Impressão em impressora térmica existente no balcão. | RF-010, RF-011, RNF-016 | Confirmada |
| ENT-020 | Como tratar erros de check-in e estornos? | Cancelamento e estorno somente com autorização de gestor, motivo e auditoria. | RF-016, RF-036, RN-009 | Confirmada |

## 20. Changelog

| Versão | Data | Autor | Mudança |
| --- | --- | --- | --- |
| 1.0 | 2026-11-08 | Agente OpenCode | Criação do documento a partir da entrevista e das decisões de clarificação. |
| 1.1 | 2026-11-08 | Agente OpenCode | Status alterado para Aprovado. |
