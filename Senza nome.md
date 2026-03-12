## 1. Obiettivo

L'obiettivo del lavoro è stato sostituire l'uso della dashboard ufficiale della microfactory per l'invio degli ordini, utilizzando esclusivamente MQTT, senza modificare la logica del PLC e senza causare instabilità o crash del sistema.

L'ordine viene:

- inviato via MQTT,
- intercettato da Node-RED,
- tradotto in una scrittura OPC UA verso il PLC,
- eseguito dal PLC,
- e infine lo stato viene restituito via MQTT.

## 2. Architettura generale

### 2.1 Flusso logico

mermaid

```mermaid
graph TB
    Client[Client MQTT]
    NodeRED[Node-RED<br/>prepare HBW order]
    OPC_W[OPC UA Write<br/>gtyp_Interface_Dashboard]
    PLC[PLC Siemens]
    OPC_R[OPC UA Read<br/>stato ordine]
    MQTT_Out[MQTT Publish<br/>f/o/order]
    
    Client -->|f/c/order<br/>payload: type| NodeRED
    NodeRED -->|Valida payload| OPC_W
    OPC_W -->|Scrive comando<br/>OrderWorkpieceButton| PLC
    PLC -->|Elabora ordine<br/>cambia stato| PLC
    PLC -->|Stato aggiornato| OPC_R
    OPC_R -->|Legge passivamente| MQTT_Out
    MQTT_Out -->|Pubblica stato| Client
    
    style Client fill:#4A90E2,stroke:#2E5EAA,stroke-width:2px,color:#fff
    style NodeRED fill:#FF6B35,stroke:#C74B2A,stroke-width:2px,color:#fff
    style OPC_W fill:#26A69A,stroke:#00796B,stroke-width:2px,color:#fff
    style PLC fill:#FFA726,stroke:#F57C00,stroke-width:3px,color:#fff
    style OPC_R fill:#26A69A,stroke:#00796B,stroke-width:2px,color:#fff
    style MQTT_Out fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
```

### 2.2 Principio fondamentale

⚠️ **Il PLC è l'unico responsabile dello stato dell'ordine**

Node-RED non forza mai lo stato, ma:

- scrive solo il comando di ordine,
- legge passivamente lo stato,
- lo ripubblica via MQTT.

Questo punto è stato essenziale per evitare crash della microfactory.

### 2.3 Ciclo di vita dell'ordine

mermaid

```mermaid
stateDiagram-v2
    [*] --> WAITING_FOR_ORDER: Sistema pronto
    WAITING_FOR_ORDER --> IN_PROCESS: Ordine ricevuto<br/>(f/c/order)
    IN_PROCESS --> IN_PROCESS: Lavorazione<br/>in corso
    IN_PROCESS --> SHIPPED: Ordine<br/>completato
    SHIPPED --> WAITING_FOR_ORDER: Reset stato
    WAITING_FOR_ORDER --> WAITING_FOR_ORDER: Nessun ordine
    
    note right of WAITING_FOR_ORDER
        Stato iniziale
        type = ""
    end note
    
    note right of IN_PROCESS
        Produzione attiva
        type = BLUE/RED/WHITE
    end note
    
    note right of SHIPPED
        Ordine evaso
        Pronto per reset
    end note
```

## 3. Topic MQTT utilizzati


