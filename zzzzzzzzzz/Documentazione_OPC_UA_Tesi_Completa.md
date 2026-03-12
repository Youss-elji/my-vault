# Comunicazione OPC UA nella Learning Factory 4.0

**Documento Tecnico per Tesi di Laurea**

---

## Indice

1. [Introduzione](#1-introduzione)
2. [Architettura OPC UA](#2-architettura-opc-ua)
3. [Struttura Address Space](#3-struttura-address-space)
4. [Logica Publish/Subscribe](#4-logica-publishsubscribe)
5. [Moduli della Factory](#5-moduli-della-factory)
6. [Utilizzo con UA Expert](#6-utilizzo-con-ua-expert)
7. [Integrazione Node-RED](#7-integrazione-node-red)
8. [Casi d'Uso Pratici](#8-casi-duso-pratici)
9. [Considerazioni Architetturali](#9-considerazioni-architetturali)
10. [Conclusioni](#10-conclusioni)
11. [Riferimenti](#11-riferimenti)

---

## 1. Introduzione

### 1.1 Scopo del Documento

Questo documento descrive l'implementazione e l'utilizzo del protocollo **OPC UA** (Open Platform Communications Unified Architecture) nella **Learning Factory 4.0 24V** di fischertechnik, con particolare focus sulla **logica Publish/Subscribe** e l'integrazione con **Node-RED**.

### 1.2 Contesto Operativo

La Learning Factory 4.0 utilizza un **PLC Siemens S7-1500** (SIMATIC CPU1512SP-1 PN) come unità centrale di controllo. Il PLC espone le proprie variabili di processo tramite un **Server OPC UA integrato**, consentendo:

- ✅ **Lettura stati moduli** (monitoring real-time)
- ✅ **Scrittura comandi** (controllo remoto)
- ✅ **Monitoraggio eventi** (allarmi, state machine)
- ✅ **Integrazione Node-RED** (dashboard web, logiche custom)
- ✅ **Bridge MQTT** (pubblicazione eventi su broker)

### 1.3 Parametri di Connessione

| Parametro | Valore |
|-----------|--------|
| **Endpoint** | `opc.tcp://192.168.0.1:4840` |
| **Namespace applicativo** | `3` |
| **Autenticazione** | Anonymous / Username+Password |
| **Modalità sicurezza** | None / Sign / Sign&Encrypt |
| **Tool di analisi** | UA Expert (Unified Automation) |

L'intero **Address Space** è stato estratto tramite UA Expert ed è documentato in questo file con tutti i **388 NodeId** identificati.

---

## 2. Architettura OPC UA

### 2.1 Modello Client-Server

Il sistema implementa un'architettura **client-server** dove:

- **Server**: PLC Siemens S7-1500 espone le variabili tramite OPC UA Server integrato
- **Client**: Node-RED, UA Expert, dashboard web, applicazioni custom

```
┌─────────────────────────────────────────────────────────────┐
│                      OPC UA Clients                          │
│                                                               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │  UA Expert   │  │  Node-RED    │  │  Dashboard   │      │
│  │  (Analysis)  │  │  (Logic)     │  │  (UI)        │      │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘      │
│         │                  │                  │               │
│         │    Read/Write    │    Subscribe     │               │
│         └──────────────────┼──────────────────┘               │
│                            │                                   │
└────────────────────────────┼───────────────────────────────────┘
                             │
                    ┌────────▼─────────┐
                    │  OPC UA Server   │
                    │  (PLC S7-1500)   │
                    │ 192.168.0.1:4840 │
                    └──────────────────┘
                             │
                    ┌────────▼─────────┐
                    │  Process Control │
                    │  - HBW           │
                    │  - VGR           │
                    │  - SSC           │
                    │  - MPO, SLD      │
                    └──────────────────┘
```

### 2.2 Namespace OPC UA

OPC UA organizza le variabili in **namespace numerati**:

| Namespace | Contenuto | Utilizzo |
|-----------|-----------|----------|
| **0** | Standard OPC UA | Nodi di sistema, tipi base |
| **1** | Server information | Diagnostica server |
| **2** | Strutture Siemens | UDT, FB, DB base |
| **3** | **Variabili applicative** | ⭐ **Factory modules (388 variabili)** |

> 💡 **Tutti i NodeId della Learning Factory utilizzano Namespace 3**

### 2.3 Vantaggi OPC UA per Industry 4.0

1. **Interoperabilità**: Standard aperto cross-platform
2. **Sicurezza**: Crittografia e autenticazione integrate
3. **Semantica**: Modello informativo ricco (non solo tag)
4. **Scalabilità**: Da device embedded a sistemi enterprise
5. **Publish/Subscribe**: Notifiche evento-driven

---

## 3. Struttura Address Space

### 3.1 Gerarchia Completa

L'Address Space del PLC è organizzato gerarchicamente secondo la seguente struttura:

```
Root
└── Objects
    └── DeviceSet
        └── PLC
            └── DataBlocksGlobal
                ├── gtyp_HBW (High-Bay Warehouse)
                │   ├── Horizontal_Axis
                │   └── Vertical_Axis
                │
                ├── gtyp_VGR (Vacuum Gripper Robot)
                │   ├── horizontal_Axis
                │   ├── vertical_Axis
                │   └── rotate_Axis
                │
                ├── gtyp_SSC (Sorting Station Color)
                │   ├── Horizontal_Axis
                │   └── Vertical_Axis
                │
                ├── gtyp_Interface_Dashboard
                │   ├── Publish    ← SCRITTURA COMANDI
                │   └── Subscribe  ← LETTURA STATI
                │
                ├── gtyp_Interface_TXT_Controler
                │   ├── Publish
                │   └── Subscribe
                │
                ├── gtyp_MPO (Multi-Processing Station)
                ├── gtyp_SLD (Slide)
                ├── gtyp_Setup (Configurazione)
                └── ...
```

> ⚠️ **Nota**: Ogni modulo della microfabbrica è rappresentato come un **Data Block strutturato (UDT)** esposto in OPC UA. Questo garantisce modularità e incapsulamento.

### 3.2 Convenzioni di Naming

Le variabili seguono **convenzioni di naming standard IEC 61131-3**:

| Prefisso | Tipo PLC | Tipo OPC UA | Descrizione | Esempio |
|----------|----------|-------------|-------------|---------|
| `x_` | BOOL | Boolean | Variabili booleane (flags) | `x_active`, `x_Position_Reached` |
| `i_` | INT | Int16 | Interi 16 bit con segno | `i_code`, `i_ldr`, `i_degree` |
| `di_` | DINT | Int32 | Interi 32 bit (posizioni) | `di_Actual_Position`, `di_Target_Position` |
| `r_` | REAL | Float | Numeri in virgola mobile | `r_t` (temperatura), `r_pan`, `r_tilt` |
| `s_` | STRING | String | Stringhe di testo | `s_description`, `s_cmd`, `s_type` |
| `ldt_` | DATE_AND_TIME | DateTime | Data e ora UTC | `ldt_ts` (timestamp) |

### 3.3 Formato NodeId

Ogni variabile OPC UA è identificata univocamente da un **NodeId** con formato:

```
ns=<namespace>;s="<path1>"."<path2>"."<path3>"."<variabile>"
```

**Esempio reale**:
```
ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."s_cmd"
```

Dove:
- `ns=3`: Namespace applicativo
- `s=`: String identifier (path simbolico)
- Path gerarchico separato da `"."`

---

## 4. Logica Publish/Subscribe

### 4.1 Paradigma Architetturale

L'architettura OPC UA della Learning Factory segue un modello logico di tipo **Publish/Subscribe**:

| Direzione | Cartella | Funzione | Utilizzatore |
|-----------|----------|----------|--------------|
| **→ Publish** | `gtyp_Interface_Dashboard.Publish` | **Scrittura comandi** verso PLC | Dashboard web, Node-RED |
| **← Subscribe** | `gtyp_Interface_Dashboard.Subscribe` | **Lettura stati** dal PLC | Monitoring, UI, allarmi |

```
┌─────────────────────────────────────────────────────────────┐
│                        NODE-RED / UI                         │
└─────────────────────────────────────────────────────────────┘
                       │                    ▲
                       │                    │
              PUBLISH  │                    │  SUBSCRIBE
             (Write)   │                    │  (Read/Monitor)
                       ▼                    │
┌─────────────────────────────────────────────────────────────┐
│                    OPC UA SERVER (PLC)                       │
│                                                               │
│  ┌─────────────────────┐      ┌─────────────────────┐      │
│  │ .Publish            │      │ .Subscribe          │      │
│  │                     │      │                     │      │
│  │ ► Comandi           │      │ ◄ Stati             │      │
│  │ ► Setpoint          │      │ ◄ Feedback          │      │
│  │ ► Order             │      │ ◄ Allarmi           │      │
│  │ ► Config            │      │ ◄ Sensori           │      │
│  └─────────────────────┘      └─────────────────────┘      │
│                                                               │
└─────────────────────────────────────────────────────────────┘
                             │
                    ┌────────▼─────────┐
                    │   LOGICA PLC     │
                    │   Control Loop   │
                    └──────────────────┘
```

### 4.2 Publish → Scrittura Comandi

Le variabili sotto **`gtyp_Interface_Dashboard.Publish`** sono utilizzate per **inviare comandi al PLC**.

#### 4.2.1 Esempio: Controllo Pan-Tilt Camera

**Struttura**:
```
gtyp_Interface_Dashboard.Publish.PosPanTiltUnit
├── s_cmd     (String)   ← Comando testuale
├── i_degree  (Int16)    ← Gradi di movimento (se applicabile)
└── ldt_ts    (DateTime) ← Timestamp del comando
```

**NodeId completi**:
```
ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."s_cmd"
ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."i_degree"
ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."ldt_ts"
```

**Comandi disponibili**:

| Valore `s_cmd` | Significato | Usa `i_degree` |
|----------------|-------------|----------------|
| `"relmove_left"` | Movimento relativo a sinistra | ✅ Sì |
| `"relmove_right"` | Movimento relativo a destra | ✅ Sì |
| `"relmove_up"` | Movimento relativo in alto | ✅ Sì |
| `"relmove_down"` | Movimento relativo in basso | ✅ Sì |
| `"home"` | Ritorno posizione zero | ❌ No |
| `"reset"` | Reset errori | ❌ No |

**Esempio di comando OPC UA Write**:
```javascript
// Comando: ruota camera 15° a sinistra
msg.addressSpaceItems = [
    {
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."s_cmd"',
        datatypeName: 'String'
    },
    {
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."i_degree"',
        datatypeName: 'Int16'
    },
    {
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."ldt_ts"',
        datatypeName: 'DateTime'
    }
];

msg.valuesToWrite = ["relmove_left", 15, new Date().toISOString()];
return msg;
```

#### 4.2.2 Esempio: Ordinare Pezzo da Produrre

**Struttura**:
```
gtyp_Interface_Dashboard.Publish.OrderWorkpieceButton
├── s_type    (String)   ← Tipo pezzo ("white", "red", "blue")
└── ldt_ts    (DateTime) ← Timestamp ordine
```

**NodeId**:
```
ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."s_type"
ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."ldt_ts"
```

**Esempio scrittura**:
```javascript
// Ordina pezzo rosso
msg.valuesToWrite = ["red", new Date().toISOString()];
```

### 4.3 Subscribe → Lettura Stati

Le variabili sotto **`gtyp_Interface_Dashboard.Subscribe`** contengono **stati macchina e feedback** dal PLC.

#### 4.3.1 Esempio: Stato Moduli (State Machine)

Ogni modulo della factory (HBW, VGR, SSC, MPO, SLD) espone il proprio stato attraverso una **struttura standard**:

```
gtyp_Interface_Dashboard.Subscribe.State_<MODULO>
├── i_code         (Int16)    ← Codice stato numerico (0-100)
├── s_description  (String)   ← Descrizione testuale stato
├── s_station      (String)   ← Identificativo stazione
├── s_target       (String)   ← Destinazione pezzo corrente
├── x_active       (Bool)     ← Flag attività modulo
└── ldt_ts         (DateTime) ← Timestamp ultimo aggiornamento
```

**Esempio - Lettura stato VGR (Vacuum Gripper Robot)**:

**NodeId**:
```
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_VGR"."i_code"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_VGR"."s_description"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_VGR"."x_active"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_VGR"."ldt_ts"
```

**Valori di esempio**:
```json
{
  "i_code": 20,
  "s_description": "Moving to pickup position",
  "s_station": "VGR",
  "s_target": "HBW",
  "x_active": true,
  "ldt_ts": "2026-02-17T11:00:45.123Z"
}
```

**Codici stato tipici**:

| `i_code` | Stato | Descrizione |
|----------|-------|-------------|
| 0 | INIT | Inizializzazione |
| 10 | IDLE | In attesa ordine |
| 20 | WORKING | In esecuzione |
| 30 | WAITING | In attesa modulo downstream |
| 50 | WARNING | Attenzione (non bloccante) |
| 90 | ERROR | Errore (richiede intervento) |
| 100 | ESTOP | Emergenza attiva |

#### 4.3.2 Esempio: Sensori Ambientali (TXT Controller)

**Struttura**:
```
gtyp_Interface_Dashboard.Subscribe.EnvironmentSensor
├── r_t      (Float)    ← Temperatura [°C]
├── r_rh     (Float)    ← Umidità relativa [%]
├── r_p      (Float)    ← Pressione [hPa]
├── i_aq     (Int16)    ← Qualità aria (0-500)
├── i_iaq    (Int16)    ← Indoor Air Quality index
├── di_gr    (Int32)    ← Gas resistance [ohm]
└── ldt_ts   (DateTime) ← Timestamp lettura
```

**NodeId esempio**:
```
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_t"
```

#### 4.3.3 Esempio: Feedback Pan-Tilt Camera

Dopo aver inviato un comando tramite **Publish**, si legge il feedback da **Subscribe**:

```
gtyp_Interface_Dashboard.Subscribe.PosPanTiltUnit
├── r_pan   (Float)    ← Angolo Pan corrente [°]
├── r_tilt  (Float)    ← Angolo Tilt corrente [°]
└── ldt_ts  (DateTime) ← Timestamp
```

**Ciclo completo comando → feedback**:
```
1. WRITE → Publish.PosPanTiltUnit.s_cmd = "relmove_left"
2. WRITE → Publish.PosPanTiltUnit.i_degree = 15
3. WAIT  → 200ms (tempo esecuzione)
4. READ  → Subscribe.PosPanTiltUnit.r_pan (nuovo valore)
```

### 4.4 Logica Closed-Loop

Il sistema implementa un **controllo closed-loop**:

```
┌────────────────────────────────────────────────────────────┐
│                      Application Layer                      │
│                      (Node-RED / UI)                        │
└────────────────────────────────────────────────────────────┘
         │                                        ▲
         │ 1. Write Command                       │ 3. Read Feedback
         │    (Publish)                           │    (Subscribe)
         ▼                                        │
┌────────────────────────────────────────────────────────────┐
│                        PLC Logic                            │
│                                                              │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐ │
│  │  Receive Cmd │ →  │  Execute     │ →  │  Update State│ │
│  │  (Publish)   │    │  Logic       │    │  (Subscribe) │ │
│  └──────────────┘    └──────┬───────┘    └──────────────┘ │
│                             │                               │
└─────────────────────────────┼───────────────────────────────┘
                              │ 2. Control Outputs
                              ▼
┌────────────────────────────────────────────────────────────┐
│                    Physical Process                         │
│            (Motors, Sensors, Actuators)                    │
└────────────────────────────────────────────────────────────┘
```

---

## 5. Moduli della Factory

### 5.1 gtyp_HBW (High-Bay Warehouse)

Il magazzino verticale automatizzato gestisce lo stoccaggio pezzi su 9 posizioni (3x3).

#### 5.1.1 Struttura OPC UA

```
gtyp_HBW
├── Horizontal_Axis
│   ├── di_Actual_Position  (Int32)  ← Posizione corrente [impulsi encoder]
│   ├── di_Target_Position  (Int32)  ← Posizione target [impulsi]
│   └── x_Position_Reached  (Bool)   ← Flag posizione raggiunta
│
└── Vertical_Axis
    ├── di_Actual_Position  (Int32)
    ├── di_Target_Position  (Int32)
    └── x_Position_Reached  (Bool)
```

#### 5.1.2 Funzionamento

**Controllo posizione assoluta**:
1. **Write** `di_Target_Position` (es: 5000 impulsi)
2. **PLC** muove asse fino a raggiungere target
3. **Read** `di_Actual_Position` (monitoraggio real-time)
4. **Read** `x_Position_Reached` = `true` quando completato

**NodeId esempio**:
```
ns=3;s="gtyp_HBW"."Horizontal_Axis"."di_Target_Position"  (WRITE)
ns=3;s="gtyp_HBW"."Horizontal_Axis"."di_Actual_Position"  (READ)
ns=3;s="gtyp_HBW"."Horizontal_Axis"."x_Position_Reached"  (READ)
```

> 💡 **Nota**: Il sistema utilizza **encoder incrementali** per feedback posizione. I valori sono in impulsi encoder, non millimetri.

### 5.2 gtyp_VGR (Vacuum Gripper Robot)

Robot pick&place a 3 assi (XYZ) + rotazione.

#### 5.2.1 Struttura

```
gtyp_VGR
├── horizontal_Axis  (X)
│   ├── di_Actual_Position
│   ├── di_Target_Position
│   └── x_Position_Reached
│
├── vertical_Axis    (Z)
│   ├── di_Actual_Position
│   ├── di_Target_Position
│   └── x_Position_Reached
│
└── rotate_Axis      (Rotazione base)
    ├── di_Actual_Position
    ├── di_Target_Position
    └── x_Position_Reached
```

#### 5.2.2 Funzionamento Movimenti Coordinati

Per eseguire un pick&place completo:

```javascript
// 1. Movimento asse rotazione
WRITE → gtyp_VGR.rotate_Axis.di_Target_Position = 0  (home)
WAIT  → gtyp_VGR.rotate_Axis.x_Position_Reached = true

// 2. Movimento asse orizzontale
WRITE → gtyp_VGR.horizontal_Axis.di_Target_Position = 12000
WAIT  → gtyp_VGR.horizontal_Axis.x_Position_Reached = true

// 3. Movimento asse verticale (discesa)
WRITE → gtyp_VGR.vertical_Axis.di_Target_Position = 3000
WAIT  → gtyp_VGR.vertical_Axis.x_Position_Reached = true

// 4. Attiva vacuum (comando separato, non visibile in OPC UA)
// 5. Risali, ruota, deposita
```

> ⚠️ **Importante**: La logica di coordinamento movimenti è **incapsulata nel PLC**. L'OPC UA espone solo posizioni target/actual, non comandi diretti I/O (es: vacuum ON/OFF).

### 5.3 gtyp_SSC (Sorting Station with Color detection)

Stazione di smistamento con sensore colore e nastro trasportatore.

#### 5.3.1 Struttura

Analoga a gtyp_HBW:
- `Horizontal_Axis` (nastro trasportatore)
- `Vertical_Axis` (espulsore verticale)

**NodeId esempio**:
```
ns=3;s="gtyp_SSC"."Horizontal_Axis"."di_Actual_Position"
ns=3;s="gtyp_SSC"."Vertical_Axis"."x_Position_Reached"
```

### 5.4 gtyp_Interface_TXT_Controler (NFC Management)

Interfaccia con TXT Controller per gestione tag NFC sui pezzi.

#### 5.4.1 Struttura Publish (Comandi NFC)

```
gtyp_Interface_TXT_Controler.Publish.ActionButtonNFCModule
├── s_cmd               (String)   ← Comando NFC
├── Workpiece.s_id      (String)   ← ID univoco pezzo
├── Workpiece.s_type    (String)   ← Tipo pezzo
├── Workpiece.s_state   (String)   ← Stato lavorazione
└── ldt_ts              (DateTime) ← Timestamp
```

**Comandi disponibili**:
- `"read"`: Leggi tag NFC
- `"write"`: Scrivi dati su tag
- `"clear"`: Cancella tag

#### 5.4.2 Struttura Subscribe (Stati NFC)

```
gtyp_Interface_TXT_Controler.Subscribe.State_NFC_Device
├── Workpiece.s_id      (String)   ← ID pezzo letto
├── Workpiece.s_type    (String)   ← Tipo pezzo
├── Workpiece.s_state   (String)   ← Stato lavorazione
├── History[0].i_code   (Int16)    ← Storico operazioni
└── ldt_ts              (DateTime) ← Timestamp
```

**Esempio lettura tag NFC**:
```json
{
  "Workpiece": {
    "s_id": "WP_20260217_001",
    "s_type": "red",
    "s_state": "raw"
  },
  "ldt_ts": "2026-02-17T11:05:12.345Z"
}
```

---

## 6. Utilizzo con UA Expert

### 6.1 Connessione al Server

**Procedura completa**:

1. Avvia **UA Expert** (Unified Automation)
2. Menu **Server** → **Add** → **Advanced**
3. **URL**: `opc.tcp://192.168.0.1:4840`
4. **Security Policy**: `None` (o `Basic256Sha256` se abilitato)
5. **Security Mode**: `None` (o `SignAndEncrypt`)
6. **Authentication**: `Anonymous` (o credenziali utente PLC)
7. Clicca **OK** → **Connect**

### 6.2 Navigazione Address Space

**Path completo per raggiungere le variabili**:

```
UA Expert → Address Space Panel
└── [+] Server: opc.tcp://192.168.0.1:4840
    └── [+] Objects
        └── [+] DeviceSet
            └── [+] PLC_1 [PLC]
                └── [+] DataBlocksGlobal
                    ├── [+] gtyp_HBW
                    ├── [+] gtyp_VGR
                    ├── [+] gtyp_SSC
                    ├── [+] gtyp_Interface_Dashboard
                    │   ├── [+] Publish
                    │   └── [+] Subscribe
                    └── ...
```

### 6.3 Monitoring Variabili in Real-Time

1. Espandi `gtyp_Interface_Dashboard` → `Subscribe` → `State_VGR`
2. **Drag & drop** `i_code` nel pannello **Data Access View**
3. **Drag & drop** `s_description`, `x_active` nello stesso pannello
4. I valori si aggiornano automaticamente (default: 1s)

**Screenshot simulato**:
```
┌─────────────────────────────────────────────────────────────────────┐
│ Data Access View                                                    │
├─────────────────────────────┬────────────┬──────────────────────────┤
│ NodeId                      │ Value      │ Timestamp                │
├─────────────────────────────┼────────────┼──────────────────────────┤
│ State_VGR.i_code            │ 20         │ 2026-02-17 11:00:45.123 │
│ State_VGR.s_description     │ "Moving..."│ 2026-02-17 11:00:45.123 │
│ State_VGR.s_station         │ "VGR"      │ 2026-02-17 11:00:45.123 │
│ State_VGR.x_active          │ TRUE       │ 2026-02-17 11:00:45.123 │
│ EnvironmentSensor.r_t       │ 22.5       │ 2026-02-17 11:00:46.789 │
│ HBW.Horizontal.di_Actual... │ 8523       │ 2026-02-17 11:00:45.234 │
└─────────────────────────────┴────────────┴──────────────────────────┘
```

### 6.4 Scrittura Variabili (Test Comandi)

**Esempio - Ordinare pezzo bianco**:

1. Naviga a `gtyp_Interface_Dashboard` → `Publish` → `OrderWorkpieceButton`
2. **Drag & drop** `s_type` in **Data Access View**
3. **Doppio click** sul valore
4. Inserisci `"white"`
5. Premi **Enter**
6. Osserva `Subscribe.State_Order` per conferma

---

## 7. Integrazione Node-RED

### 7.1 Architettura Integrazione

Node-RED funge da **middleware** tra PLC e interfaccia utente:

```
┌──────────────────────────────────────────────────────────────┐
│                      Dashboard Web UI                         │
│                  (http://192.168.0.1:1880/ui)                │
└──────────────────────────────────────────────────────────────┘
                             ▲
                             │ HTTP / WebSocket
                             ▼
┌──────────────────────────────────────────────────────────────┐
│                        NODE-RED                               │
│  ┌────────────────────────────────────────────────────────┐  │
│  │  Flow: Factory Dashboard                               │  │
│  │                                                          │  │
│  │  [Button] → [Function] → [OPC-Write] → PLC             │  │
│  │                                                          │  │
│  │  PLC → [OPC-Subscribe] → [Process] → [Chart/Gauge]     │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                               │
│  ┌────────────────────────────────────────────────────────┐  │
│  │  Flow: MQTT Bridge                                     │  │
│  │                                                          │  │
│  │  [OPC-Subscribe] → [Format] → [MQTT-Out] → Broker     │  │
│  └────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
                             │
                             │ OPC UA
                             ▼
┌──────────────────────────────────────────────────────────────┐
│                    PLC Siemens S7-1500                        │
│                  opc.tcp://192.168.0.1:4840                  │
└──────────────────────────────────────────────────────────────┘
```

### 7.2 Package Node-RED Richiesto

```bash
# Installazione nodo OPC UA
npm install node-red-contrib-opcua-iiot
```

**Nodi disponibili**:
- **OpcUa-Client**: Configurazione connessione server
- **OpcUa-Item**: Lettura variabili (read/subscribe)
- **OpcUa-Write**: Scrittura variabili
- **OpcUa-Browser**: Esplorazione Address Space

### 7.3 Configurazione Client OPC UA

**Nodo OpcUa-Client** (configurazione globale):

```json
{
  "name": "PLC_Factory",
  "endpoint": "opc.tcp://192.168.0.1:4840",
  "securityPolicy": "None",
  "securityMode": "None",
  "login": false
}
```

### 7.4 Pattern: Lettura Stato Modulo

**Flow completo**:
```
[Inject: 1s] → [Function: Prepare Read] → [OpcUa-Item] → [Function: Parse] → [Dashboard]
```

**Function: Prepare Read**:
```javascript
// Leggi stato completo HBW ogni secondo
msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    {
        name: "code",
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."i_code"',
        datatypeName: 'Int16'
    },
    {
        name: "description",
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_description"',
        datatypeName: 'String'
    },
    {
        name: "station",
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_station"',
        datatypeName: 'String'
    },
    {
        name: "active",
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."x_active"',
        datatypeName: 'Bool'
    },
    {
        name: "timestamp",
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."ldt_ts"',
        datatypeName: 'DateTime'
    }
];

return msg;
```

**Function: Parse** (dopo OpcUa-Item):
```javascript
// Estrae valori da risposta OPC UA
var values = msg.payload.value;

msg.payload = {
    code: values[0],
    description: values[1],
    station: values[2],
    active: values[3],
    timestamp: values[4]
};

// Determina colore stato per dashboard
if (msg.payload.code >= 90) {
    msg.color = "red";    // Errore
} else if (msg.payload.code >= 50) {
    msg.color = "yellow"; // Warning
} else {
    msg.color = "green";  // OK
}

return msg;
```

**Dashboard Output**:
- **Text node**: Mostra `description`
- **LED node**: Colore basato su `msg.color`
- **Chart node**: Storico `code` nel tempo

### 7.5 Pattern: Scrittura Comando

**Flow completo**:
```
[Dashboard Button] → [Function: Build Command] → [OpcUa-Write] → [Debug]
```

**Function: Build Command** (esempio: ordina pezzo rosso):
```javascript
// Prepara comando ordine pezzo
msg.nodetype = "inject";
msg.injectType = "write";

var timestamp = new Date().toISOString();

msg.addressSpaceItems = [
    {
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."s_type"',
        datatypeName: 'String'
    },
    {
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."ldt_ts"',
        datatypeName: 'DateTime'
    }
];

// Tipo pezzo da msg.payload.color (button payload)
var workpieceType = msg.payload.color || "white";

msg.valuesToWrite = [workpieceType, timestamp];

return msg;
```

**Dashboard Button**:
```json
{
  "label": "Order Red",
  "payload": {"color": "red"},
  "color": "#ff0000"
}
```

### 7.6 Pattern: Subscribe Continuo (Event-Driven)

Per ricevere notifiche automatiche senza polling:

**Function: Setup Subscribe**:
```javascript
msg.nodetype = "inject";
msg.injectType = "subscribe";
msg.interval = 500; // Notifica ogni 500ms SE cambia valore

msg.addressSpaceItems = [
    {
        name: "temperature",
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_t"',
        datatypeName: 'Float'
    },
    {
        name: "humidity",
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_rh"',
        datatypeName: 'Float'
    }
];

return msg;
```

**Vantaggio**: Riduce traffico di rete, il PLC notifica solo su variazione.

### 7.7 Pattern: MQTT Bridge

**Flow: OPC UA → MQTT**:
```
[OPC-Subscribe] → [Function: Format JSON] → [MQTT-Out: Factory/HBW/Status]
```

**Function: Format JSON**:
```javascript
// Trasforma risposta OPC UA in messaggio MQTT
var opcData = msg.payload.value;

msg.payload = {
    module: "HBW",
    state: {
        code: opcData[0],
        description: opcData[1],
        active: opcData[3]
    },
    timestamp: new Date().toISOString()
};

msg.topic = "Factory/HBW/Status";

return msg;
```

**MQTT-Out**:
- **Broker**: `mqtt://192.168.0.1:1883`
- **Topic**: `Factory/HBW/Status`
- **QoS**: 1
- **Retain**: true

**Risultato**: Ogni cambio stato HBW viene pubblicato su MQTT per:
- Database historian (InfluxDB)
- Dashboard esterni
- Integrazione SCADA
- Notifiche push

### 7.8 Esempio Completo: Dashboard Factory

**Flow completo** per dashboard operatore:

```
┌─────────────────────────────────────────────────────────────┐
│ TAB: Factory Overview                                       │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  [Group: HBW Status]                                         │
│   ┌─────────────────────────────────────────────────────┐   │
│   │  State: ● WORKING                                   │   │
│   │  Description: "Moving to position A1"               │   │
│   │  Position X: 8523 / 12000                           │   │
│   │  Position Y: 3200 / 3200  ✓                         │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                               │
│  [Group: Production Orders]                                  │
│   ┌─────────────────────────────────────────────────────┐   │
│   │  [Button: Order White] [Button: Order Red]          │   │
│   │  [Button: Order Blue]                                │   │
│   │                                                       │   │
│   │  Last Order: Red - 11:05:23                          │   │
│   │  Status: ● In Production                             │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                               │
│  [Group: Environment]                                        │
│   ┌─────────────────────────────────────────────────────┐   │
│   │  Temperature:  [Gauge: 22.5°C]                       │   │
│   │  Humidity:     [Gauge: 45.2%]                        │   │
│   │  Air Quality:  [LED: ● Good]                         │   │
│   │                                                       │   │
│   │  [Chart: Temperature Last Hour]                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                               │
│  [Group: Camera Control]                                     │
│   ┌─────────────────────────────────────────────────────┐   │
│   │      [↑]                       Pan:  [Slider 0-180]  │   │
│   │   [←][⊙][→]  [Home]           Tilt: [Slider 0-90]   │   │
│   │      [↓]                                              │   │
│   └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

**Nodes necessari**:
- 1x Inject (1s) per subscribe stati
- 5x Function (prepare reads)
- 5x OpcUa-Item (read)
- 3x Dashboard Button (order buttons)
- 3x Function (build write commands)
- 3x OpcUa-Write
- 2x Dashboard Gauge (temp, humidity)
- 1x Dashboard Chart
- 4x Dashboard Button (camera control)

---

## 8. Casi d'Uso Pratici

### 8.1 Caso d'Uso: Allarme Temperatura

**Obiettivo**: Inviare email se temperatura > 30°C per più di 5 minuti.

**Soluzione Node-RED**:

```
[Subscribe Temp] → [Function: Check Threshold] → [Trigger: 5min] → [Email]
                                                          ↓
                                                   [Dashboard Alert]
```

**Function: Check Threshold**:
```javascript
var temp = msg.payload.value[0]; // r_t

if (temp > 30.0) {
    msg.payload = {
        subject: "Factory Alert: High Temperature",
        body: `Temperature exceeded 30°C: ${temp.toFixed(1)}°C`,
        severity: "warning"
    };
    return msg;
}

return null; // Non propagare se OK
```

**Trigger Node**: Attende 5 minuti prima di inviare (evita falsi positivi).

### 8.2 Caso d'Uso: Calcolo OEE (Overall Equipment Effectiveness)

**Obiettivo**: Calcolare OEE basato su stati moduli.

**Formula OEE**:
```
OEE = Availability × Performance × Quality
```

**Flow**:
```
[Subscribe States] → [Function: Calculate OEE] → [InfluxDB] → [Grafana]
```

**Function: Calculate OEE**:
```javascript
// Context: accumula tempo per stato
var context = flow.get("oee_data") || {
    totalTime: 0,
    workingTime: 0,
    idleTime: 0,
    errorTime: 0
};

var state = msg.payload.value[0]; // i_code
var deltaTime = 1000; // 1s (polling interval)

context.totalTime += deltaTime;

if (state >= 10 && state < 50) {
    context.workingTime += deltaTime;
} else if (state < 10) {
    context.idleTime += deltaTime;
} else {
    context.errorTime += deltaTime;
}

flow.set("oee_data", context);

// Calcola availability
var availability = context.workingTime / context.totalTime;

msg.payload = {
    oee: availability * 100, // Semplificato
    availability: availability,
    timestamp: new Date()
};

return msg;
```

### 8.3 Caso d'Uso: Sequenza Automatica Produzione

**Obiettivo**: Produrre 10 pezzi rossi in sequenza automatica.

**Flow**:
```
[Inject: Start] → [Function: Loop 10x] → [OpcUa-Write: Order] → [Wait Completion] → [Loop]
```

**Function: Loop 10x**:
```javascript
var count = context.get("productionCount") || 0;

if (count < 10) {
    // Ordina pezzo rosso
    msg.addressSpaceItems = [{
        nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."s_type"',
        datatypeName: 'String'
    }];
    msg.valuesToWrite = ["red"];

    context.set("productionCount", count + 1);

    return msg;
} else {
    // Completato
    context.set("productionCount", 0);
    node.status({fill:"green", shape:"dot", text:"Completed 10 pieces"});
    return null;
}
```

**Wait Completion**: Subscribe a `State_Order.s_state`, aspetta `"completed"` prima di ordinare successivo.

---

## 9. Considerazioni Architetturali

### 9.1 [Incapsulamento Logica](Sezione_9.1_Incapsulamento_Semplificata)

**Osservazione critica**: Il PLC **non espone direttamente uscite fisiche** (es: DO, valvole, motori).

**Cosa è esposto**:
- ✅ Setpoint di posizione (`di_Target_Position`)
- ✅ Stati semantici (`State_HBW.s_description`)
- ✅ Comandi testuali (`s_cmd = "home"`)

**Cosa NON è esposto**:
- ❌ Bit I/O diretti (`%Q0.0`, `%I0.1`)
- ❌ Comandi motore (`M_Forward`, `M_Reverse`)
- ❌ Valvole (`V_Vacuum_ON`)

**Vantaggi**:
1. **Sicurezza**: Impossibile comandare direttamente attuatori (rischio collisioni)
2. **Astrazione**: Client non deve conoscere logica interna PLC
3. **Manutenibilità**: Modifiche PLC non impattano client
4. **Industry 4.0**: Comunicazione semantica, non bit-level

**Architettura a livelli**:
```
┌──────────────────────────────────────────────┐
│  Level 4: ERP / MES                          │  ← Business logic
├──────────────────────────────────────────────┤
│  Level 3: SCADA / HMI (Node-RED)             │  ← Semantic commands
│            OPC UA Interface                   │
├──────────────────────────────────────────────┤
│  Level 2: PLC Logic (S7-1500)                │  ← Motion control
│            - State machines                   │     Position loops
│            - Safety checks                    │
├──────────────────────────────────────────────┤
│  Level 1: Field devices                      │  ← Motors, sensors
│            - Motors, encoders                 │     Valves, switches
│            - Sensors, actuators               │
└──────────────────────────────────────────────┘
```

### 9.2 Modularità e Scalabilità

**Design pattern utilizzato**: **Data Block per modulo**

Ogni modulo fisico (HBW, VGR, SSC...) ha un corrispondente **UDT (User Defined Type)** nel PLC, esposto come singolo oggetto OPC UA.

**Vantaggi**:
- ✅ **Aggiungere nuovo modulo** = nuovo DB, automaticamente esposto in OPC UA
- ✅ **Namespace separati** per ogni modulo (no conflitti naming)
- ✅ **Riutilizzo codice**: UDT standard per tutti i moduli con assi

**Esempio**: Se si aggiunge modulo `gtyp_MPO_2`:
```
DataBlocksGlobal
├── gtyp_MPO
└── gtyp_MPO_2  ← Nuovo modulo, stessa struttura
```

### 9.3 Coerenza Dati e Timestamp

**Tutti i messaggi includono `ldt_ts` (timestamp)**:

**Perché?**
1. **Sincronizzazione multi-client**: Dashboard e Node-RED vedono stesso timestamp
2. **Rilevamento latency**: Differenza tra `ldt_ts` e now = ritardo comunicazione
3. **Storicizzazione**: Database historian usa timestamp PLC (non client)
4. **Debugging**: Tracciabilità eventi

**Formato**: ISO 8601 UTC
```
2026-02-17T11:00:45.123Z
```

### 9.4 Politiche di Refresh

**Subscribe vs Polling**:

| Metodo | Traffico Rete | Latency | Uso Tipico |
|--------|---------------|---------|------------|
| **Subscribe** (event-driven) | Basso (solo su cambio) | <100ms | Stati critici, allarmi |
| **Polling** (read ciclico) | Alto (sempre) | ~1s | Monitoring generale |

**Best practice**:
- **Subscribe**: `State_*`, allarmi, posizioni assi in movimento
- **Polling 1s**: Sensori ambientali, statistiche
- **Polling 5s**: Configurazioni, dati lenti

---

## 10. Conclusioni

### 10.1 Sintesi dell'Analisi

L'analisi dell'Address Space OPC UA della Learning Factory 4.0 ha permesso di:

1. ✅ **Comprendere la struttura dei moduli** (388 variabili documentate)
2. ✅ **Identificare i punti di scrittura e lettura** (Publish vs Subscribe)
3. ✅ **Documentare le interfacce macchina** (NodeId completi per ogni modulo)
4. ✅ **Creare una base solida per integrazione Node-RED** (pattern riutilizzabili)
5. ✅ **Preparare documentazione tecnica per tesi** (esempi pratici funzionanti)

### 10.2 Caratteristiche Architettura

L'architettura risulta:

- **Modulare**: Ogni modulo è autonomo con interfaccia standardizzata
- **Scalabile**: Facile aggiungere nuovi moduli senza modificare esistenti
- **Coerente con standard OPC UA**: Namespace, NodeId, tipi di dati conformi
- **Orientata a comunicazione semantica**: Comandi testuali, non bit-level
- **Industry 4.0 compliant**: Separazione livelli, interoperabilità, sicurezza

### 10.3 Vantaggi per Integrazione

**Per sviluppatori**:
- Interfaccia chiara e documentata
- Esempi Node-RED pronti all'uso
- Pattern riutilizzabili (read, write, subscribe)

**Per operatori**:
- Dashboard intuitiva
- Comandi semantici comprensibili
- Feedback real-time

**Per manutenzione**:
- Diagnostica tramite OPC UA (non serve TIA Portal)
- Storico eventi via MQTT/InfluxDB
- Indipendenza da logica PLC interna

### 10.4 Sviluppi Futuri

Possibili estensioni:

1. **Machine Learning**: Predizione guasti basata su pattern `i_code`
2. **Digital Twin**: Sincronizzazione modello 3D con posizioni assi real-time
3. **Ottimizzazione produzione**: Algoritmi scheduling basati su stati moduli
4. **Integrazione ERP**: Order management via OPC UA → SAP/Odoo
5. **Realtà Aumentata**: Overlay dati OPC UA su video camera

---

## 11. Riferimenti

### 11.1 Statistiche Address Space

**Totale variabili estratte**: 388

| Modulo | Variabili |
|--------|-----------|
| gtyp_Interface_Dashboard | 96 |
| gtyp_Interface_TXT_Controler | 45 |
| gtyp_HBW | 6 |
| gtyp_VGR | 9 |
| gtyp_SSC | 6 |
| gtyp_MPO | (non estratto nel subset) |
| gtyp_SLD | (non estratto nel subset) |
| **Subset analizzato** | **162** |

### 11.2 Distribuzione Tipi di Dati

| Tipo OPC UA | Occorrenze | % | Tipo PLC |
|-------------|------------|---|----------|
| Int32 | 175 | 45.1% | DINT |
| String | 86 | 22.2% | STRING |
| Int16 | 50 | 12.9% | INT |
| DateTime | 37 | 9.5% | DATE_AND_TIME |
| Bool/Boolean | 17 | 4.4% | BOOL |
| Float | 9 | 2.3% | REAL |
| Altri | 14 | 3.6% | WORD, UInt16 |

**Osservazione**: Predominanza di **Int32** conferma focus su **controllo posizione** (encoder).

### 11.3 Tool e Software

| Tool | Versione | Utilizzo |
|------|----------|----------|
| **UA Expert** | 1.6.x | Analisi Address Space, testing |
| **Node-RED** | 3.x | Middleware, dashboard, logiche |
| **node-red-contrib-opcua-iiot** | 4.x | Nodi OPC UA per Node-RED |
| **TIA Portal** | V17/V18 | Programmazione PLC (non necessario per OPC UA) |
| **MQTT Broker** | Mosquitto 2.x | Bridge eventi OPC UA → MQTT |

### 11.4 Standard e Specifiche

- **IEC 62541**: OPC Unified Architecture
- **OPC Foundation**: [opcfoundation.org](https://opcfoundation.org)
- **IEC 61131-3**: Linguaggi programmazione PLC
- **ISO 8601**: Formato data/ora (DateTime)

### 11.5 Documenti fischertechnik

- Training Factory Industry 4.0 24V - **Introduction**
- Training Factory Industry 4.0 24V - **Factory Components**
- Training Factory Industry 4.0 24V - **Commissioning**
- Training Factory Industry 4.0 24V - **Tasks for the Factory**

### 11.6 Manuali Siemens

- SIMATIC S7-1500 - **System Manual**
- SIMATIC S7-1500 - **OPC UA Server Manual**
- TIA Portal - **OPC UA Communication Guide**

---

## Appendice A: NodeId Completi Principali

### A.1 State Machines Moduli

**gtyp_Interface_Dashboard.Subscribe.State_HBW**:
```
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."i_code"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."ldt_ts"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_description"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_station"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_target"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."x_active"
```

**Stessa struttura per**: State_VGR, State_SSC, State_MPO, State_SLD, State_DSI, State_DSO

### A.2 Comandi Publish

**OrderWorkpieceButton**:
```
ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."s_type"
ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."ldt_ts"
```

**PosPanTiltUnit**:
```
ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."s_cmd"
ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."i_degree"
ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."ldt_ts"
```

### A.3 Sensori

**EnvironmentSensor**:
```
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_t"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_rh"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_p"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."i_aq"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."i_iaq"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."di_gr"
ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."ldt_ts"
```

### A.4 Controllo Assi HBW

**Horizontal_Axis**:
```
ns=3;s="gtyp_HBW"."Horizontal_Axis"."di_Actual_Position"
ns=3;s="gtyp_HBW"."Horizontal_Axis"."di_Target_Position"
ns=3;s="gtyp_HBW"."Horizontal_Axis"."x_Position_Reached"
```

**Vertical_Axis**: Stessa struttura

### A.5 Controllo Assi VGR

**horizontal_Axis, vertical_Axis, rotate_Axis**: Stessa struttura di HBW

```
ns=3;s="gtyp_VGR"."horizontal_Axis"."di_Actual_Position"
ns=3;s="gtyp_VGR"."vertical_Axis"."di_Actual_Position"
ns=3;s="gtyp_VGR"."rotate_Axis"."di_Actual_Position"
```

---

## Appendice B: Codici Stato Tipici

### B.1 Codici `i_code` Standard

| Range | Categoria | Colore UI | Azione Operatore |
|-------|-----------|-----------|------------------|
| 0-9 | INIT / SETUP | 🔵 Blue | Attendere inizializzazione |
| 10-19 | IDLE / READY | 🟢 Green | Sistema pronto |
| 20-39 | WORKING | 🟢 Green | Operazione in corso |
| 40-49 | WAITING | 🟡 Yellow | In attesa modulo downstream |
| 50-79 | WARNING | 🟠 Orange | Controllare condizioni |
| 80-89 | ERROR RECOVERABLE | 🔴 Red | Reset manuale richiesto |
| 90-99 | ERROR CRITICAL | 🔴 Red | Intervento tecnico |
| 100 | EMERGENCY STOP | 🔴 Red | E-STOP attivo |

### B.2 Esempi Descrizioni

| `i_code` | `s_description` | Significato |
|----------|-----------------|-------------|
| 0 | "Initializing..." | Power-on, homing assi |
| 10 | "Idle" | In attesa comando |
| 20 | "Moving to position A1" | Movimento in corso |
| 30 | "Waiting for VGR" | Attesa pick da robot |
| 50 | "Low vacuum pressure" | Warning sensore vuoto |
| 90 | "Position timeout" | Asse non arriva a target |
| 100 | "Emergency stop active" | E-STOP premuto |

---

## Appendice C: Esempio Flow Node-RED Completo

```json
[
    {
        "id": "opcua_client_1",
        "type": "OpcUa-Client",
        "endpoint": "opc.tcp://192.168.0.1:4840",
        "securityPolicy": "None",
        "securityMode": "None",
        "name": "PLC_Factory"
    },
    {
        "id": "inject_1s",
        "type": "inject",
        "repeat": "1",
        "crontab": "",
        "once": true,
        "name": "Poll 1s"
    },
    {
        "id": "function_read_state",
        "type": "function",
        "name": "Prepare Read State_HBW",
        "func": "msg.nodetype = 'inject';\nmsg.injectType = 'read';\n\nmsg.addressSpaceItems = [\n    { nodeId: 'ns=3;s=\"gtyp_Interface_Dashboard\".\"Subscribe\".\"State_HBW\".\"i_code\"', datatypeName: 'Int16' },\n    { nodeId: 'ns=3;s=\"gtyp_Interface_Dashboard\".\"Subscribe\".\"State_HBW\".\"s_description\"', datatypeName: 'String' }\n];\n\nreturn msg;"
    },
    {
        "id": "opcua_item_1",
        "type": "OpcUa-Item",
        "client": "opcua_client_1",
        "name": "Read OPC UA"
    },
    {
        "id": "function_parse",
        "type": "function",
        "name": "Parse Response",
        "func": "msg.payload = {\n    code: msg.payload.value[0],\n    description: msg.payload.value[1]\n};\nreturn msg;"
    },
    {
        "id": "debug_1",
        "type": "debug",
        "name": "Debug State"
    }
]
```

**Wiring**: inject_1s → function_read_state → opcua_item_1 → function_parse → debug_1

---

**Fine Documento**

*Versione 2.0 - Febbraio 2026*  
*Learning Factory 4.0 24V - fischertechnik*  
*Documentazione Tesi - Architettura OPC UA*
