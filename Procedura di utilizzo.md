# Documentazione Tecnica  
## Accesso alla rete OT e Lettura Topic MQTT della FischerTechnik Learning Factory 4.0

---

## 1. Obiettivo dell’attività
Lo scopo dell’attività è:

- Accedere alla rete locale della microfactory FischerTechnik Learning Factory 4.0  
- Stabilire connessione con PLC, Raspberry Pi e TXT Controller  
- Accedere al flusso dati MQTT generato dalla fabbrica  
- Leggere in tempo reale i topic relativi a sensori, attuatori, camera e inventario  

Questa procedura permette l’integrazione della fabbrica in sistemi IoT esterni.

---

## 2. Architettura della rete

Ogni dispositivo nella rete utilizza un indirizzo IP statico:

| Componente                 | IP                | Funzione                         |
| -------------------------- | ----------------- | -------------------------------- |
| PLC Siemens S7-1500        | **192.168.0.1**   | Controllo fisico degli attuatori |
| Raspberry Pi (IoT Gateway) | **192.168.0.5**   | Node-RED, OPC-UA Bridge          |
| TXT 4.0 Controller         | **192.168.0.10**  | Broker MQTT + Camera + Cloud     |
| TP-Link Router             | **192.168.0.252** | DHCP e rete interna              |
| PC Utente                  | 192.168.0.x       | Client IoT / HMI                 |

Il TXT Controller funge da **broker MQTT locale** su porta **1883**.

---

## 3. Accesso alla rete OT

### 3.1 Collegamento Wi-Fi
SSID: **FischerTechnik Network**  
Password: **98066117**

Dopo la connessione:  

`IPv4: 192.168.0.11 `
`Gateway: 192.168.0.252`

Il PC risulta quindi nella subnet corretta per comunicare con PLC, Raspberry e TXT Controller.

---

## 4. Verifica connettività dispositivi
Per confermare che il PC sia correttamente integrato nella rete della microfactory,
è necessario verificare la raggiungibilità dei tre componenti principali tramite ping:

`ping 192.168.0.1 → PLC S7-1500`
`ping 192.168.0.5 → Raspberry / Node-RED`
`ping 192.168.0.10 → TXT Controller (MQTT Broker)`

Una risposta regolare e con tempi di latenza ridotti (1–10 ms) indica che:

- la rete è configurata correttamente  
- tutti i nodi sono attivi  
- si può procedere alla lettura dei topic MQTT

---

## 5. Accesso a Node-RED

Node-RED è il componente logico della microfactory che gestisce:
- Lettura dati via OPC-UA dal PLC  
- Pubblicazione su MQTT verso il TXT Controller  
- Interfaccia HMI della fabbrica  

### Accesso editor:
`http://192.168.0.5:1880`

### Accesso alla dashboard HMI:
`http://192.168.0.5:1880/ui`

I tab *MQTT-pub* e *MQTT-conf* mostrano quali topic vengono generati e verso quale broker vengono inviati.


---

## 6. Configurazione broker MQTT

Il broker MQTT locale della Learning Factory è ospitato sul TXT 4.0 Controller.

| Parametro | Valore           |
| --------- | ---------------- |
| Host      | **192.168.0.10** |
| Porta     | **1883**         |
| Username  | *(vuoto)*        |
| Password  | *(vuoto)*        |
| TLS/SSL   | disattivato      |
Node-RED è già configurato per pubblicare verso questo broker, e i messaggi generati dai sensori e dalle stazioni vengono inviati qui..

---

## 7. Lettura topic con mosquitto_sub

Per visualizzare in tempo reale tutti i messaggi pubblicati sul broker MQTT della microfactory,
si utilizza il comando:

`mosquitto_sub -h 192.168.0.10 -p 1883 -t "#" -v`
### Esempi di dati rilevati:

#### **Frame Camera** (topic `c/cam`)
Il TXT 4.0 pubblica i frame JPEG della telecamera in formato Base64:

```json
{
 "ts": "2025-12-09T12:49:41.862Z",
 "data": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAA..."
}
```

#### **Sensore BME680** (topic `c/bme680`):

```json
{
 "ts":"2025-12-09T12:49:34.202Z",
 "t":26.8,
 "rh":30.5,
 "p0":1022.3,
 "iaq":47,
 "gr":0
}

```

#### **LDR** (topic `c/ldr`):
```json
{
 "ts":"2025-12-09T12:49:01.741Z",
 "ldr":946
}
```


---

## 8. Lettura con MQTT Explorer

MQTT Explorer fornisce un'interfaccia grafica per analizzare i topic pubblicati.

### Configurazione della connessione:

| Campo | Valore |
|-------|--------|
| Protocol | mqtt:// |
| Host | **192.168.0.10** |
| Port | **1883** |
| TLS | Off |
| Username | *(vuoto)* |
| Password | *(vuoto)* |

Una volta connesso, MQTT Explorer mostra i topic organizzati gerarchicamente, consentendo:

- ispezione del payload JSON  
- monitoraggio dei cambiamenti in tempo reale  
- analisi strutturata dello stato della fabbrica  

---

## 9. Mappa e significato dei topic

### 9.1 Sensori ambiente
| Topic | Descrizione |
|-------|-------------|
| `c/bme680` | Dati ambientali: temperatura, umidità, pressione, qualità aria |
| `c/ldr` | Sensore luminosità |
| `c/cam` | Frame della telecamera (JPEG Base64) |

---

### 9.2 Stato dei moduli (PLC → MQTT via Node-RED)
I topic sotto `v/state` rappresentano lo stato operativo dei vari moduli della microfactory.

| Topic           | Modulo                |
| --------------- | --------------------- |
| `f/i/state/vgr` | Vacuum Gripper        |
| `f/i/state/mpo` | Multi Processing Oven |
| `f/i/state/sld` | Sorting Line          |
| `f/i/state/hbw` | Warehouse             |
| `f/i/state/dsi` | Input Station         |
| `f/i/state/dso` | Output Station        |

Ogni messaggio contiene tipicamente:
- timestamp (`ts`)  
- codice stato (`code`)  
- descrizione testuale (`description`)  

---

## 10. Inventario Magazzino (HBW)

Il topic `f/i/stock` contiene informazioni sul contenuto dell’High-Bay Warehouse (HBW).
Ogni messaggio rappresenta un singolo workpiece rilevato nel magazzino.

### Esempio di payload:

```json
{
"workpiece": {
"id": "044078ca341291",
"state": "RAW",
"type": "RED"
},
"location": "A1"
}
```
#### Significato dei campi:
- **id** → identificatore unico del workpiece (UID NFC)
- **state** → stato del pezzo (RAW, PROCESSED, ERROR…)
- **type** → colore/materiale (WHITE, BLUE, RED)
- **location** → posizione nella griglia del magazzino (es. A1, B3…)

Questo topic fornisce una vista in tempo reale dell’inventario della factory

---

## 11. Topic NFC

La microfactory utilizza tag NFC per tracciare ogni workpiece.
I comandi e le risposte del lettore NFC vengono pubblicati sul topic:

`fl/o/nfc`

### Esempio di messaggio:

```json
{"ts":"2025-12-09T12:49:54.288Z","cmd":"read_uid"}
```

Il campo **cmd** rappresenta l'operazione in corso:

- `read_uid` → richiesta di lettura UID
- `read_all` → lettura completa della memoria
- `write` → scrittura dati sul tag
- `delete` → reset memoria del tag

Questi topic sono fondamentali per la tracciabilità digitale del pezzo (Digital Twin).

---

## 12. Considerazioni finali

La procedura eseguita permette di:

- Accedere completamente alla rete OT della microfactory  
- Verificare la raggiungibilità dei nodi principali (PLC, Raspberry, TXT)  
- Interfacciarsi con Node-RED per monitoraggio e diagnostica  
- Collegarsi al broker MQTT locale del TXT Controller  
- Leggere in tempo reale topic relativi a:  
  - sensori ambientali  
  - camera  
  - stati operativi dei moduli  
  - inventario del magazzino HBW  
  - comunicazioni NFC  

L’intero processo costituisce la base per successivi sviluppi:

- controllo remoto della fabbrica via MQTT  
- integrazione in sistemi IoT custom (Java, Python, Node-RED esterni)  
- archiviazione dati, analisi e dashboard personalizzate  
- implementazione di logiche di controllo avanzate e automazioni

La documentazione raccolta è quindi un riferimento completo per comprendere
il flusso dati IoT della FischerTechnik Learning Factory 4.0.
