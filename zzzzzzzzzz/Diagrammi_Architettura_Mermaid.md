# Diagrammi Architettura Learning Factory 4.0

## 1. Architettura Completa Sistema

```mermaid
graph TB
    subgraph "LIVELLO 4 - Presentazione"
        UI[Dashboard Web<br/>http://192.168.0.1:1880/ui]
        EXT[Client Esterni<br/>Mobile App, SCADA]
    end

    subgraph "LIVELLO 3 - Middleware"
        NR[Node-RED<br/>Logiche & Orchestrazione]
        MQTT[MQTT Broker<br/>Mosquitto<br/>Port 1883]
        DB[(Database<br/>InfluxDB<br/>Time Series)]
    end

    subgraph "LIVELLO 2 - Controllo"
        PLC[PLC Siemens S7-1500<br/>OPC UA Server<br/>192.168.0.1:4840]
    end

    subgraph "LIVELLO 1 - Campo"
        HBW[High-Bay<br/>Warehouse]
        VGR[Vacuum<br/>Gripper Robot]
        SSC[Sorting<br/>Station]
        TXT[TXT Controller<br/>Sensori + NFC]
    end

    UI -->|HTTP/WebSocket| NR
    EXT -->|MQTT Subscribe| MQTT

    NR -->|OPC UA Read/Write| PLC
    NR -->|Publish Events| MQTT
    NR -->|Store Data| DB

    MQTT -.->|Subscribe States| NR
    DB -.->|Query History| NR

    PLC -->|I/O Fieldbus| HBW
    PLC -->|I/O Fieldbus| VGR
    PLC -->|I/O Fieldbus| SSC
    PLC -->|Ethernet| TXT

    style PLC fill:#0066cc,stroke:#003d7a,color:#fff
    style NR fill:#8b0000,stroke:#5a0000,color:#fff
    style MQTT fill:#660066,stroke:#400040,color:#fff
    style UI fill:#006600,stroke:#003d00,color:#fff
```

---

## 2. Flusso Dati Publish/Subscribe

```mermaid
sequenceDiagram
    participant UI as Dashboard Web
    participant NR as Node-RED
    participant OPC as OPC UA Server<br/>(PLC)
    participant MQTT as MQTT Broker
    participant DB as InfluxDB

    Note over UI,DB: SCENARIO: Utente ordina pezzo rosso

    UI->>NR: Click "Order Red"<br/>(HTTP POST)

    NR->>NR: Prepare OPC UA Write
    Note right of NR: msg.addressSpaceItems = [<br/>  {nodeId: "...OrderButton.s_type",<br/>   datatypeName: "String"}]<br/>msg.valuesToWrite = ["red"]

    NR->>OPC: OPC UA Write<br/>"red" → Publish.OrderButton

    OPC->>OPC: PLC Logic Execution
    Note right of OPC: Valida ordine<br/>Aggiorna state machine<br/>Avvia produzione

    OPC-->>NR: OPC UA Subscribe<br/>State_Order.s_state = "in_production"

    NR->>MQTT: Publish Event<br/>Topic: Factory/Orders/New<br/>Payload: {type:"red", state:"in_production"}

    NR->>DB: Store Event<br/>Measurement: orders<br/>Tags: type=red<br/>Value: 1

    NR->>UI: Update Dashboard<br/>(WebSocket push)

    UI->>UI: Display: "Order in production ●"

    Note over UI,DB: Ogni 1s: Node-RED legge stati via OPC UA

    loop Monitoring Loop (1s interval)
        NR->>OPC: OPC UA Read<br/>State_HBW, State_VGR, State_SSC
        OPC-->>NR: Return States
        NR->>UI: Update Real-Time
        NR->>MQTT: Publish States
    end
```

---

## 3. Logica Publish vs Subscribe

```mermaid
graph LR
    subgraph "CLIENT SIDE - Node-RED"
        CMD[Comandi Utente]
        MONITOR[Monitoring & UI]
    end

    subgraph "PLC - Namespace 3"
        PUB[".Publish"<br/>📤 WRITE Access<br/>Comandi → PLC]
        SUB[".Subscribe"<br/>📥 READ Access<br/>Stati ← PLC]
        LOGIC[PLC Logic<br/>State Machines<br/>Safety Checks]
    end

    subgraph "MQTT"
        TOPIC_CMD[Topic:<br/>Factory/Commands/#]
        TOPIC_STATE[Topic:<br/>Factory/States/#]
    end

    CMD -->|OPC UA Write| PUB
    PUB -->|Input| LOGIC
    LOGIC -->|Output| SUB
    SUB -->|OPC UA Read/Subscribe| MONITOR

    CMD -->|Publish| TOPIC_CMD
    MONITOR -->|Publish| TOPIC_STATE

    TOPIC_STATE -.->|Subscribe| MONITOR

    style PUB fill:#ff6b6b,stroke:#c92a2a,color:#fff
    style SUB fill:#51cf66,stroke:#2f9e44,color:#fff
    style LOGIC fill:#0066cc,stroke:#003d7a,color:#fff
```

---

## 4. Pattern Node-RED: Comando → Feedback

```mermaid
graph TB
    subgraph "Flow Node-RED"
        START[Dashboard Button<br/>'Move Camera Left']

        FUNC1[Function: Build Command<br/>---<br/>msg.addressSpaceItems = [nodeId]<br/>msg.valuesToWrite = ['relmove_left', 15]]

        WRITE[OPC-UA Write Node<br/>---<br/>Endpoint: 192.168.0.1:4840<br/>NodeId: Publish.PosPanTiltUnit]

        WAIT[Delay 200ms<br/>Tempo esecuzione comando]

        FUNC2[Function: Prepare Read<br/>---<br/>msg.addressSpaceItems = [<br/>  Subscribe.PosPanTiltUnit.r_pan]]

        READ[OPC-UA Read Node<br/>---<br/>Leggi feedback posizione]

        PARSE[Function: Parse Response<br/>---<br/>pan_angle = msg.payload.value[0]]

        UI_UPDATE[Dashboard Gauge<br/>Mostra angolo corrente]

        MQTT_PUB[MQTT Out<br/>---<br/>Topic: Factory/Camera/Position<br/>Payload: {pan: 45, tilt: 30}]

        DB_WRITE[InfluxDB Out<br/>---<br/>Measurement: camera_position<br/>Field: pan_angle]
    end

    START --> FUNC1
    FUNC1 --> WRITE
    WRITE --> WAIT
    WAIT --> FUNC2
    FUNC2 --> READ
    READ --> PARSE
    PARSE --> UI_UPDATE
    PARSE --> MQTT_PUB
    PARSE --> DB_WRITE

    style WRITE fill:#8b0000,stroke:#5a0000,color:#fff
    style READ fill:#8b0000,stroke:#5a0000,color:#fff
    style MQTT_PUB fill:#660066,stroke:#400040,color:#fff
    style UI_UPDATE fill:#006600,stroke:#003d00,color:#fff
```

---

## 5. Bridge OPC UA → MQTT

```mermaid
flowchart LR
    subgraph PLC["PLC S7-1500"]
        STATE_HBW["Subscribe.State_HBW<br/>---<br/>i_code: 20<br/>s_description: 'Moving'<br/>x_active: true"]
        STATE_VGR["Subscribe.State_VGR<br/>---<br/>i_code: 10<br/>s_description: 'Idle'"]
        SENSORS["Subscribe.EnvironmentSensor<br/>---<br/>r_t: 22.5<br/>r_rh: 45.2"]
    end

    subgraph NR["Node-RED Bridge"]
        SUB1[OPC UA Subscribe<br/>Interval: 1s]
        TRANS1[Transform JSON<br/>---<br/>Formatta payload]
        SUB2[OPC UA Subscribe<br/>Interval: 1s]
        TRANS2[Transform JSON]
        SUB3[OPC UA Subscribe<br/>Interval: 5s]
        TRANS3[Transform JSON]
    end

    subgraph MQTT["MQTT Broker"]
        T1[Topic:<br/>Factory/HBW/Status<br/>---<br/>QoS: 1, Retain: true]
        T2[Topic:<br/>Factory/VGR/Status<br/>---<br/>QoS: 1, Retain: true]
        T3[Topic:<br/>Factory/Environment<br/>---<br/>QoS: 0, Retain: true]
    end

    subgraph CLIENTS["MQTT Clients"]
        GRAFANA[Grafana<br/>Dashboard]
        MOBILE[Mobile App]
        HISTORIAN[Historian<br/>Database]
    end

    STATE_HBW -->|OPC UA| SUB1
    SUB1 --> TRANS1
    TRANS1 -->|Publish| T1

    STATE_VGR -->|OPC UA| SUB2
    SUB2 --> TRANS2
    TRANS2 -->|Publish| T2

    SENSORS -->|OPC UA| SUB3
    SUB3 --> TRANS3
    TRANS3 -->|Publish| T3

    T1 -.->|Subscribe| GRAFANA
    T1 -.->|Subscribe| MOBILE
    T2 -.->|Subscribe| HISTORIAN
    T3 -.->|Subscribe| GRAFANA

    style PLC fill:#0066cc,stroke:#003d7a,color:#fff
    style NR fill:#8b0000,stroke:#5a0000,color:#fff
    style MQTT fill:#660066,stroke:#400040,color:#fff
```

---

## 6. Architettura a Livelli (Industry 4.0)

```mermaid
graph TB
    subgraph L5["LIVELLO 5 - ERP/MES"]
        ERP[Enterprise Resource Planning<br/>Gestione Ordini, Produzione]
    end

    subgraph L4["LIVELLO 4 - SCADA/HMI"]
        DASH[Dashboard Web<br/>Supervisione & Controllo]
        NODE[Node-RED<br/>Logiche Business]
    end

    subgraph L3["LIVELLO 3 - Controllo"]
        direction LR
        PLC_MAIN[PLC S7-1500<br/>State Machines<br/>Safety Logic]

        subgraph OPC["OPC UA Interface"]
            PUB_INT["📤 Publish<br/>(Comandi)"]
            SUB_INT["📥 Subscribe<br/>(Stati)"]
        end
    end

    subgraph L2["LIVELLO 2 - I/O"]
        IO[Digital I/O<br/>%Q0.0 - %Q7.7<br/>%I0.0 - %I7.7]
        ANALOG[Analog I/O<br/>%IW0 - %QW255]
        PROFINET[PROFINET<br/>TXT Controller]
    end

    subgraph L1["LIVELLO 1 - Campo"]
        MOTORS[Motori<br/>Encoders]
        VALVES[Valvole<br/>Pneumatiche]
        SENS[Sensori<br/>Digitali/Analogici]
        TXT_CTRL[TXT Controller<br/>NFC + Sensori]
    end

    ERP <-->|REST API| DASH
    DASH <-->|WebSocket| NODE
    NODE <-->|OPC UA| OPC

    OPC <--> PLC_MAIN
    PUB_INT -.->|Write| PLC_MAIN
    PLC_MAIN -.->|Update| SUB_INT

    PLC_MAIN <-->|Field Bus| IO
    PLC_MAIN <-->|Field Bus| ANALOG
    PLC_MAIN <-->|Ethernet| PROFINET

    IO --> MOTORS
    IO --> VALVES
    ANALOG --> SENS
    PROFINET --> TXT_CTRL

    MOTORS -.->|Feedback| IO
    VALVES -.->|Status| IO
    SENS -.->|Measure| ANALOG
    TXT_CTRL -.->|Data| PROFINET

    style L5 fill:#e3f2fd
    style L4 fill:#fff3e0
    style L3 fill:#e8f5e9
    style L2 fill:#fce4ec
    style L1 fill:#f3e5f5

    style PLC_MAIN fill:#0066cc,stroke:#003d7a,color:#fff
    style NODE fill:#8b0000,stroke:#5a0000,color:#fff
```

---

## 7. Sequence Diagram: Ciclo Completo Produzione

```mermaid
sequenceDiagram

    actor User as Operatore

    participant UI as Dashboard

    participant NR as Node-RED

    participant PLC as PLC S7-1500

    participant HBW as High-Bay Warehouse

    participant VGR as Vacuum Gripper

    participant SSC as Sorting Station

  

    User->>UI: Click "Order Red Piece"

    UI->>NR: HTTP POST /order {type:"red"}

  

    NR->>PLC: OPC UA Write<br/>Publish.OrderButton.s_type = "red"

  

    activate PLC

    Note over PLC: Valida ordine<br/>Aggiorna state machine

    PLC->>HBW: Comando: Move to position A2 (red)

    activate HBW

  

    PLC-->>NR: Subscribe.State_HBW.s_description<br/>"Moving to A2"

    NR-->>UI: Update: HBW Status ●

  

    HBW-->>PLC: Position reached

    deactivate HBW

  

    PLC->>VGR: Comando: Pickup from HBW

    activate VGR

  

    PLC-->>NR: Subscribe.State_VGR.s_description<br/>"Moving to pickup"

    NR-->>UI: Update: VGR Status ●

  

    VGR->>VGR: Activate vacuum<br/>Check pressure

  

    alt Vacuum OK

        VGR-->>PLC: Piece picked

        PLC->>VGR: Move to SSC

        PLC-->>NR: State: "Delivering to SSC"

        NR-->>UI: Update Status

    else Vacuum Fail

        VGR-->>PLC: Error: No vacuum

        PLC-->>NR: State_VGR.i_code = 90<br/>"Error: Vacuum failure"

        NR-->>UI: ⚠️ Alert: VGR Error

    end

  

    deactivate VGR

  

    VGR->>SSC: Place piece on belt

    activate SSC

  

    PLC->>SSC: Comando: Process piece

  

    SSC->>SSC: Detect color<br/>Sort piece

  

    SSC-->>PLC: Piece sorted

    deactivate SSC

  

    PLC-->>NR: Subscribe.State_Order.s_state<br/>"completed"

    deactivate PLC

  

    NR->>NR: Log to InfluxDB<br/>Publish MQTT event

  

    NR-->>UI: ✅ Order Completed

    UI-->>User: Show "Piece delivered"
```

---

## 8. Node-RED Flow Visuale (Esempio Monitoring)

```mermaid
flowchart TD
    subgraph TAB1["Tab: Factory Monitoring"]
        direction TB

        INJ1[Inject: Every 1s]

        subgraph GROUP1["Group: Read All States"]
            FUNC_READ[Function:<br/>Prepare Read States<br/>---<br/>Build addressSpaceItems<br/>for all modules]
            OPC_READ[OPC UA Item<br/>---<br/>Client: PLC_Factory<br/>Mode: Read]
            FUNC_PARSE[Function:<br/>Parse States<br/>---<br/>Extract values from array]
        end

        subgraph GROUP2["Group: Process & Display"]
            SWITCH[Switch:<br/>Route by Module]

            FUNC_HBW[Function: HBW<br/>Format state]
            GAUGE_HBW[Dashboard Gauge<br/>HBW Status]

            FUNC_VGR[Function: VGR<br/>Format state]
            TEXT_VGR[Dashboard Text<br/>VGR Description]

            FUNC_ENV[Function: Environment<br/>Format sensors]
            CHART_ENV[Dashboard Chart<br/>Temperature History]
        end

        subgraph GROUP3["Group: MQTT Bridge"]
            FUNC_MQTT[Function:<br/>Format MQTT Payload<br/>---<br/>msg.topic = Factory/States<br/>msg.payload = JSON]
            MQTT_OUT[MQTT Out<br/>---<br/>Broker: localhost:1883<br/>QoS: 1]
        end

        subgraph GROUP4["Group: Database"]
            FUNC_DB[Function:<br/>Format InfluxDB<br/>---<br/>Measurement: factory_states]
            INFLUX[InfluxDB Out<br/>---<br/>Database: factory_db]
        end
    end

    INJ1 --> FUNC_READ
    FUNC_READ --> OPC_READ
    OPC_READ --> FUNC_PARSE
    FUNC_PARSE --> SWITCH

    SWITCH -->|HBW| FUNC_HBW
    SWITCH -->|VGR| FUNC_VGR
    SWITCH -->|ENV| FUNC_ENV

    FUNC_HBW --> GAUGE_HBW
    FUNC_VGR --> TEXT_VGR
    FUNC_ENV --> CHART_ENV

    FUNC_PARSE --> FUNC_MQTT
    FUNC_MQTT --> MQTT_OUT

    FUNC_PARSE --> FUNC_DB
    FUNC_DB --> INFLUX

    style FUNC_READ fill:#fff3cd
    style OPC_READ fill:#8b0000,stroke:#5a0000,color:#fff
    style MQTT_OUT fill:#660066,stroke:#400040,color:#fff
    style GAUGE_HBW fill:#006600,stroke:#003d00,color:#fff
```

---

## Come Usare i Diagrammi

### In Markdown/GitHub
Copia-incolla direttamente - il rendering Mermaid è automatico

### In Documento Word/PDF
1. Usa editor online: https://mermaid.live
2. Incolla codice Mermaid
3. Esporta come PNG/SVG
4. Inserisci immagine nel documento

### In Presentazione PowerPoint
1. Usa https://mermaid.ink
2. Converti in immagine high-res
3. Drag & drop nella slide

---
