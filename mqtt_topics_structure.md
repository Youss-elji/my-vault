# Struttura dei topic MQTT della microfactory

## 1. Mappatura generale dei topic

Dall'analisi dei log MQTT e dei flow Node-RED ufficiali, la microfactory utilizza una struttura gerarchica coerente, basata su:

| Prefisso | Significato                                  |
| -------- | -------------------------------------------- |
| `f/`     | factory (livello logico della microfabbrica) |
| `i/`     | input verso Node-RED / PLC                   |
| `o/`     | output generato dal PLC / Node-RED           |
| `c/`     | comandi applicativi (client → factory)       |

### Convenzione principale

- `f/i/...` → comandi o dati in ingresso alla factory
- `f/o/...` → stato / feedback ufficiale della factory
- `i/...` → telemetria e dati live
- `o/...` → comandi hardware eseguiti (PTU, NFC, ecc.)

👉 Questa separazione è fondamentale: scrivere sul topic sbagliato può causare instabilità o crash.

## 2. Topic principali osservati (con esempi reali)

### 2.1 Ordini di produzione

#### Invio ordine (comando)

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

**Valori supportati osservati:**
- `BLUE`
- `RED`
- `WHITE`

#### Stato ordine (feedback ufficiale)

**Topic**
```
f/o/order
```

**Payload reali osservati**
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

**Stati osservati**
- `WAITING_FOR_ORDER`
- `IN_PROCESS`
- `SHIPPED`

⚠️ **Importante:** lo stato non va mai scritto manualmente.

## 3. Controllo telecamera – PTU (Pan Tilt Unit)

### 3.1 Comandi PTU (movimento relativo)

#### Endpoint reale dei comandi

**Topic**
```
o/ptu
```

👉 Questo è l'endpoint giusto, confermato dai log quando si usa la dashboard.

**Payload supportati (osservati)**
```json
{
  "cmd": "relmove_left",
  "degree": 10
}
```

```json
{
  "cmd": "relmove_right",
  "degree": 10
}
```

```json
{
  "cmd": "relmove_up",
  "degree": 10
}
```

```json
{
  "cmd": "relmove_down",
  "degree": 10
}
```

```json
{
  "cmd": "home"
}
```

```json
{
  "cmd": "stop"
}
```

✔ Questi comandi funzionano sempre, anche senza dashboard.

### 3.2 Feedback posizione PTU (telemetria)

**Topic**
```
i/ptu/pos
```

**Payload reale**
```json
{
  "pan": 0.9838532209396362,
  "tilt": -0.3706666827201843,
  "ts": "2026-01-13T13:41:41.619Z"
}
```

**Osservazioni reali:**
- il tilt cambia visibilmente durante i comandi
- il pan può rimanere stabile se il movimento è solo verticale
- aggiornamento ~1 Hz

## 4. Preset SSC (CENTER / HBW)

### 4.1 Endpoint preset SSC

**Topic**
```
f/i/ssc/cmd
```

**Payload**
```json
{
  "preset": "HBW"
}
```

Oppure:
```json
{
  "preset": "CENTER"
}
```

### ⚠️ Condizione fondamentale

Il preset funziona solo se:
- In dashboard è attivo **"Activate pos. move"**
- Le posizioni sono caricate da ConfigData → PLC
- Dopo l'uso, Active pos. move va disattivato

📌 **Se rimane attivo:**
- blocca tutti i comandi PTU manuali
- la telecamera ignora `relmove_*`

## 5. Acquisizione dati camera "live"

### 5.1 Stream immagine

**Topic**
```
i/cam
```

**Payload reale (estratto)**
```json
{
  "ts": "2026-01-13T13:41:41.583Z",
  "data": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD..."
}
```

- ✔ JPEG in base64
- ✔ Usato dalla dashboard per lo stream video
- ⚠️ Payload molto pesante → impatto su CPU e rete

### 5.2 Stato camera (flag)

**Topic**
```
i/cam
```

**Payload**
```json
{
  "on": true,
  "fps": 2,
  "ts": "2026-01-13T12:27:21.297Z"
}
```

### 5.3 Sensori ambientali

**Topic osservati**
- `i/ldr`
- `i/bme680`

**Payload tipico**
```json
{
  "t": 27.8,
  "h": 22.6,
  "p": 1020.4,
  "iaq": 169,
  "ts": "2026-01-13T13:41:28.354Z"
}
```

## 6. NFC – ruolo e topic

### Comando lettura NFC

**Topic**
```
o/nfc
```

**Payload**
```json
{
  "cmd": "read_uid"
}
```

### Feedback NFC

**Topic**
```
i/nfc
```

**Payload reale**
```json
{
  "workpiece": {
    "id": "047984ca341290",
    "type": "BLUE",
    "state": "PROCESSED"
  }
}
```

👉 L'NFC è centrale per:
- identificare il pezzo
- tracciare la storia (history)
- collegare ordine → oggetto fisico

## 7. Tabella riassuntiva finale

| Topic | Direzione | Contenuto | Esempio |
|-------|-----------|-----------|---------|
| `f/c/order` | input | invio ordine | `{ "type":"BLUE" }` |
| `f/o/order` | output | stato ordine | `IN_PROCESS` |
| `o/ptu` | output | comando camera | `relmove_down` |
| `i/ptu/pos` | input | pan/tilt | `{pan,tilt}` |
| `f/i/ssc/cmd` | input | preset camera | `{preset:"HBW"}` |
| `i/cam` | input | stream / stato | base64 / fps |
| `o/nfc` | output | comando NFC | `read_uid` |
| `i/nfc` | input | dati pezzo | id, type |

## 8. Considerazioni finali (importanti)

- La microfactory non è **MQTT-first**, ma **PLC-first**
- MQTT è una vista e un canale di controllo, non il core
- Usare solo topic realmente osservati

### Evitare:
- ❌ scritture su `state`
- ❌ duplicazione dei flow ufficiali
- ❌ preset SSC lasciati attivi