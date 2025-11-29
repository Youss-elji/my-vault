# 05 – Logica di Controllo

## 1. Struttura generale
La logica di controllo coordina tutte le stazioni della microfactory attraverso il PLC Siemens S7‑1500. Il comportamento del sistema è suddiviso in fasi operative con transizioni basate su segnali, sensori e conferme.

## 2. Ciclo principale
Il PLC esegue un ciclo deterministico che comprende:
- lettura ingressi;
- esecuzione dei blocchi funzionali;
- aggiornamento stati;
- scrittura uscite.

```mermaid
flowchart TD
    START([Inizio scan])
    READ[Acquisizione ingressi]
    EXEC[Logica PLC
    FB_VGR / FB_HBW / FB_MPO /
    FB_SLD / FB_DPS / FB_SYSTEM]
    WRITE[Scrittura uscite]
    END([Fine scan])

    START --> READ --> EXEC --> WRITE --> END --> START
```

## 3. Coordinamento dei moduli
La supervisione globale è gestita dal blocco principale (FB_SYSTEM). Ogni stazione ha un proprio blocco dedicato che gestisce: condizioni di esecuzione, timeout, errori e conferme.

| Blocco | Stazione | Funzioni principali |
|--------|----------|---------------------|
| FB_VGR | VGR | movimenti assi, presa/rilascio, sicurezza finecorsa |
| FB_HBW | HBW | posizionamento verticale/orizzontale, gestione slot |
| FB_MPO | MPO | controllo piano, lavorazione termica, timer |
| FB_SLD | SLD | rilevamento colore, smistamento |
| FB_DPS | DPS | presenza pezzo, sincronizzazione NFC |
| FB_SYSTEM | Sistema | sequenze globali, stati macchina, errori |

## 4. Sequenza operativa
La produzione segue una sequenza fissa gestita dal PLC.

```mermaid
sequenceDiagram
    autonumber
    participant PLC
    participant HBW
    participant VGR
    participant MPO
    participant SLD
    participant DPS

    PLC->>HBW: Posizionamento contenitore
    HBW-->>PLC: Pronto

    PLC->>VGR: Prelievo
    VGR-->>PLC: Pick_OK

    PLC->>VGR: Trasferimento a MPO
    VGR-->>MPO: Deposito
    MPO-->>PLC: Ready

    PLC->>MPO: Avvio processo
    MPO-->>PLC: Fine lavorazione

    PLC->>VGR: Trasferimento a SLD
    VGR-->>SLD: Deposito
    SLD-->>PLC: Colore

    PLC->>SLD: Smistamento
    SLD-->>PLC: Done

    PLC->>VGR: Verso DPS
    VGR-->>DPS: Deposito
    DPS-->>PLC: Presenza
```

## 5. Stati del sistema
```mermaid
stateDiagram-v2
    [*] --> IDLE
    IDLE --> WAIT_ORDER
    WAIT_ORDER --> RETRIEVE
    RETRIEVE --> PROCESS
    PROCESS --> SORT
    SORT --> FINALIZE
    FINALIZE --> REPORT
    REPORT --> IDLE

    RETRIEVE --> ERROR
    PROCESS --> ERROR
    SORT --> ERROR
    FINALIZE --> ERROR

    ERROR --> RECOVERY
    RECOVERY --> IDLE
```

## 6. Gestione errori
Il PLC rileva condizioni anomale tramite sensori, mancate conferme, tempi massimi superati o incoerenze. Le principali categorie sono:
- errori di movimento (assi VGR, HBW, MPO);
- errori di posizione o finecorsa;
- errori di sensori (colore, presenza, NFC);
- timeout operativi.

Il sistema passa allo stato ERROR e attende reset o intervento.

## 7. Transizioni e sincronizzazione
Ogni transizione tra fasi richiede:
- conferma del modulo precedente;
- assenza di errori;
- segnale di pronto per la fase successiva.

La sincronizzazione mantiene coerente l'intera pipeline produttiva.

