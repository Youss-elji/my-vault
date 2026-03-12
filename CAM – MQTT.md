
# Controllo telecamera (PTU + SSC) via MQTT

## 1. Obiettivo

Consentire il controllo completo della telecamera della microfactory tramite MQTT, senza usare direttamente la dashboard HMI, riutilizzando i flow ufficiali di Node-RED e senza riscrivere la logica del PLC.

Il controllo è diviso in due funzionalità distinte:
1. **Comandi PTU** → movimento relativo (gradi)
2. **Preset SSC** → posizioni preconfigurate (CENTER / HBW)

## 2. Architettura generale

### 2.1 Flusso PTU (movimento manuale)

```
MQTT (f/i/cam/cmd)
   ↓
Parse PTU Command (Function)
   ↓
Forward PTU cmd
   ↓
MQTT (o/ptu)
   ↓
PTU (telecamera)
   ↓
Feedback posizione
   ↓
MQTT (i/ptu/pos)
```

### 2.2 Flusso SSC (preset)

```
MQTT (f/i/ssc/cmd)
   ↓
Parse SSC preset
   ↓
Select SSC Position
   ↓
OPC UA Write (setpoint + start)
   ↓
PLC
   ↓
Movimento automatico telecamera
```

## 3. PTU – Comandi manuali (relativi)

### 3.1 Endpoint di comando

**Topic (INPUT):**
```
f/i/cam/cmd
```

I comandi vengono inoltrati internamente verso:
```
o/ptu
```

⚠️ `o/ptu` è l'endpoint reale usato dal sistema. `f/i/cam/cmd` è solo l'interfaccia MQTT "pulita" che abbiamo creato.

### 3.2 Payload supportati

**Movimento relativo (gradi)**
```json
{ "cmd": "relmove_left",  "degree": 10 }
{ "cmd": "relmove_right", "degree": 10 }
{ "cmd": "relmove_up",    "degree": 10 }
{ "cmd": "relmove_down",  "degree": 10 }
```

📌 `degree` indica l'angolo di spostamento relativo, non una posizione assoluta.

**Comandi speciali**
```json
{ "cmd": "home" }
{ "cmd": "stop" }
```

- `home` → posizione di riferimento PTU
- `stop` → ferma immediatamente il movimento

## 4. PTU – Feedback posizione (lettura)

### 4.1 Endpoint di feedback

**Topic (OUTPUT):**
```
i/ptu/pos
```

### 4.2 Payload

```json
{
  "pan": 0.14935779571533203,
  "tilt": -0.0633333253860474,
  "ts": "2026-01-22T11:53:28.110Z"
}
```

### 4.3 Comportamento osservato (reale)

- `tilt` cambia visibilmente con i comandi
- `pan` spesso resta costante (limiti meccanici o logica PTU)
- il feedback è continuo (stream di stato)

✔ Questo conferma che:
- i comandi PTU funzionano
- il feedback è affidabile
- MQTT è read/write corretto

## 5. SSC – Preset di posizione (CENTER / HBW)

Questa parte è diversa dai comandi PTU.

### 5.1 Cosa sono i preset SSC

- Sono posizioni assolute
- Sono definite nel ConfigData del PLC
- Vengono caricate con Load config data to PLC
- Sono gestite dal flow ufficiale HMI – SSC Positions

### 5.2 Prerequisito fondamentale (OBBLIGATORIO)

⚠️ **Prima di usare i preset via MQTT:**
1. Aprire HMI – SSC Positions
2. Attivare **Activate pos. move**
3. Selezionare almeno una volta il preset (HBW / Center)

**Senza questo:**
- il PLC ignora i comandi SSC
- sembra "non funzionare" ma in realtà è disabilitato

### 5.3 Endpoint MQTT preset

**Topic (INPUT):**
```
f/i/ssc/cmd
```

### 5.4 Payload

```json
{ "preset": "HBW" }
```

oppure

```json
{ "preset": "CENTER" }
```

### 5.5 Effetto reale

- La telecamera si muove automaticamente verso la posizione salvata
- I setpoint vengono presi dal ConfigData del PLC
- Il movimento è bloccante

## 6. Limitazione critica (IMPORTANTE)

🚨 **Quando Activate pos. move = ON:**
- ❌ tutti i comandi PTU manuali vengono ignorati
- ❌ non puoi muovere la camera con gradi
- ❌ sembra che MQTT "non funzioni"

👉 È comportamento corretto del PLC, non un bug.

### Soluzione corretta

Dopo aver raggiunto il preset:
1. Disattivare **Activate pos. move**
2. Tornare a usare i comandi PTU

## 7. Endpoint riassuntivi

| Funzione | Topic |
|----------|-------|
| Comandi PTU | `f/i/cam/cmd` |
| Endpoint reale PTU | `o/ptu` |
| Feedback posizione | `i/ptu/pos` |
| Preset SSC | `f/i/ssc/cmd` |

## 8. Cosa NON fare (per evitare blocchi)

- ❌ Scrivere direttamente sui nodi OPC UA SSC
- ❌ Forzare setpoint SSC senza `Activate pos. move`
- ❌ Mescolare PTU e SSC contemporaneamente
- ❌ Riscrivere la logica ufficiale HMI

## 9. Stato attuale del lavoro

- ✔ Comandi PTU via MQTT funzionanti
- ✔ Feedback pan/tilt in tempo reale
- ✔ Preset SSC funzionanti se abilitati da HMI
- ✔ Architettura stabile
- ✔ Nessun crash della microfactory