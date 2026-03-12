
## 9.1 Incapsulamento Logica PLC

### Cosa Espone il PLC via OPC UA

Il PLC **non espone direttamente** le uscite fisiche (motori, valvole, relè). Invece, espone un'**interfaccia logica semantica**.

#### ✅ Variabili ESPOSTE

| Tipo | Esempio | Descrizione |
|------|---------|-------------|
| **Setpoint posizione** | `di_Target_Position = 8000` | Posizione target assi (impulsi encoder) |
| **Stati semantici** | `s_description = "Moving to pickup"` | Descrizione testuale stato modulo |
| **Comandi testuali** | `s_cmd = "home"` | Comandi comprensibili (non bit) |
| **Feedback sensori** | `r_t = 22.5` | Temperatura già convertita in °C |
| **Codici stato** | `i_code = 20` | Codice numerico stato (0-100) |
| **Timestamp** | `ldt_ts = "2026-02-17T11:30:00Z"` | Data/ora eventi |

**Esempio concreto**:
```javascript
// Client scrive comando semantico
WRITE s_cmd = "home"

// PLC esegue internamente:
// - Controlla limiti meccanici
// - Calcola traiettoria sicura
// - Muove motori con rampe controllate
// - Aggiorna stato: s_description = "Homing in progress"
```

#### ❌ Variabili NON ESPOSTE

| Tipo | Esempio | Perché NON esposto |
|------|---------|---------------------|
| **Bit I/O diretti** | `%Q0.0 = TRUE` | Rischio attivazione motori senza safety check |
| **Comandi motore** | `M_Forward = TRUE` | Nessun controllo velocità/posizione |
| **Valvole** | `V_Vacuum_ON = TRUE` | Rischio attivazione senza pezzo presente |
| **PWM** | `PWM_DutyCycle = 5000` | Possibili velocità pericolose |
| **Registri interni** | `%M100.0`, `%DB10.DBW0` | Dettagli implementativi non rilevanti |

**Esempio pratico - Movimento asse HBW**:

```
❌ APPROCCIO BIT-LEVEL (NON POSSIBILE):
   WRITE %Q0.0 = TRUE        // Abilita motore
   WRITE %Q0.1 = FALSE       // Direzione forward
   WRITE %QW2 = 5000         // PWM al 50%
   → PERICOLOSO: Nessun controllo fine corsa, velocità, collisioni

✅ APPROCCIO SEMANTICO (IMPLEMENTATO):
   WRITE di_Target_Position = 8000
   → SICURO: PLC gestisce velocità, rampe, limiti, safety
```

### Vantaggi dell'Incapsulamento

1. **Sicurezza**: Impossibile comandare direttamente attuatori → prevenzione danni hardware
2. **Semplicità**: Client scrive 1 variabile invece di gestire 10 segnali I/O
3. **Manutenibilità**: Cambio sensore/motore → zero modifiche software client
4. **Industry 4.0**: Comunicazione semantica conforme standard CPS

**Esempio reale**:

```
SCENARIO: Ordinare pezzo rosso

Approccio bit-level (se fosse esposto):
  WRITE %MB100 = 0x02       // Tipo pezzo in memoria
  WRITE %MD200 = 5000       // Posizione magazzino
  WRITE %M10.1 = TRUE       // Start order flag
  WAIT %M10.2 = TRUE        // Wait complete flag
  → 30+ righe codice client, conoscenza indirizzi memoria PLC

Approccio semantico (implementato):
  WRITE OrderWorkpieceButton.s_type = "red"
  → 1 riga codice, interfaccia comprensibile
```

### Come Modificare Variabili Esposte

Le variabili esposte in OPC UA sono definite nel **programma PLC** (TIA Portal). Per modificare cosa viene esposto:

#### 1. Modifiche via TIA Portal (Programmazione PLC)

**Software necessario**: SIEMENS TIA Portal V17/V18

**Procedura**:

1. **Connessione al PLC**:
   - Apri TIA Portal
   - Connetti via Ethernet a `192.168.0.1`
   - Vai a `PLC_1` → `Program blocks` → `Data blocks`

2. **Modifica Data Block**:
   ```
   TIA Portal:
   └── PLC_1 [CPU 1512SP-1 PN]
       └── Program blocks
           └── Data blocks
               ├── gtyp_HBW (DB10)
               │   └── Horizontal_Axis
               │       ├── di_Actual_Position    ← VISIBILE in OPC UA
               │       ├── di_Target_Position    ← VISIBILE in OPC UA
               │       └── Internal_State        ← NON VISIBILE (no attributo)
               │
               └── gtyp_Interface_Dashboard (DB20)
                   └── Publish
                       └── OrderWorkpieceButton  ← VISIBILE in OPC UA
   ```

3. **Attributo OPC UA**:

   Ogni variabile ha un checkbox **"Accessible from HMI/OPC UA"**:

   | Variabile | Attributo | Risultato |
   |-----------|-----------|-----------|
   | `di_Target_Position` | ☑ Accessible | ✅ Visibile in OPC UA |
   | `Internal_Counter` | ☐ Not accessible | ❌ Nascosto |

4. **Download al PLC**:
   - Dopo modifiche: `Download to device` → `Hardware and software (all)`
   - PLC si riavvia (downtime ~30s)
   - Nuovo Address Space disponibile

#### 2. Aggiungere Nuove Variabili

**Esempio**: Aggiungere counter produzione al modulo HBW

**Step**:

1. Apri Data Block `gtyp_HBW` in TIA Portal
2. Aggiungi variabile:
   ```
   Name: di_Production_Counter
   Type: DInt
   Initial value: 0
   ☑ Accessible from HMI/OPC UA
   Comment: "Total pieces produced by HBW"
   ```

3. Logica PLC per incrementare:
   ```pascal
   // In Function Block FB_HBW
   IF Piece_Delivered = TRUE THEN
       gtyp_HBW.di_Production_Counter := gtyp_HBW.di_Production_Counter + 1;
   END_IF;
   ```

4. Download → Nuovo NodeId disponibile:
   ```
   ns=3;s="gtyp_HBW"."di_Production_Counter"
   ```

#### 3. Modificare Struttura Publish/Subscribe

**Esempio**: Aggiungere comando "pause" alla dashboard

**Step**:

1. Apri UDT (User Defined Type) `typ_Interface_Dashboard`

2. Espandi struttura `Publish`:
   ```
   typ_Interface_Dashboard
   └── Publish (Struct)
       ├── OrderWorkpieceButton (Struct)
       ├── PosPanTiltUnit (Struct)
       └── PauseProductionButton (Struct) ← NUOVO
           ├── x_pause (Bool)
           └── ldt_ts (DateTime)
   ```

3. Logica PLC:
   ```pascal
   IF gtyp_Interface_Dashboard.Publish.PauseProductionButton.x_pause = TRUE THEN
       // Metti tutti moduli in stato PAUSED
       State_HBW := 40; // PAUSED
       State_VGR := 40;
   END_IF;
   ```

4. Client può ora scrivere:
   ```javascript
   WRITE PauseProductionButton.x_pause = true
   ```

#### 4. Limitazioni e Sicurezza

**Cosa NON si può fare** (anche con TIA Portal):

❌ **Esporre direttamente I/O fisici** come variabili OPC UA sicure:
   - Le uscite `%Q` possono essere esposte, MA è **fortemente sconsigliato**
   - Motivo: Bypass di tutta la logica safety del PLC

✅ **Best practice**:
   - Crea sempre variabili intermedie in Data Block
   - Logica PLC converte variabile OPC UA → controllo I/O con safety
   - Esempio:
     ```pascal
     // ❌ MAI fare questo:
     %Q0.0 := OPC_UA_Command;  // Comando diretto motore

     // ✅ Sempre fare questo:
     IF OPC_UA_Target_Position > 0 AND Safety_OK = TRUE THEN
         %Q0.0 := TRUE;  // Enable motore
     ELSE
         %Q0.0 := FALSE;
     END_IF;
     ```

#### 5. Permessi Accesso

**Configurazione in TIA Portal** → `PLC` → `Protection & Security` → `Access levels`:

| Livello | Permessi | Cosa può fare via OPC UA |
|---------|----------|--------------------------|
| **Monitor** | Read only | Solo lettura variabili |
| **Operate** | Read + Write | Scrittura variabili Publish, lettura Subscribe |
| **Maintain** | Read + Write + Config | Modifica parametri (es: velocità max) |
| **Full access** | All | Modifica logica PLC (pericoloso!) |

**Configurazione OPC UA Server in TIA Portal**:
```
Server Interfaces → OPC UA
├── Enable OPC UA Server: ☑
├── Port: 4840
├── Security: 
│   ├── None (anonymous) ← Usato in Learning Factory
│   ├── Sign (username/password)
│   └── Sign & Encrypt
└── User Management:
    ├── User: "operator" → Operate level
    ├── User: "technician" → Maintain level
    └── User: "admin" → Full access
```

### Workflow Completo Modifica

```
┌─────────────────────────────────────────────────────────────┐
│ 1. RICHIESTA MODIFICA                                        │
│    Esempio: "Aggiungere comando stop di emergenza"          │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 2. PROGETTAZIONE                                             │
│    - Definire struttura dati (Bool? String?)                │
│    - Progettare logica PLC safety                           │
│    - Decidere Publish o Subscribe                           │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 3. IMPLEMENTAZIONE TIA PORTAL                                │
│    - Modificare Data Block gtyp_Interface_Dashboard          │
│    - Aggiungere variabile Emergency_Stop (Bool)             │
│    - ☑ Accessible from OPC UA                                │
│    - Implementare logica in FB_Main                         │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 4. TEST IN SIMULAZIONE                                       │
│    - PLCSIM Advanced (simulatore Siemens)                   │
│    - Verificare comportamento                                │
│    - Test safety (cosa succede se scrivi valori invalidi?)  │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 5. DOWNLOAD AL PLC REALE                                     │
│    - Backup programma esistente                              │
│    - Download to device                                      │
│    - PLC restart (~30s downtime)                             │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 6. VERIFICA OPC UA                                           │
│    - UA Expert: Refresh Address Space                       │
│    - Verificare nuovo NodeId presente                        │
│    - Test read/write                                         │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 7. AGGIORNAMENTO CLIENT (Node-RED)                           │
│    - Aggiungere nuovo nodo OPC UA Write                     │
│    - Configurare NodeId                                      │
│    - Test integrazione completa                              │
└─────────────────────────────────────────────────────────────┘
```

### Esempio Reale: Aggiunta Variabile Production Count

**Prima** (Address Space):
```
gtyp_HBW
└── Horizontal_Axis
    ├── di_Actual_Position
    ├── di_Target_Position
    └── x_Position_Reached
```

**Dopo modifica TIA Portal**:
```
gtyp_HBW
├── Horizontal_Axis
│   ├── di_Actual_Position
│   ├── di_Target_Position
│   └── x_Position_Reached
│
└── Statistics ← NUOVO
    ├── di_Total_Pieces (DInt)
    ├── di_Pieces_Today (DInt)
    └── ldt_Last_Reset (DateTime)
```

**Codice PLC aggiunto**:
```pascal
// In FB_HBW, dopo pezzo consegnato:
IF Piece_Delivered_Rising_Edge THEN
    gtyp_HBW.Statistics.di_Total_Pieces := 
        gtyp_HBW.Statistics.di_Total_Pieces + 1;
    gtyp_HBW.Statistics.di_Pieces_Today := 
        gtyp_HBW.Statistics.di_Pieces_Today + 1;
END_IF;

// Reset giornaliero (00:00)
IF HOUR(CURRENT_TIME) = 0 AND Daily_Reset_Done = FALSE THEN
    gtyp_HBW.Statistics.di_Pieces_Today := 0;
    gtyp_HBW.Statistics.ldt_Last_Reset := NOW();
    Daily_Reset_Done := TRUE;
ELSIF HOUR(CURRENT_TIME) > 0 THEN
    Daily_Reset_Done := FALSE;
END_IF;
```

**Utilizzo in Node-RED**:
```javascript
// Leggi statistiche
msg.addressSpaceItems = [
    {
        nodeId: 'ns=3;s="gtyp_HBW"."Statistics"."di_Total_Pieces"',
        datatypeName: 'Int32'
    },
    {
        nodeId: 'ns=3;s="gtyp_HBW"."Statistics"."di_Pieces_Today"',
        datatypeName: 'Int32'
    }
];

// Visualizza su dashboard:
// "Today: 247 pieces | Total: 15,832 pieces"
```

---

### Considerazioni Finali

**Chi può modificare le variabili esposte?**
- ✅ **Programmatore PLC** con TIA Portal e accesso fisico/VPN al PLC
- ❌ **Operatore dashboard** (può solo usare variabili esistenti)
- ❌ **Client OPC UA** (non può modificare Address Space, solo leggere/scrivere valori)

**Quando modificare?**
- ✅ Aggiungere nuove funzionalità dashboard
- ✅ Esporre nuovi sensori/attuatori
- ✅ Aggiungere statistiche/diagnostica
- ❌ MAI esporre variabili safety-critical senza protezione logica

**Documentazione obbligatoria**:
- Ogni modifica deve essere documentata in:
  - Comment delle variabili in TIA Portal
  - File esterno `OPC_UA_Interface_v2.0.xlsx`
  - Changelog PLC program

---
