# Comunicazione OPC UA nella Learning Factory 4.0

**Documento Tecnico per Tesi di Laurea**

---

## Indice

1. Introduzione]
2. Architettura OPC UA
3. Address Space del PLC
4. Moduli e Variabili
5. Utilizzo con UA Expert
6. Integrazione con Node-RED
7. Esempi Pratici
8. Riferimenti

---

## 1. Introduzione

### 1.1 Scopo del Documento

Questo documento descrive l'implementazione e l'utilizzo del protocollo **OPC UA** (Open Platform Communications Unified Architecture) nella **Learning Factory 4.0 24V** di fischertechnik. L'analisi si concentra sull'indirizzamento delle variabili, la struttura dell'Address Space e l'integrazione con sistemi di supervisione.

### 1.2 Contesto

La Learning Factory 4.0 utilizza OPC UA come protocollo di comunicazione principale tra:
- **PLC Siemens S7-1500** (SIMATIC CPU1512SP-1 PN)
- **Sistemi di supervisione** (Node-RED, dashboard web)
- **Client OPC UA** (UA Expert, applicazioni personalizzate)

### 1.3 Configurazione Server OPC UA

| Parametro | Valore |
|-----------|--------|
| **Endpoint** | `opc.tcp://192.168.0.1:4840` |
| **Namespace principale** | `3` |
| **Tipo autenticazione** | Anonymous / Username+Password |
| **Modalità sicurezza** | None / Sign / Sign&Encrypt |

---

## 2. Architettura OPC UA

### 2.1 Modello Client-Server

Il PLC Siemens agisce come **OPC UA Server**, esponendo le variabili del programma PLC attraverso un **Address Space** strutturato gerarchicamente. I client (UA Expert, Node-RED) si connettono al server per leggere e scrivere variabili in tempo reale.

```
┌─────────────────────────────────────────────────────────┐
│                    OPC UA Clients                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  UA Expert   │  │  Node-RED    │  │  Dashboard   │  │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘  │
│         │                  │                  │           │
│         └──────────────────┼──────────────────┘           │
│                            │                               │
└────────────────────────────┼───────────────────────────────┘
                             │
                    ┌────────▼────────┐
                    │  OPC UA Server  │
                    │  (PLC S7-1500)  │
                    │ 192.168.0.1:4840│
                    └─────────────────┘
```

### 2.2 Namespace

OPC UA organizza le variabili in **namespace** numerati. La Learning Factory utilizza:

- **Namespace 0**: Standard OPC UA (nodi di sistema)
- **Namespace 1**: Server information
- **Namespace 2**: Strutture di base Siemens
- **Namespace 3**: **Variabili applicative della Learning Factory** ⭐

---

## 3. Address Space del PLC

### 3.1 Struttura Gerarchica

L'Address Space del PLC è organizzato ad albero, con una struttura modulare che rispecchia l'architettura fisica della microfabbrica:

```
Objects (Root)
└── Namespace 3
    ├── gtyp_HBW (High-Bay Warehouse)
    │   ├── Horizontal_Axis
    │   └── Vertical_Axis
    │
    ├── gtyp_VGR (Vacuum Gripper Robot)
    │   ├── horizontal_Axis
    │   ├── vertical_Axis
    │   └── rotate_Axis
    │
    ├── gtyp_SSC (Sorting Station with Color detection)
    │   ├── Horizontal_Axis
    │   └── Vertical_Axis
    │
    ├── gtyp_Interface_Dashboard
    │   ├── Publish (comandi verso PLC)
    │   └── Subscribe (dati dal PLC)
    │
    └── gtyp_Interface_TXT_Controler
        ├── Publish
        └── Subscribe
```

### 3.2 Convenzioni di Naming

Le variabili seguono convenzioni di naming standard:

| Prefisso | Tipo | Descrizione | Esempio |
|----------|------|-------------|---------|
| `x_` | Bool | Variabili booleane | `x_active`, `x_Position_Reached` |
| `i_` | Int16 | Interi a 16 bit | `i_code`, `i_ldr` |
| `di_` | Int32 | Interi a 32 bit (DInt) | `di_Actual_Position` |
| `r_` | Real/Float | Numeri in virgola mobile | `r_t` (temperatura), `r_h` (umidità) |
| `s_` | String | Stringhe di testo | `s_description`, `s_station` |
| `ldt_` | DateTime | Data e ora | `ldt_ts` (timestamp) |

### 3.3 Formato NodeId

Ogni variabile OPC UA è identificata univocamente da un **NodeId** con formato:

```
ns=<namespace>;s="<path>.<path>.<variabile>"
```

**Esempio**:
```
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSI"."i_code"
```

Dove:
- `ns=3`: Namespace 3 (applicativo)
- `s=`: Stringa identificativa
- Path gerarchico separato da `"."`

---

## 4. Moduli e Variabili

### 4.1 gtyp_Interface_Dashboard

Il modulo **Dashboard Interface** gestisce lo scambio dati tra PLC e interfaccia utente web.

#### 4.1.1 Struttura Subscribe (Dati dal PLC)

##### State Machine dei Moduli

Ogni modulo della factory (HBW, VGR, SSC, MPO, SLD) espone il proprio stato attraverso variabili strutturate:

| Variabile       | Tipo     | Descrizione                      |
| --------------- | -------- | -------------------------------- |
| `i_code`        | Int16    | Codice stato numerico (0-100)    |
| `s_description` | String   | Descrizione testuale dello stato |
| `s_station`     | String   | Identificativo stazione          |
| `s_target`      | String   | Destinazione pezzo               |
| `x_active`      | Bool     | Indicatore attività modulo       |
| `ldt_ts`        | DateTime | Timestamp ultimo aggiornamento   |

**Esempio - Stato High-Bay Warehouse**:
```
gtyp_Interface_Dashboard.Subscribe.State_HBW
├── i_code            : Int16      (es: 10 = "Idle")
├── ldt_ts            : DateTime   (es: 2026-02-17T10:45:00Z)
├── s_description     : String     (es: "Waiting for order")
├── s_station         : String     (es: "HBW")
├── s_target          : String     (es: "VGR")
└── x_active          : Bool       (es: true)
```

**NodeId completi**:
- `ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."i_code"`
- `ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."ldt_ts"`
- `ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_description"`
- `ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_station"`
- `ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_target"`
- `ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."x_active"`

##### Sensori Ambientali

Il TXT Controller espone dati dai sensori:

**EnvironmentSensor**:

| Variabile | Tipo | Unità | Descrizione |
|-----------|------|-------|-------------|
| `r_t` | Float | °C | Temperatura |
| `r_rh` | Float | % | Umidità relativa |
| `r_p` | Float | hPa | Pressione atmosferica |
| `i_aq` | Int16 | - | Qualità aria (0-500) |
| `i_iaq` | Int16 | - | Indice qualità aria indoor |
| `di_gr` | Int32 | ohm | Resistenza sensore gas |
| `ldt_ts` | DateTime | - | Timestamp lettura |

**NodeId esempio**:
```
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_t"
```

**BrightnessSensor**:

| Variabile | Tipo | Descrizione |
|-----------|------|-------------|
| `i_ldr` | Int16 | Valore raw fotoresistenza (0-1023) |
| `r_br` | Float | Luminosità percentuale (0-100%) |
| `ldt_ts` | DateTime | Timestamp |

#### 4.1.2 Struttura Publish (Comandi al PLC)

##### Ordini Produzione

**OrderWorkpieceButton**:

| Variabile | Tipo | Descrizione |
|-----------|------|-------------|
| `s_type` | String | Tipo pezzo richiesto ("white", "red", "blue") |
| `ldt_ts` | DateTime | Timestamp richiesta |

**NodeId**:
```
ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."s_type"
```

##### Controllo Pan-Tilt Camera

**PosPanTiltUnit**:

| Variabile | Tipo | Descrizione |
|-----------|------|-------------|
| `s_cmd` | String | Comando ("up", "down", "left", "right", "reset") |
| `i_degree` | Int16 | Angolo movimento (gradi) |
| `ldt_ts` | DateTime | Timestamp comando |

### 4.2 gtyp_HBW (High-Bay Warehouse)

Il magazzino verticale espone posizioni assi per monitoring:

#### Horizontal_Axis

| Variabile | Tipo | Descrizione |
|-----------|------|-------------|
| `di_Actual_Position` | Int32 | Posizione attuale asse X (impulsi encoder) |
| `di_Target_Position` | Int32 | Posizione target asse X |
| `x_Position_Reached` | Boolean | Flag posizione raggiunta |

**NodeId esempio**:
```
ns=3;s="gtyp_HBW"."Horizontal_Axis"."di_Actual_Position"
```

#### Vertical_Axis

Stessa struttura di Horizontal_Axis per asse verticale Y.

### 4.3 gtyp_VGR (Vacuum Gripper Robot)

Robot a 3 assi (XYZ + rotazione):

#### horizontal_Axis, vertical_Axis, rotate_Axis

Ogni asse espone:
- `di_Actual_Position` (Int32)
- `di_Target_Position` (Int32)
- `x_Position_Reached` (Boolean)

**NodeId esempio**:
```
ns=3;s="gtyp_VGR"."rotate_Axis"."di_Actual_Position"
```

### 4.4 gtyp_SSC (Sorting Station with Color detection)

Stazione di sorting con 2 assi:

Stessa struttura di gtyp_HBW (Horizontal_Axis, Vertical_Axis).

### 4.5 gtyp_Interface_TXT_Controler

Interfaccia con TXT Controller per gestione NFC:

#### Publish.ActionButtonNFCModule

| Variabile | Tipo | Descrizione |
|-----------|------|-------------|
| `s_cmd` | String | Comando NFC ("read", "write", "clear") |
| `Workpiece.s_id` | String | ID pezzo NFC |
| `Workpiece.s_type` | String | Tipo pezzo |
| `Workpiece.s_state` | String | Stato lavorazione |
| `ldt_ts` | DateTime | Timestamp |

#### Subscribe.State_NFC_Device

Stessa struttura di Publish, contenente lo stato corrente del dispositivo NFC.

---

## 5. Utilizzo con UA Expert

### 5.1 Connessione al Server

**Procedura**:

1. Avvia **UA Expert** (Unified Automation)
2. Vai su **Server** → **Add** → **Advanced**
3. Inserisci endpoint: `opc.tcp://192.168.0.1:4840`
4. Modalità sicurezza: **None** (o Sign&Encrypt se configurato)
5. Autenticazione: **Anonymous** (o credenziali utente)
6. Clicca **OK** e poi **Connect**

### 5.2 Navigazione Address Space

1. Nel pannello **Address Space** espandi:
   - `Objects`
   - `Default Binary`
   - Namespace URI `SPLC`
   - Cartella con namespace `3`

2. Vedrai i moduli principali:
   ```
   └── [3] "SPLC"
       ├── gtyp_HBW
       ├── gtyp_Interface_Dashboard
       ├── gtyp_Interface_TXT_Controler
       ├── gtyp_MPO
       ├── gtyp_SLD
       ├── gtyp_SSC
       ├── gtyp_Setup
       └── gtyp_VGR
   ```

### 5.3 Monitoring Variabili

1. Espandi il modulo desiderato (es: `gtyp_Interface_Dashboard` → `Subscribe` → `State_HBW`)
2. **Drag & drop** la variabile (es: `i_code`) nel pannello **Data Access View**
3. Il valore si aggiorna in tempo reale

**Esempio visuale**:
```
┌─────────────────────────────────────────────────────────────┐
│ Data Access View                                            │
├─────────────────────┬────────────┬──────────────────────────┤
│ NodeId              │ Value      │ Timestamp                │
├─────────────────────┼────────────┼──────────────────────────┤
│ State_HBW.i_code    │ 10         │ 2026-02-17 10:45:23.456 │
│ State_HBW.s_station │ "HBW"      │ 2026-02-17 10:45:23.456 │
│ State_HBW.x_active  │ TRUE       │ 2026-02-17 10:45:23.456 │
└─────────────────────┴────────────┴──────────────────────────┘
```

### 5.4 Scrittura Variabili

1. Nel pannello **Data Access View**, doppio click sul valore
2. Inserisci nuovo valore
3. Premi **Enter** per scrivere

**Esempio - Ordinare un pezzo bianco**:
- Variabile: `gtyp_Interface_Dashboard.Publish.OrderWorkpieceButton.s_type`
- Nuovo valore: `"white"`

---

## 6. Integrazione con Node-RED

### 6.1 Nodi OPC UA

Node-RED utilizza il package **node-red-contrib-opcua** con i seguenti nodi:

- **OpcUa-Client**: Configurazione connessione server
- **OpcUa-Item**: Lettura variabili (subscribe)
- **OpcUa-Write**: Scrittura variabili

### 6.2 Configurazione Client

**Configurazione nodo OpcUa-Client**:
```json
{
  "endpoint": "opc.tcp://192.168.0.1:4840",
  "securityPolicy": "None",
  "securityMode": "None",
  "name": "PLC_Factory"
}
```

### 6.3 Lettura Variabili

**Esempio - Leggere stato HBW**:

**Function node (preparazione richiesta)**:
```javascript
msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    {
        "name": "",
        "nodeId": 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."i_code"',
        "datatypeName": 'Int16'
    },
    {
        "name": "",
        "nodeId": 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_description"',
        "datatypeName": 'String'
    },
    {
        "name": "",
        "nodeId": 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."x_active"',
        "datatypeName": 'Bool'
    }
];

return msg;
```

**Risposta OPC UA**:
```json
{
  "payload": {
    "value": [10, "Waiting for order", true],
    "sourceTimestamp": ["2026-02-17T10:45:23.456Z", "2026-02-17T10:45:23.456Z", "2026-02-17T10:45:23.456Z"]
  }
}
```

### 6.4 Scrittura Variabili

**Esempio - Ordinare pezzo rosso**:

**Function node**:
```javascript
msg.nodetype = "inject";
msg.injectType = "write";

var ts = new Date().toISOString();

msg.addressSpaceItems = [
    {
        "nodeId": 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."s_type"',
        "datatypeName": 'String'
    },
    {
        "nodeId": 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."ldt_ts"',
        "datatypeName": 'DateTime'
    }
];

msg.valuesToWrite = ["red", ts];

return msg;
```

### 6.5 Monitoring Continuo (Subscribe)

Per ricevere aggiornamenti automatici:

**Function node**:
```javascript
msg.nodetype = "inject";
msg.injectType = "subscribe";
msg.interval = 1000; // Polling ogni 1 secondo

msg.addressSpaceItems = [
    {
        "name": "temperature",
        "nodeId": 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_t"',
        "datatypeName": 'Float'
    }
];

return msg;
```

---

## 7. Esempi Pratici

### 7.1 Caso d'Uso: Monitoraggio Temperatura

**Obiettivo**: Visualizzare temperatura in tempo reale su dashboard.

**Soluzione Node-RED**:

```
[Inject 1s] → [Function: Read Temp] → [OpcUa-Item] → [Chart]
```

**Codice Function**:
```javascript
msg.addressSpaceItems = [{
    "nodeId": 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_t"',
    "datatypeName": 'Float'
}];
return msg;
```

### 7.2 Caso d'Uso: Ordinare Pezzo da Dashboard

**Obiettivo**: Button su dashboard web per ordinare pezzo bianco.

**Soluzione Node-RED**:

```
[Dashboard Button] → [Function: Order White] → [OpcUa-Write]
```

**Codice Function**:
```javascript
msg.addressSpaceItems = [{
    "nodeId": 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."s_type"',
    "datatypeName": 'String'
}];
msg.valuesToWrite = ["white"];
return msg;
```

### 7.3 Caso d'Uso: Alert su Anomalia

**Obiettivo**: Email quando `State_HBW.i_code` > 50 (errore).

**Soluzione Node-RED**:

```
[Subscribe State] → [Function: Check Error] → [Switch] → [Email]
                                                   ↓
                                              [Dashboard Alert]
```

**Codice Function**:
```javascript
var code = msg.payload.value[0]; // i_code
if (code > 50) {
    msg.payload = {
        subject: "Factory Alert: HBW Error",
        body: `HBW error code: ${code}`
    };
    return msg;
}
return null;
```

---

## 8. Riferimenti

### 8.1 Statistiche Address Space

**Totale variabili estratte**: 388

| Modulo                       | Variabili          |
| ---------------------------- | ------------------ |
| gtyp_Interface_Dashboard     | 96                 |
| gtyp_Interface_TXT_Controler | 45                 |
| gtyp_HBW                     | 6                  |
| gtyp_VGR                     | 9                  |
| gtyp_SSC                     | 6                  |
| **Totale**                   | **162** (univoche) |

### 8.2 Distribuzione Tipi di Dati

| Tipo | Occorrenze | % |
|------|------------|---|
| Int32 | 175 | 45.1% |
| String | 86 | 22.2% |
| Int16 | 50 | 12.9% |
| DateTime | 37 | 9.5% |
| Bool/Boolean | 17 | 4.4% |
| Float | 9 | 2.3% |
| Altri | 14 | 3.6% |

### 8.3 Tool e Software

- **UA Expert**: [https://www.unified-automation.com](https://www.unified-automation.com)
- **Node-RED**: [https://nodered.org](https://nodered.org)
- **node-red-contrib-opcua**: [https://flows.nodered.org/node/node-red-contrib-opcua-iiot](https://flows.nodered.org/node/node-red-contrib-opcua-iiot)
- **TIA Portal**: Siemens STEP 7 per programmazione PLC

### 8.4 Standard OPC UA

- **Specifica IEC 62541**: OPC Unified Architecture
- **OPC Foundation**: [https://opcfoundation.org](https://opcfoundation.org)

### 8.5 Documenti fischertechnik

- **Learning Factory 4.0 24V - Commissioning Guide**
- **Learning Factory 4.0 24V - Factory Components**
- **SIMATIC CPU1512SP-1 PN Manual**

---

## Appendice A: NodeId Completi per Modulo

### A.1 gtyp_Interface_Dashboard.Subscribe.State_HBW

```
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."i_code"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."ldt_ts"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_description"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_station"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_target"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."x_active"
```

### A.2 gtyp_Interface_Dashboard.Subscribe.EnvironmentSensor

```
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_t"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_rh"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_p"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."i_aq"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."i_iaq"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."di_gr"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."ldt_ts"
```

### A.3 gtyp_HBW Assi

**Horizontal_Axis**:
```
ns=3;s="gtyp_HBW"."Horizontal_Axis"."di_Actual_Position"
ns=3;s="gtyp_HBW"."Horizontal_Axis"."di_Target_Position"
ns=3;s="gtyp_HBW"."Horizontal_Axis"."x_Position_Reached"
```

**Vertical_Axis**:
```
ns=3;s="gtyp_HBW"."Vertical_Axis"."di_Actual_Position"
ns=3;s="gtyp_HBW"."Vertical_Axis"."di_Target_Position"
ns=3;s="gtyp_HBW"."Vertical_Axis"."x_Position_Reached"
```

### A.4 gtyp_VGR Assi

```
ns=3;s="gtyp_VGR"."horizontal_Axis"."di_Actual_Position"
ns=3;s="gtyp_VGR"."horizontal_Axis"."di_Target_Position"
ns=3;s="gtyp_VGR"."horizontal_Axis"."x_Position_Reached"

ns=3;s="gtyp_VGR"."vertical_Axis"."di_Actual_Position"
ns=3;s="gtyp_VGR"."vertical_Axis"."di_Target_Position"
ns=3;s="gtyp_VGR"."vertical_Axis"."x_Position_Reached"

ns=3;s="gtyp_VGR"."rotate_Axis"."di_Actual_Position"
ns=3;s="gtyp_VGR"."rotate_Axis"."di_Target_Position"
ns=3;s="gtyp_VGR"."rotate_Axis"."x_Position_Reached"
```

---

## Appendice B: Mappatura Tipi di Dati

### B.1 Tipi OPC UA ↔ PLC Siemens

| OPC UA | Siemens S7 | Dimensione | Range |
|--------|------------|------------|-------|
| Boolean | BOOL | 1 bit | true/false |
| Int16 | INT | 16 bit | -32768 ... 32767 |
| UInt16 | WORD/UINT | 16 bit | 0 ... 65535 |
| Int32 | DINT | 32 bit | -2147483648 ... 2147483647 |
| Float | REAL | 32 bit | ±1.18×10⁻³⁸ ... ±3.40×10³⁸ |
| String | STRING | variabile | Testo UTF-8 |
| DateTime | DATE_AND_TIME | 64 bit | Data/ora ISO 8601 |

### B.2 Formato DateTime

OPC UA utilizza **UTC timestamp** in formato:
```
2026-02-17T10:45:23.456Z
```

Il PLC Siemens utilizza **DATE_AND_TIME** con precisione al millisecondo.

---

**Fine Documento**

*Versione 1.0 - Febbraio 2026*  
*Learning Factory 4.0 24V - fischertechnik*
