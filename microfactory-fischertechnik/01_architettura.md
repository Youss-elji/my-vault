# 01 – Architettura Generale della Microfactory 4.0

Questa sezione presenta la **visione complessiva** della Learning Factory 4.0: l'organizzazione del sistema, l'interazione tra le componenti e il flusso di controllo e informazione.

---

# 1. Vista Generale della Fabbrica

![Schema generale](microfactory_overview.drawio.png)
---

# 2. Architettura Logica Completa
Questa sezione descrive i collegamenti tra i moduli: connessioni fisiche, logiche e informatiche del sistema.

```mermaid
flowchart TB
    classDef hw fill:#e8f5e9,stroke:#2e7d32,stroke-width:1px,color:#1b5e20;
    classDef ctrl fill:#e3f2fd,stroke:#1565c0,stroke-width:1px,color:#0d47a1;
    classDef cloud fill:#f3e5f5,stroke:#6a1b9a,stroke-width:1px,color:#4a148c;
    classDef net fill:#fff3e0,stroke:#ef6c00,stroke-width:1px,color:#e65100;

    subgraph Hardware
        HBW[[HBW – Magazzino Verticale]]:::hw
        VGR[[VGR – Vacuum Gripper Robot]]:::hw
        MPO[[MPO – Stazione di Lavorazione]]:::hw
        SLD[[SLD – Sorting Line]]:::hw
        DPS[[DPS – NFC Station]]:::hw
        SSC[[SSC – Camera + Sensori]]:::hw
    end

    subgraph Controllo Locale
        PLC[[PLC Siemens S7-1500]]:::ctrl
        GATEWAY[[IoT Gateway – Node-RED]]:::ctrl
        TXT[[TXT Controller 4.0]]:::ctrl
    end

    CLOUD[(Cloud Fischertechnik)]:::cloud
    ROUTER{{TP-Link WR902AC}}:::net

    HBW --> VGR --> MPO --> SLD --> DPS
    SSC --> TXT

    PLC --> HBW
    PLC --> VGR
    PLC --> MPO
    PLC --> SLD
    PLC --> DPS

    PLC -- OPC-UA --> GATEWAY
    GATEWAY -- MQTT --> TXT
    TXT -- MQTT --> CLOUD

    ROUTER -. LAN/WiFi .-> PLC
    ROUTER -. LAN/WiFi .-> GATEWAY
    ROUTER -. WiFi .-> TXT
    ROUTER -. Internet .-> CLOUD
```

---

# 3. Chi Controlla Cosa (Tabella OT/IT)
Questa sezione definisce il ruolo dei diversi livelli del sistema.

| Livello | Componente | Responsabilità |
|---------|------------|----------------|
| **OT (Operativo)** | PLC Siemens S7-1500 | Controllo reale della produzione, gestione sensori/attuatori, sicurezza |
| **Edge** | IoT Gateway (Node-RED) | Calibrazioni, dashboard, comunicazione OPC-UA→MQTT, gestione file |
| **IoT Device** | TXT Controller 4.0 | Lettura NFC, telecamera, sensori, sincronizzazione con cloud |
| **Cloud** | Fischer Cloud | Gestione ordini, dashboard remota |
| **Networking** | TP-Link WR902AC | Gestione rete interna, DHCP, connessione Internet |
| **Moduli fisici** | HBW, VGR, MPO, SLD, DPS, SSC | Parte meccatronica che movimenta e processa il pezzo |

---

# 4. Flusso di Comunicazione OT → IT → Cloud
Qui mostro in che modo i dati e i comandi si spostano tra PLC, Gateway, TXT e Cloud. È utile per capire come avviene la sincronizzazione.

```mermaid
sequenceDiagram
    autonumber
    participant PLC as PLC (OPC-UA)
    participant GW as IoT Gateway (Node-RED)
    participant TXT as TXT Controller 4.0
    participant CLOUD as Cloud

    PLC->>GW: Richiesta configurazioni (OPC-UA)
    GW-->>PLC: Invio parametri e posizioni

    CLOUD->>TXT: Nuovo ordine (MQTT)
    TXT->>GW: Sincronizzazione ordine
    GW->>PLC: Aggiornamento stato ordine

    PLC->>GW: Stato avanzamento
    GW->>TXT: Notifica aggiornamento
    TXT->>CLOUD: Aggiorna dashboard
```

---

# 5. Schema della Rete OT/IT
Rappresentazione semplice di come sono collegati tra loro router, PLC, Gateway, TXT e Cloud.

```mermaid
flowchart LR
    classDef net fill:#fffde7,stroke:#f9a825,stroke-width:1px,color:#f57f17;
    classDef dev fill:#e3f2fd,stroke:#1565c0,stroke-width:1px,color:#0d47a1;

    ROUTER{{TP-Link WR902AC}}:::net
    PLC([PLC S7-1500]):::dev
    GATEWAY([IoT Gateway – RPi]):::dev
    TXT([TXT Controller 4.0]):::dev
    CLOUD[(Cloud)]:::net

    ROUTER -- LAN/WiFi --> PLC
    ROUTER -- LAN/WiFi --> GATEWAY
    ROUTER -- WiFi --> TXT
    ROUTER -- Internet --> CLOUD
```

---

# 6. Architettura Cyber-Fisica (CPS)
La fabbrica si basa sul concetto di **sistema cyber-fisico**: una parte fisica fatta di moduli reali e una parte digitale fatta di PLC, Gateway, TXT e Cloud. È proprio la comunicazione continua tra queste due parti che rende la microfactory un esempio realistico di Industry 4.0.

---

# 7. Scopo della Sezione
Questa sezione fornisce una panoramica dell'intero ecosistema, facilitando la comprensione dei singoli moduli.

