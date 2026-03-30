
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

```
MQTT (f/c/order)
   ↓
Node-RED (prepare HBW order)
   ↓
OPC UA Write
   ↓
PLC Siemens
   ↓
OPC UA Read (stato)
   ↓
MQTT (f/o/order)
```

### 2.2 Principio fondamentale

⚠️ **Il PLC è l'unico responsabile dello stato dell'ordine**

Node-RED non forza mai lo stato, ma:

- scrive solo il comando di ordine,
- legge passivamente lo stato,
- lo ripubblica via MQTT.

Questo punto è stato essenziale per evitare crash della microfactory.

## 3. Topic MQTT utilizzati

### 3.1 Invio ordine (INPUT)

**Topic**

```
f/c/order
```

**Payload**

```json
{
  "type": "BLUE"
}
```

**Tipi supportati:**

- `BLUE`
- `RED`
- `WHITE`

📌 Il payload viene validato in Node-RED prima di essere inviato al PLC.

### 3.2 Stato ordine (OUTPUT)

**Topic**

```
f/o/order
```

**Payload tipico**

```json
{
  "ts": "2026-01-13T10:24:36.515Z",
  "state": "IN_PROCESS",
  "type": "WHITE"
}
```

## 4. Stati dell'ordine e transizioni

Gli stati non sono inventati, ma osservati direttamente dal comportamento reale del PLC.

**Stati osservati:**

- `WAITING_FOR_ORDER`
- `IN_PROCESS`
- `SHIPPED`

**Transizione tipica**

```
WAITING_FOR_ORDER
        ↓
   IN_PROCESS
        ↓
    SHIPPED
        ↓
WAITING_FOR_ORDER
```

**Esempio reale (log MQTT)**

```json
{
  "ts": "2026-01-13T10:24:36.515Z",
  "state": "IN_PROCESS",
  "type": "WHITE"
}
```

```json
{
  "ts": "2026-01-13T10:25:58.589Z",
  "state": "WAITING_FOR_ORDER",
  "type": ""
}
```

## 5. Comportamento con ordini sovrapposti

### Test effettuato

È stato testato l'invio di più ordini consecutivi mentre un ordine era già in corso.

### Comportamento osservato

- Il PLC ignora o ritarda i nuovi ordini
- Node-RED non va in errore
- La microfactory non crasha

### Raccomandazione ufficiale

⚠️ **SI SCONSIGLIA DI INVIARE PIÙ ORDINI CONTEMPORANEAMENTE**

Il sistema è progettato per:

- un ordine alla volta
- gestione sequenziale lato PLC

## 6. Cosa è stato evitato (fondamentale)

Per evitare crash e comportamenti anomali non è stato fatto quanto segue:

- ❌ Scrivere direttamente su `f/i/order`
- ❌ Forzare manualmente `IN_PROCESS` o `SHIPPED`
- ❌ Simulare lo stato dell'ordine via MQTT
- ❌ Duplicare o modificare i flow ufficiali
- ❌ Replicare la logica del PLC in Node-RED

👉 Tutti questi tentativi portavano a instabilità o blocchi della microfactory.

## 7. Flow Node-RED "ORDER – MQTT"

Il flow finale:

- intercetta `f/c/order`,
- prepara il comando OPC UA,
- scrive nel nodo PLC ufficiale:

```
gtyp_Interface_Dashboard.Publish.OrderWorkpieceButton
```

- lascia al PLC il controllo totale del ciclo ordine.

## 📎 Allegati

- Flow Node-RED esportato (`ORDER - MQTT.json`)
- Screenshot del flow funzionante
- Log MQTT del test completo

## 8. Test completo registrato

### Comando inviato

**Topic**

```
f/c/order
```

**Payload**

```json
{
  "type": "WHITE"
}
```

### Risultato osservato

- Dashboard aggiornata correttamente
- Stato pubblicato su `f/o/order`
- Ordine completato
- Ritorno a `WAITING_FOR_ORDER`

✅ **Test superato**