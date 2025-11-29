# 03. Flussi Operativi della Microfactory

In questa sezione vengono modellati i **flussi logici e operativi** della Learning Factory 4.0, con particolare attenzione a:
- flusso produttivo end-to-end (material flow),
- interazione tra le stazioni meccatroniche e il PLC,
- flusso di comunicazione tra PLC, IoT Gateway, TXT Controller e Cloud,
- stati operativi globali del sistema (state machine).

L’obiettivo è fornire una vista **concettuale e architetturale** della microfactory, complementare alla descrizione puntuale dei singoli moduli hardware (sezione 02).

---

## 03.1 Flusso Produttivo End-to-End (Material Flow)

Questa flowchart rappresenta il percorso tipico di un workpiece dalla fase di ingresso fino all’uscita, attraversando tutte le principali stazioni.

```mermaid
flowchart LR
    classDef station fill:#e8f5e9,stroke:#2e7d32,stroke-width:1px,color:#1b5e20;
    classDef action fill:#e3f2fd,stroke:#1565c0,stroke-width:1px,color:#0d47a1;
    classDef decision fill:#fff3e0,stroke:#ef6c00,stroke-width:1px,color:#e65100;

    IN(["Ingresso materiale"]):::action
    HBW[["HBW\nMagazzino Verticale"]]:::station
    VGR[["VGR\nVacuum Gripper Robot"]]:::station
    MPO[["MPO\nStazione di Lavorazione"]]:::station
    SLD[["SLD\nSorting Line"]]:::station
    DPS[["DPS\nNFC Station"]]:::station
    OUT(["Uscita / Fine ciclo"]):::action

    IN --> HBW
    HBW --> VGR
    VGR --> MPO
    MPO --> VGR
    VGR --> SLD
    SLD --> DPS
    DPS --> OUT
```

Questa rappresentazione serve come **overview compatta** per spiegare il percorso standard del pezzo all’interno della fabbrica.

---

## 03.2 Sequence Diagram – Interazione PLC e Stazioni Meccatroniche

Di seguito un **sequence diagram** semplificato che mostra la sequenza di messaggi tra PLC e stazioni principali durante l’elaborazione di un singolo workpiece.

```mermaid
sequenceDiagram
    autonumber
    participant PLC as PLC S7-1500
    participant HBW as HBW
    participant VGR as VGR
    participant MPO as MPO
    participant SLD as SLD
    participant DPS as DPS

    PLC->>HBW: Richiesta contenitore per ordine
    HBW-->>PLC: Contenitore pronto in posizione VGR

    PLC->>VGR: Preleva da HBW
    VGR-->>PLC: PICK_DONE

    PLC->>VGR: Trasporta a MPO
    VGR-->>MPO: Deposita workpiece
    MPO-->>PLC: MPO_READY_FOR_PROCESS

    PLC->>MPO: Avvia lavorazione
    MPO-->>PLC: MPO_DONE

    PLC->>VGR: Trasporta a SLD
    VGR-->>SLD: Deposita workpiece
    SLD-->>PLC: SLD_COLOR_DETECTED

    PLC->>SLD: Smista in base al colore
    SLD-->>PLC: SLD_DONE

    PLC->>VGR: Trasporta contenitore verso DPS o Output
    VGR-->>DPS: Deposita workpiece
    DPS-->>PLC: DPS_ITEM_PRESENT
```

Questo diagramma è utile per spiegare la **coreografia di controllo** implementata nel PLC tramite i vari blocchi funzionali.

---

## 03.3 Sequence Diagram – Flusso di Comunicazione IoT (PLC ↔ Gateway ↔ TXT ↔ Cloud)

Qui viene rappresentato il flusso dei dati e dei comandi tra i livelli OT/IT.

```mermaid
sequenceDiagram
    autonumber
    participant PLC as PLC S7-1500
    participant GW as IoT Gateway (Node-RED)
    participant TXT as TXT Controller 4.0
    participant CLOUD as Cloud Fischertechnik

    note over PLC,TXT: Inizializzazione e sincronizzazione configurazioni
    PLC->>GW: Richiesta/lettura configurazioni (OPC-UA)
    GW-->>PLC: Invio posizioni/parametri da ConfigData.csv

    note over PLC,TXT: Gestione ordine di produzione
    CLOUD->>TXT: Nuovo ordine di produzione (MQTT)
    TXT->>GW: Messaggio di sincronizzazione (MQTT)
    GW->>PLC: Aggiornamento stato ordine (OPC-UA)

    note over PLC,CLOUD: Avanzamento produzione
    PLC->>GW: Stato lavorazione, errori, avanzamento
    GW->>TXT: Stato aggiornato (MQTT)
    TXT->>CLOUD: Update dashboard ordine
```

Questo livello di dettaglio è fondamentale per descrivere la natura **cyber-fisica** del sistema: il PLC controlla il processo, ma l’IoT Gateway e il TXT gestiscono comunicazione, configurazioni e cloud.

---

## 03.4 State Machine – Stati Globali della Microfactory

La seguente macchina a stati descrive gli **stati operativi globali** del sistema, così come coordinati dal PLC.

```mermaid
stateDiagram-v2
    [*] --> IDLE

    IDLE: Sistema fermo
    IDLE --> WAIT_FOR_ORDER: ordine disponibile dal cloud

    WAIT_FOR_ORDER --> RETRIEVE: avvio fase prelievo da HBW
    RETRIEVE --> PROCESS: pezzo trasferito a MPO
    PROCESS --> SORT: lavorazione completata
    SORT --> FINALIZE: smistamento + NFC update
    FINALIZE --> REPORT: invio stato produzione al cloud
    REPORT --> IDLE: nessun altro ordine
    REPORT --> WAIT_FOR_ORDER: nuovo ordine in coda

    RETRIEVE --> ERROR: timeout o sensori incoerenti
    PROCESS --> ERROR: errore MPO o VGR
    SORT --> ERROR: colore non riconosciuto o jam meccanico
    FINALIZE --> ERROR: scrittura NFC fallita

    ERROR: Gestione fault
    ERROR --> RECOVERY: reset logico/meccanico
    RECOVERY --> IDLE: sistema pronto
```

Questa vista è utile per descrivere il comportamento della **logica di supervisione** implementata nel PLC (tipicamente nel blocco `FB_SYSTEM` o equivalente).

---

## 03.5 State Machine – VGR (Esempio di Sotto-Macchina a Stati)

Come esempio di approfondimento, il VGR può essere modellato come una sotto-macchina a stati.

```mermaid
stateDiagram-v2
    [*] --> HOME

    HOME: Posizione di riferimento
    HOME --> MOVE_TO_PICK: comando di prelievo

    MOVE_TO_PICK: Movimento verso posizione di pick
    MOVE_TO_PICK --> PICK: posizione raggiunta

    PICK: Attivazione ventosa
    PICK --> LIFT: pezzo rilevato dal sensore vuoto
    PICK --> ERROR: mancata presa / timeout

    LIFT: Risalita asse Z
    LIFT --> MOVE_TO_DROP

    MOVE_TO_DROP: Spostamento verso posizione di deposito
    MOVE_TO_DROP --> DROP: destinazione raggiunta

    DROP: Rilascio workpiece
    DROP --> RETURN_HOME

    RETURN_HOME: Ritorno in posizione neutra
    RETURN_HOME --> HOME

    ERROR: stato di errore
    ERROR --> HOME: dopo reset/calibrazione
```

Questo schema può essere citato e riutilizzato nella descrizione del blocco funzionale `FB_VGR` e nella documentazione degli algoritmi di controllo.

---

## 03.6 Flowchart – Gestione degli Errori

Infine, una flowchart di alto livello per rappresentare la logica generale di gestione degli errori.

```mermaid
flowchart TD
    classDef state fill:#f3e5f5,stroke:#6a1b9a,stroke-width:1px,color:#4a148c;
    classDef action fill:#e3f2fd,stroke:#1565c0,stroke-width:1px,color:#0d47a1;
    classDef decision fill:#fff3e0,stroke:#ef6c00,stroke-width:1px,color:#e65100;

    START([Inizio ciclo PLC]):::action
    CHECK_ERR{Errore presente?}:::decision
    NORMAL["Esecuzione normale\n(RET\nRIEVE/PROCESS/SORT/FINALIZE)"]:::state
    HANDLE_ERR["Gestione errore\n(log, segnalazione, arresto sicuro)"]:::state
    RESET_REQ{Reset ricevuto?}:::decision
    RECOVER["Fase di recovery\n(calibrazione / homing)"]:::state
    STOP([Stop sistema]):::action

    START --> CHECK_ERR
    CHECK_ERR --> NORMAL:::state
    CHECK_ERR --> HANDLE_ERR

    NORMAL --> CHECK_ERR

    HANDLE_ERR --> RESET_REQ
    RESET_REQ --> RECOVER
    RESET_REQ --> STOP

    RECOVER --> CHECK_ERR
```

Questa parte è particolarmente utile per spiegare, **come il sistema reagisce a condizioni di malfunzionamento**.

---


