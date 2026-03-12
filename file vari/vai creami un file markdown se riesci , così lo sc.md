<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# vai creami un file markdown se riesci , così lo scarico

Ho raggiunto il limite di chiamate, ma ho raccolto abbastanza informazioni per creare il documento Markdown. Ecco il file completo:

***

# Documentazione Variabili OPC UA - Learning Factory 4.0 24V

**Data**: 11 Febbraio 2026
**Sistema**: Gateway IoT Node-RED - PLC Siemens S7-1500
**Endpoint OPC UA**: `opc.tcp://192.168.0.1:4840`

***

## 1. Struttura Address Space OPC UA

Il PLC espone i dati attraverso **7 moduli principali** nel **Namespace 3**:


| NodeId OPC UA | Modulo | Descrizione |
| :-- | :-- | :-- |
| `ns=3;s="gtyp_HBW"` | High-Bay Warehouse | Magazzino verticale automatizzato |
| `ns=3;s="gtyp_VGR"` | Vacuum Gripper Robot | Robot con ventosa a vuoto |
| `ns=3;s="gtyp_SSC"` | Sorting Station with Camera | Stazione smistamento con camera Pan-Tilt |
| `ns=3;s="gtyp_SLD"` | Multi-Processing Station | Stazione lavorazione multi-processo |
| `ns=3;s="gtyp_Setup"` | Setup \& Calibration | Configurazione e calibrazione sensori |
| `ns=3;s="gtyp_Interface_Dashboard"` | Dashboard Interface | Interfaccia dati sensori TXT (BME680, LDR, Camera) |
| `ns=3;s="gtyp_Interface_TXT_Controler"` | TXT Controller Interface | Interfaccia controller TXT |


***

## 2. Sensori TXT Controller (Dashboard Interface)

### 2.1 Sensore Ambientale BME680

**NodeId Base**: `ns=3;s="gtyp_Interface_Dashboard.EnvironmentSensor"`


| Variabile | Tipo | Topic MQTT | Descrizione |
| :-- | :-- | :-- | :-- |
| `.t` | REAL | `i/bme680` | Temperatura (°C) |
| `.h` | REAL | `i/bme680` | Umidità relativa (% r.H.) |
| `.p` | REAL | `i/bme680` | Pressione atmosferica (hPa) |
| `.iaq` | INT | `i/bme680` | Indice qualità aria (0-500) |
| `.aq` | INT | `i/bme680` | Accuratezza qualità aria (0-3) |
| `.ts` | STRING | `i/bme680` | Timestamp ISO8601 |

**Configurazione**: `c/bme680` - periodo di campionamento (default: 3 secondi)

### 2.2 Fotoresistenza LDR

**NodeId Base**: `ns=3;s="gtyp_Interface_Dashboard.BrightnessSensor"`


| Variabile | Tipo | Topic MQTT | Descrizione |
| :-- | :-- | :-- | :-- |
| `.br` | REAL | `i/ldr` | Luminosità (%) |
| `.ts` | STRING | `i/ldr` | Timestamp ISO8601 |

**Configurazione**: `c/ldr` - periodo di campionamento (default: 3 secondi)

### 2.3 Camera USB

**NodeId Base**: `ns=3;s="gtyp_Interface_Dashboard.CameraPicture"`


| Variabile | Tipo | Topic MQTT | Descrizione |
| :-- | :-- | :-- | :-- |
| `.s_data` | STRING | `i/cam` | Immagine base64 encoded |
| `.ldt_ts` | STRING | `i/cam` | Timestamp acquisizione |

**Configurazione**: `c/cam` - `{"on": true/false, "fps": 1-10}`

***

## 3. Moduli Fabbrica

### 3.1 High-Bay Warehouse (HBW)

**NodeId Base**: `ns=3;s="gtyp_HBW"`

#### Comandi

| Variabile | Tipo | MQTT | Descrizione |
| :-- | :-- | :-- | :-- |
| Posizioni Rack | ARRAY | `f/i/hbw/cmd` | Coordinate X, Y, Z scaffalatura |
| Posizioni Belt | INT | `f/i/hbw/cmd1` | Posizione nastro trasportatore |
| Stato | STRUCT | `f/i/state/hbw` | Stato operativo HBW |

### 3.2 Vacuum Gripper Robot (VGR)

**NodeId Base**: `ns=3;s="gtyp_VGR"`

#### Posizioni Predefinite

| Variabile | Descrizione |
| :-- | :-- |
| Position HBW | Interfaccia con magazzino |
| Position DSI | Stazione ingresso |
| Position DSO | Stazione uscita |
| Position Color Reader | Sensore colore |
| Position MPO | Multi-Processing Station |
| Position NFC | Lettore RFID/NFC |
| Position SLD | Sorting Line |
| Position NiO | NFC I/O Station |

### 3.3 Sorting Station Camera (SSC)

**NodeId Base**: `ns=3;s="gtyp_SSC"`


| Variabile | Tipo | MQTT | Descrizione |
| :-- | :-- | :-- | :-- |
| Pan-Tilt Control | STRUCT | `f/i/ssc/cmd` | Controllo camera PTU |
| Position Centre | INT | - | Posizione centrale |
| Position HBW | INT | - | Vista magazzino |

### 3.4 Sorting Line (SLD)

**NodeId Base**: `ns=3;s="gtyp_SLD"`


| Variabile | Descrizione |
| :-- | :-- |
| Push Out Counter | Contatore espulsioni pezzi |
| Calibration Data | Dati calibrazione sensori colore |


***

## 4. Setup e Calibrazione

**NodeId Base**: `ns=3;s="gtyp_Setup"`

### 4.1 Calibrazione Sensori Colore

| Variabile | Descrizione |
| :-- | :-- |
| `x_Color_Sensor_Calibration` | Dati calibrazione sensore DSI |
| `x_Color_Sensor_SLD_Calibration` | Dati calibrazione sensore SLD |
| Color Sensor DSI - act. values | Valori RGB attuali DSI |
| Color Sensor SLD - act. values | Valori RGB attuali SLD |

### 4.2 Sincronizzazione Temporale

| Comando | Descrizione |
| :-- | :-- |
| Time Sync | Sincronizzazione NTP con PLC |


***

## 5. Topic MQTT - Mappatura Completa

### 5.1 Informazioni dai Sensori (i/)

```
i/bme680       → Sensore ambientale BME680
i/ldr          → Fotoresistenza
i/cam          → Immagine camera
i/ptu/pos      → Posizione Pan-Tilt Unit
i/alert        → Messaggi di alert
```


### 5.2 Comandi di Configurazione (c/)

```
c/bme680       → Config periodo sensore ambientale
c/ldr          → Config periodo fotoresistenza  
c/cam          → Config camera (on/off, fps)
```


### 5.3 Comandi Fabbrica (f/i/)

```
f/i/hbw/cmd    → Comandi High-Bay Warehouse
f/i/hbw/cmd1   → Comandi secondari HBW
f/i/ssc/cmd    → Comandi Sorting Station Camera
f/i/cam/cmd    → Comandi camera PTU
f/i/nfc/ds     → Dati NFC tag
```


### 5.4 Stati Fabbrica (f/i/state/)

```
f/i/state/hbw  → Stato High-Bay Warehouse
f/i/state/dsi  → Stato stazione ingresso
f/i/state/dso  → Stato stazione uscita
f/i/state/mpo  → Stato Multi-Processing Station
f/i/state/vgr  → Stato Vacuum Gripper Robot
```


### 5.5 Ordini Produzione (f/o/)

```
f/o/order      → Invio ordini (WHITE, RED, BLUE)
f/c/order      → Conferma ordini
```


***

## 6. Convenzioni di Indirizzamento

### 6.1 Formato NodeId

```
ns=<namespace>;s="<identifier>"
```

**Esempio**:

```
ns=3;s="gtyp_Interface_Dashboard.EnvironmentSensor.t"
```


### 6.2 Notazione Gerarchica

Le variabili utilizzano la **notazione punto** per indicare la gerarchia:

```
gtyp_<Modulo>.<Sottomodulo>.<Variabile>
```


### 6.3 Accesso da UA Expert

Nell'Address Space di **UA Expert**, naviga a:

```
Objects
  └─ <ns=3>
      ├─ gtyp_HBW
      ├─ gtyp_VGR
      ├─ gtyp_SSC
      ├─ gtyp_SLD
      ├─ gtyp_Setup
      ├─ gtyp_Interface_Dashboard
      │   ├─ EnvironmentSensor
      │   ├─ BrightnessSensor
      │   └─ CameraPicture
      └─ gtyp_Interface_TXT_Controler
```


***

## 7. Note Tecniche

### 7.1 Configurazione Rete

- **PLC Siemens**: `192.168.0.1:4840`
- **Gateway IoT (Raspberry Pi)**: `192.168.0.5`
- **TXT Controller**: `192.168.0.10:1883` (MQTT Broker)


### 7.2 Protocolli

- **PLC ↔ Gateway IoT**: OPC UA
- **Gateway IoT ↔ TXT Controller**: MQTT
- **TXT ↔ Cloud fischertechnik**: MQTT


### 7.3 Timestamp Format

Tutti i timestamp usano **ISO 8601 UTC**:

```javascript
"ts": "2026-02-11T13:24:00.000Z"
```


***

## Riferimenti

- Repository GitHub: [fischertechnik/plc_training_factory_24v](https://github.com/fischertechnik/plc_training_factory_24v)
- MQTT Interface Doc: [MqttInterface.md](https://github.com/fischertechnik/txt_training_factory/blob/master/TxtSmartFactoryLib/doc/MqttInterface.md)
- Dashboard Node-RED: `http://192.168.0.5:1880/ui`

***

**Documento generato da**: Analisi flow Node-RED `flows-6.json`
**Versione**: 1.0

Puoi copiare questo contenuto e salvarlo come file `.md` sul tuo computer!

