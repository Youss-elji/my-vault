# Indirizzamento OPC UA - Learning Factory 4.0 24V

**Data documento**: 11 Febbraio 2026  
**Versione**: 1.0  
**Server OPC UA**: Siemens S7-1500 PLC @ `opc.tcp://192.168.0.1:4840`

---

## 1. Introduzione

Questo documento descrive l'**Address Space OPC UA** del PLC Siemens S7-1500 nella Learning Factory 4.0 e come accedere ai dati tramite **UA Expert** e **Node-RED**.

### 1.1 Architettura di comunicazione

```
┌─────────────────┐
│  PLC S7-1500    │ OPC UA Server
│  192.168.0.1    │ porta 4840
└────────┬────────┘
         │ OPC UA Protocol
         │
┌────────▼────────┐
│  Gateway IoT    │ OPC UA Client
│  Raspberry Pi   │ + Node-RED
│  192.168.0.5    │
└─────────────────┘
```

---

## 2. Namespace e Identificatori

### 2.1 Formato NodeId

Tutti i nodi OPC UA del PLC seguono questa convenzione:

```
ns=<namespace_index>;s="<string_identifier>"
```

**Componenti**:
- `ns` = Namespace Index (tipicamente **3** per i dati applicativi)
- `s` = String Identifier (nome simbolico della variabile)

**Esempio**:
```
ns=3;s="gtyp_HBW"
```

### 2.2 Convenzioni di naming

Le variabili PLC utilizzano il prefisso **`gtyp_`** (Global Type) seguito dal nome del modulo:

| Prefisso | Significato | Esempio |
|----------|-------------|---------|
| `gtyp_` | Global Type | `gtyp_HBW` |
| Notazione punto | Gerarchia | `gtyp_Interface_Dashboard.EnvironmentSensor` |
| Underscore | Separatore parole | `Interface_TXT_Controler` |

---

## 3. Address Space - Struttura Principale

### 3.1 Root Objects

Nell'**Address Space** di UA Expert, i nodi principali si trovano sotto:

```
Objects (ns=0;i=85)
  └─ Server (ns=0;i=2253)
  └─ <Namespace 3 - Application Data>
      ├─ gtyp_HBW
      ├─ gtyp_Interface_Dashboard
      ├─ gtyp_Interface_TXT_Controler
      ├─ gtyp_MPO
      ├─ gtyp_SLD
      ├─ gtyp_SSC
      ├─ gtyp_Setup
      ├─ gtyp_SetupAxis
      └─ gtyp_VGR
```

### 3.2 Moduli Principali (Namespace 3)

| NodeId | Nome Modulo | Descrizione |
|--------|-------------|-------------|
| `ns=3;s="gtyp_HBW"` | High-Bay Warehouse | Magazzino verticale automatizzato |
| `ns=3;s="gtyp_VGR"` | Vacuum Gripper Robot | Robot con ventosa a vuoto |
| `ns=3;s="gtyp_SSC"` | Sorting Station Camera | Stazione smistamento con camera |
| `ns=3;s="gtyp_MPO"` | Multi-Processing Oven | Forno multi-processo |
| `ns=3;s="gtyp_SLD"` | Sorting Line with Detection | Linea smistamento |
| `ns=3;s="gtyp_Setup"` | Setup & Calibration | Configurazione sistema |
| `ns=3;s="gtyp_SetupAxis"` | Axis Setup | Setup assi movimento |
| `ns=3;s="gtyp_Interface_Dashboard"` | Dashboard Interface | Interfaccia sensori TXT |
| `ns=3;s="gtyp_Interface_TXT_Controler"` | TXT Controller | Controller fischertechnik TXT |

---

## 4. gtyp_Interface_Dashboard

### 4.1 Descrizione

Il nodo **`gtyp_Interface_Dashboard`** espone i dati dei **sensori connessi al TXT Controller**:
- Sensore ambientale **BME680**
- Fotoresistenza **LDR**
- **Camera USB**

### 4.2 Struttura Address Space

```
ns=3;s="gtyp_Interface_Dashboard"
  │
  ├─ EnvironmentSensor (BME680)
  │   ├─ t          → Temperatura (°C)
  │   ├─ h          → Umidità (% r.H.)
  │   ├─ p          → Pressione (hPa)
  │   ├─ iaq        → Air Quality Index
  │   ├─ aq         → Air Quality Accuracy
  │   └─ ts         → Timestamp
  │
  ├─ BrightnessSensor (LDR)
  │   ├─ br         → Luminosità (%)
  │   └─ ts         → Timestamp
  │
  └─ CameraPicture
      ├─ s_data     → Immagine (base64)
      └─ ldt_ts     → Timestamp
```

### 4.3 Accesso da UA Expert

Per leggere la temperatura del sensore BME680:

1. Naviga nell'Address Space:
   ```
   Objects → <ns=3> → gtyp_Interface_Dashboard → EnvironmentSensor → t
   ```

2. NodeId completo:
   ```
   ns=3;s="gtyp_Interface_Dashboard.EnvironmentSensor.t"
   ```

3. **Drag & Drop** il nodo nella finestra di **Monitoring** per vedere i valori in tempo reale

### 4.4 Tabella Variabili

#### Sensore Ambientale BME680

| Variabile | NodeId | Tipo Dati | Unità | Descrizione |
|-----------|--------|-----------|-------|-------------|
| Temperatura | `ns=3;s="gtyp_Interface_Dashboard.EnvironmentSensor.t"` | REAL | °C | Temperatura ambiente |
| Umidità | `ns=3;s="gtyp_Interface_Dashboard.EnvironmentSensor.h"` | REAL | % r.H. | Umidità relativa |
| Pressione | `ns=3;s="gtyp_Interface_Dashboard.EnvironmentSensor.p"` | REAL | hPa | Pressione atmosferica |
| IAQ | `ns=3;s="gtyp_Interface_Dashboard.EnvironmentSensor.iaq"` | INT | - | Indice qualità aria (0-500) |
| Accuratezza | `ns=3;s="gtyp_Interface_Dashboard.EnvironmentSensor.aq"` | INT | - | Accuratezza IAQ (0-3) |

#### Fotoresistenza LDR

| Variabile | NodeId | Tipo Dati | Unità | Descrizione |
|-----------|--------|-----------|-------|-------------|
| Luminosità | `ns=3;s="gtyp_Interface_Dashboard.BrightnessSensor.br"` | REAL | % | Percentuale luminosità |

#### Camera USB

| Variabile | NodeId | Tipo Dati | Descrizione |
|-----------|--------|-----------|-------------|
| Immagine | `ns=3;s="gtyp_Interface_Dashboard.CameraPicture.s_data"` | STRING | Immagine codificata base64 |
| Timestamp | `ns=3;s="gtyp_Interface_Dashboard.CameraPicture.ldt_ts"` | STRING | Data/ora acquisizione |

---

## 5. Moduli Fabbrica

### 5.1 High-Bay Warehouse (HBW)

**NodeId Base**: `ns=3;s="gtyp_HBW"`

**Struttura tipica** (da esplorare in UA Expert):

```
gtyp_HBW
  ├─ Status
  │   ├─ xReady           → Pronto
  │   ├─ xError           → Errore
  │   └─ xBusy            → Occupato
  │
  ├─ Command
  │   ├─ xStart           → Comando Start
  │   ├─ xStop            → Comando Stop
  │   └─ xReset           → Reset
  │
  ├─ Position
  │   ├─ nPosX            → Posizione X (colonna)
  │   ├─ nPosY            → Posizione Y (fila)
  │   └─ nPosZ            → Posizione Z (altezza)
  │
  └─ Data
      ├─ RackPositions    → Array posizioni scaffale
      └─ BeltPosition     → Posizione nastro
```

### 5.2 Vacuum Gripper Robot (VGR)

**NodeId Base**: `ns=3;s="gtyp_VGR"`

**Struttura tipica**:

```
gtyp_VGR
  ├─ Status
  │   ├─ xReady
  │   ├─ xError
  │   └─ xAtPosition
  │
  ├─ Command
  │   ├─ xGripperOn       → Attiva ventosa
  │   ├─ xGripperOff      → Disattiva ventosa
  │   └─ xMoveToPosition  → Muovi a posizione
  │
  └─ Positions
      ├─ HBW              → Posizione magazzino
      ├─ DSI              → Stazione ingresso
      ├─ DSO              → Stazione uscita
      ├─ ColorReader      → Sensore colore
      ├─ NFC              → Lettore NFC
      └─ MPO              → Multi-Processing Oven
```

### 5.3 Sorting Station Camera (SSC)

**NodeId Base**: `ns=3;s="gtyp_SSC"`

**Struttura tipica**:

```
gtyp_SSC
  ├─ Status
  │   ├─ xReady
  │   └─ xCameraActive
  │
  ├─ Command
  │   ├─ xMovePan         → Movimento orizzontale
  │   └─ xMoveTilt        → Movimento verticale
  │
  └─ Position
      ├─ nPanAngle        → Angolo Pan (°)
      ├─ nTiltAngle       → Angolo Tilt (°)
      ├─ Centre           → Posizione centrale
      └─ HBW              → Vista magazzino
```

### 5.4 Multi-Processing Oven (MPO)

**NodeId Base**: `ns=3;s="gtyp_MPO"`

### 5.5 Sorting Line (SLD)

**NodeId Base**: `ns=3;s="gtyp_SLD"`

---

## 6. gtyp_Setup - Calibrazione

**NodeId Base**: `ns=3;s="gtyp_Setup"`

### 6.1 Calibrazione Sensori Colore

```
gtyp_Setup
  ├─ x_Color_Sensor_Calibration
  │   ├─ White_R          → Riferimento bianco R
  │   ├─ White_G          → Riferimento bianco G
  │   ├─ White_B          → Riferimento bianco B
  │   ├─ Red_R            → Riferimento rosso R
  │   ├─ Red_G            → Riferimento rosso G
  │   └─ Red_B            → Riferimento rosso B
  │
  └─ x_Color_Sensor_SLD_Calibration
      └─ [struttura simile]
```

---

## 7. Come Usare OPC UA in Node-RED

### 7.1 Nodi OPC UA Utilizzati

Il flow Node-RED della Learning Factory utilizza i seguenti nodi:

| Nodo | Funzione | Esempio di utilizzo |
|------|----------|---------------------|
| **OPCUA-IIoT-Read** | Legge variabili dal PLC | Lettura stato moduli |
| **OPCUA-IIoT-Write** | Scrive variabili nel PLC | Invio comandi |
| **OPCUA-IIoT-Listener** | Subscribe a cambiamenti | Monitoring in tempo reale |

### 7.2 Configurazione Connessione

**Endpoint**: `opc.tcp://192.168.0.1:4840`

```javascript
{
  "endpoint": "opc.tcp://192.168.0.1:4840",
  "securityPolicy": "None",
  "securityMode": "None",
  "keepSessionAlive": true
}
```

### 7.3 Esempio: Lettura Temperatura

**Flusso Node-RED**:

```
[inject] → [OPCUA-IIoT-Read] → [function] → [debug]
```

**Configurazione nodo Read**:
```javascript
{
  "nodeId": "ns=3;s="gtyp_Interface_Dashboard.EnvironmentSensor.t"",
  "datatype": "Float"
}
```

### 7.4 Esempio: Scrittura Comando

**Flusso Node-RED**:

```
[inject] → [function] → [OPCUA-IIoT-Write] → [debug]
```

**Function node** (prepara il payload):
```javascript
msg.payload = {
    nodeId: 'ns=3;s="gtyp_HBW.Command.xStart"',
    datatype: 'Boolean',
    value: true
};
return msg;
```

---

## 8. Guida Pratica UA Expert

### 8.1 Connessione al Server

1. Apri **UA Expert**
2. **Server** → **Add** → **Custom Discovery**
3. Inserisci: `opc.tcp://192.168.0.1:4840`
4. Click **Get Endpoints**
5. Seleziona l'endpoint con Security Mode: **None**
6. Click **OK** e poi **Connect**

### 8.2 Navigazione Address Space

1. Espandi **Objects** nel pannello di sinistra
2. Cerca il **Namespace 3** (potrebbe essere indicato come `<ns=3>`)
3. Espandi per vedere tutti i moduli `gtyp_*`

### 8.3 Monitoring Variabili

**Per monitorare una variabile**:

1. Trascina il nodo dall'Address Space alla finestra **Data Access View**
2. La variabile viene aggiunta alla lista di monitoring
3. I valori si aggiornano automaticamente

**Per leggere/scrivere valori**:

1. Click destro sulla variabile
2. Seleziona **Read** (lettura) o **Write** (scrittura)
3. Inserisci il nuovo valore se in scrittura

### 8.4 Browse by NodeId

Per accedere direttamente a un NodeId:

1. Menu **Session** → **Browse NodeId**
2. Inserisci il NodeId completo:
   ```
   ns=3;s="gtyp_Interface_Dashboard.EnvironmentSensor.t"
   ```
3. Click **Browse**

---

## 9. Tipi di Dati OPC UA Utilizzati

| Tipo OPC UA | Tipo PLC (S7) | Descrizione | Esempio |
|-------------|---------------|-------------|---------|
| Boolean | BOOL | Valore booleano | `TRUE`, `FALSE` |
| Int16 | INT | Intero 16 bit | `-32768` ... `32767` |
| UInt16 | WORD | Intero senza segno 16 bit | `0` ... `65535` |
| Int32 | DINT | Intero 32 bit | `-2147483648` ... `2147483647` |
| Float | REAL | Virgola mobile | `123.45` |
| String | STRING | Stringa caratteri | `"Hello"` |
| DateTime | DATE_AND_TIME | Data e ora | `2026-02-11T14:30:00Z` |

---

## 11. Riferimenti

### 11.1 Documentazione

- **OPC UA Specification**: [opcfoundation.org](https://opcfoundation.org)
- **UA Expert**: [Unified Automation](https://www.unified-automation.com/products/development-tools/uaexpert.html)
- **Node-RED OPC UA**: [node-red-contrib-opcua-iiot](https://flows.nodered.org/node/node-red-contrib-opcua-iiot)

### 11.2 Repository GitHub

- Learning Factory: [fischertechnik/plc_training_factory_24v](https://github.com/fischertechnik/plc_training_factory_24v)

### 11.3 Configurazione di Rete

| Dispositivo | Indirizzo IP | Porta | Protocollo |
|-------------|--------------|-------|------------|
| PLC Siemens S7-1500 | 192.168.0.1 | 4840 | OPC UA |
| Gateway IoT (Raspberry Pi) | 192.168.0.5 | 1880 | HTTP (Node-RED UI) |
| TXT Controller | 192.168.0.10 | 1883 | MQTT |

---

**Documento redatto da**: Analisi flow Node-RED Learning Factory 4.0  
**Ultimo aggiornamento**: 11 Febbraio 2026  
**Versione documento**: 1.0
