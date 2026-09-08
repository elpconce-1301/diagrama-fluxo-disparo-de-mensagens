# Arquitetura v1 — Diagrama C4 (Containers) em Mermaid

```mermaid
flowchart LR
  %% Sistema externo / atores
  CAM["Time de Campanhas"]
  CUST["Cliente"]
  DA["Digital Assistant Engine <<external>>"]

  subgraph OutboundNotification[Outbound Notification Service]
    direction TB
    MI["Mailing Ingestion Service"]
    MDB[("Mailing DB")]
    OBJ[("Object Storage <<external>>")]

    MP["Mailing Processing Service"]
    PLQ[("ProcessLine (Queue)")]
    SMQ[("SendMessage (Queue)")]
    SCHED["Scheduling Service"]

    DISP["Message Dispatch Service"]
    ORCH["Message Orchestration Service"]
    CFG[("Configuration Service")]

    MPQ[("MessageProducer (Queue)")]
    PROD["Message Producer"]

    CHORCH["Channel Orchestrator"]
    CHPA[("Canal A Plugin <<external>>")]
    CHPB[("Canal B Plugin <<external>>")]

    MDBG[("Message Status DB")]
  end

  %% Fluxos principais
  CAM -->|"Upload: arquivo de mailing"| MI
  MI -->|"Persiste metadados"| MDB
  MI -->|"Salva arquivo e gera URL"| OBJ

  MP -->|"Baixa arquivo"| OBJ
  MP -->|"Enfileira itens imediatos"| SMQ
  MP -->|"Enfileira itens agendados"| PLQ

  SCHED -->|"Consome itens agendados"| PLQ
  SCHED -->|"Move itens para envio"| SMQ

  DISP -->|"Consome itens a enviar"| SMQ
  DISP -->|"Invoca orquestração e registra status"| ORCH

  ORCH -->|"Consulta template/canal/endpoint"| CFG
  ORCH -->|"Publica payload pronto"| MPQ
  ORCH -->|"Registra status/histórico"| MDBG

  PROD -->|"Consome payload"| MPQ
  PROD -->|"Requisição HTTP"| CHORCH

  CHORCH -->|"Aciona plugin"| CHPA
  CHORCH -->|"Aciona plugin"| CHPB
  CHPA -->|"Entrega mensagem"| CUST
  CHPB -->|"Entrega mensagem"| CUST

  CUST -->|"Interação bidirecional"| DA
  CHORCH -->|"Roteia respostas / callbacks"| DA

  %% Legenda simples
  classDef external fill:#f8f8f8,stroke:#333,stroke-dasharray: 5 2
  class OBJ,CHPA,CHPB,DA external
```

Observações:
- Componentes externos são marcados como `<<external>>`.
- O diagrama segue o nível C4 — Containers (não mostra classes, endpoints ou payloads).
- Ajuste rótulos e posicionamento conforme preferir antes de revisar no GitHub.
