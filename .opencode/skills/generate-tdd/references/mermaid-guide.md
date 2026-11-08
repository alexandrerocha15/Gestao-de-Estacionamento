# Guia de diagramas Mermaid para o TDD

Use este guia para escolher, escrever e revisar os diagramas do TDD. Diagramas
existem para explicar relações, sequências, estados e dependências que seriam
mais difíceis de entender apenas em texto ou tabelas.

## Princípios

- **Um diagrama, uma ideia.** Não misture contexto, classes e deploy no mesmo
  desenho.
- **Sempre contextualize.** Todo diagrama deve ter um título ou legenda e um
  parágrafo curto dizendo o que ele mostra e por que importa.
- **Consistência.** Use os mesmos nomes de componentes, entidades e atores do
  restante do documento.
- **Legibilidade.** Prefira até ~15 nós por diagrama. Se ficar grande, divida
  por assunto ou por fluxo.
- **Código versionável.** Mantenha Mermaid dentro de blocos de código com a
  linguagem `mermaid`, nunca como imagem externa.
- **Sem duplicação.** O diagrama deve acrescentar informação; não repita uma
  tabela inteira em forma de desenho.
- **Fonte da verdade.** O diagrama deve refletir as decisões `DEC-XXX` e os
  requisitos; se houver divergência, corrija o diagrama ou registre a mudança.

## Quando usar cada tipo

| Necessidade | Tipo | Exemplo de uso |
| --- | --- | --- |
| Atores, sistemas externos e fronteiras | `flowchart` | Visão de contexto |
| Containers, componentes e dependências | `flowchart` + `subgraph` | Arquitetura da solução |
| Relacionamentos entre entidades | `erDiagram` | Modelo de dados |
| Estrutura de classes/domínio | `classDiagram` | Agregados e serviços de domínio |
| Ordem de chamadas em um fluxo | `sequenceDiagram` | Entrada, retirada, pagamento |
| Ciclo de vida de uma entidade | `stateDiagram-v2` | Estados de veículo, estadia ou pagamento |
| Ambientes e infraestrutura | `flowchart` | Implantação por ambiente |
| Cronograma de fases | `gantt` | Roadmap, quando ajudar |

## Exemplos

### Visão de contexto

```mermaid
flowchart LR
  Cliente(["Cliente"]) --> Sistema["Plataforma de Estacionamento"]
  Atendente(["Atendente"]) --> Sistema
  Sistema --> Pagamento["Gateway de pagamento"]
  Sistema --> Notificacao["Serviço de notificação"]
```

### Containers e componentes

```mermaid
flowchart TB
  subgraph Frontend["Frontend"]
    Web["SPA Angular"]
  end
  subgraph Backend["Backend"]
    Api["API ASP.NET Core"]
    Worker["Worker de jobs"]
  end
  Db[("SQL Server")]
  Web -->|HTTPS/JSON| Api
  Api --> Db
  Worker --> Db
```

### Sequência de um fluxo crítico

```mermaid
sequenceDiagram
  autonumber
  actor Atendente
  participant Web as SPA Angular
  participant Api as API ASP.NET Core
  participant Db as SQL Server
  Atendente->>Web: Registra entrada
  Web->>Api: POST /api/estadias
  Api->>Db: Insere estadia
  Db-->>Api: OK
  Api-->>Web: 201 Created
  Web-->>Atendente: Confirmação
```

### Relacionamentos entre entidades

```mermaid
erDiagram
  CLIENTE ||--o{ VEICULO : possui
  VEICULO ||--o{ ESTADIA : participa
  VAGA ||--o{ ESTADIA : ocupa
  ESTADIA ||--o{ PAGAMENTO : gera
```

### Classes de domínio

```mermaid
classDiagram
  class Estadia {
    +Guid Id
    +DateTime EntradaEm
    +DateTime? SaidaEm
    +Abrir()
    +RegistrarSaida()
  }
  class Veiculo {
    +Guid Id
    +string Placa
  }
  Estadia "1" --> "1" Veiculo
```

### Estados e transições

```mermaid
stateDiagram-v2
  [*] --> Aberta
  Aberta --> EmRetirada: solicitar retirada
  EmRetirada --> Entregue: veículo entregue
  Aberta --> Cancelada: cancelamento
  Entregue --> [*]
```

### Implantação

```mermaid
flowchart TB
  subgraph Producao["Produção"]
    Lb["Load balancer"]
    App["API + SPA"]
    Db[("SQL Server")]
  end
  Usuario["Usuário"] --> Lb --> App --> Db
```

## Boas práticas de sintaxe

- Coloque rótulos com espaços, acentos, parênteses ou barras entre aspas:
  `A["API (produção)"]`.
- Evite acentos e espaços em IDs de nós; use rótulos para o texto visível:
  `api["API de Estacionamento"]`.
- Use `subgraph id["Título"]` para nomear agrupamentos.
- Quebre linhas de rótulos longos com `<br/>`.
- Evite recursos experimentais ou específicos de uma versão do Mermaid.
- Valide o diagrama renderizando-o antes de considerar o TDD concluído.
- Se a sintaxe falhar, simplifique o diagrama; não entregue código quebrado.

## Checklist de revisão

- [ ] O diagrama responde a uma pergunta concreta do TDD.
- [ ] Há um título/legenda e uma explicação curta.
- [ ] Os nomes batem com o texto, as tabelas e as decisões `DEC-XXX`.
- [ ] O diagrama está legível e não excessivamente grande.
- [ ] As relações, setas e cardinalidades estão corretas.
- [ ] O fluxo/estado tem início, fim e tratamento de erro quando aplicável.
- [ ] O código Mermaid renderiza sem erros.
- [ ] O diagrama não duplica desnecessariamente outra seção.
