# 1. Introduzione

## 1.1 Contesto generale
La digitalizzazione dei processi produttivi industriali richiede sistemi capaci di integrare sensori, attuatori, logiche di controllo e servizi digitali in un unico ecosistema coerente. Questo paradigma, noto come **Industria 4.0**, promuove una produzione connessa, adattiva e basata sui dati, in cui ogni fase del processo è monitorabile e ottimizzabile in tempo reale.

La **Learning Factory 4.0 – Fischertechnik 24V** rappresenta un modello didattico e sperimentale progettato per replicare su piccola scala i principi fondamentali dei sistemi cyber-fisici industriali. È un ambiente modulare e completamente automatizzato che consente di studiare e comprendere:

- processi di produzione automatizzata  
- comunicazione industriale (OPC-UA, MQTT)  
- logiche PLC  
- sensori ambientali e tracciamento NFC  
- integrazione cloud e monitoraggio remoto  

Questa piattaforma è pensata per attività formative nelle scuole tecniche, nelle università e nei reparti di Ricerca & Sviluppo, permettendo di simulare scenari realistici prima di implementazioni su larga scala.  

---

## 1.2 Obiettivi della microfactory
Gli obiettivi principali del modello didattico sono:

- **Simulare il processo completo di produzione**, dall’ordine del cliente fino alla consegna del pezzo finito.
- **Mostrare l’interazione tra moduli fisici** (robot, magazzino, linee di lavorazione, sensori).
- **Studiare sistemi cyber-fisici reali**, con PLC Siemens, gateway IoT e controller embedded.
- **Fornire strumenti per analizzare dati di processo**, grazie a sensori ambientali e dashboard cloud.
- **Comprendere sistemi di tracciamento digitale**, tramite tag NFC integrati nei pezzi.
- **Mostrare l’interconnessione con piattaforme cloud**, parte essenziale di ogni architettura Industry 4.0.

La Learning Factory 4.0 permette di visualizzare il ciclo completo: ordine, stoccaggio materie prime, lavorazione, smistamento, identificazione, tracciamento e consegna.  

---

## 1.3 Architettura generale
La fabbrica è composta da una serie di moduli fisici interconnessi, ciascuno con un ruolo ben definito nel processo produttivo:

- **VGR – Vacuum Gripper Robot**: manipolazione pezzi  
- **HBW – High-Bay Warehouse**: magazzino automatico verticale  
- **MPO – Multi-Processing Station**: forno + operazioni di lavorazione  
- **SLD – Sorting Line**: smistamento tramite sensori colore  
- **SSC – Sensor Station + Camera**: monitoraggio ambientale e visivo  
- **DPS – Input/Output + NFC**: identificazione e gestione pezzi  
- **PLC Siemens S7-1500**: logica di controllo sequenziale  
- **IoT Gateway (Raspberry Pi + Node-RED)**: ponte OPC-UA → MQTT  
- **TXT 4.0 Controller**: interfaccia cloud + gestione camera/sensori  
- **TP-Link Router (WISP)**: accesso WAN e rete locale della fabbrica

La comunicazione tra i componenti avviene attraverso una catena integrata:

- **PLC → IoT Gateway** tramite OPC-UA  
- **IoT Gateway → TXT Controller** tramite MQTT  
- **TXT Controller → Cloud Fischertechnik**

Questo modello rispecchia fedelmente le architetture industriali moderne in cui PLC locali, gateway edge e servizi cloud lavorano insieme.  

---

## 1.4 Processo simulato: ordine → produzione → consegna
Il ciclo produttivo implementato segue una pipeline digitale:

1. Il cliente invia un ordine tramite dashboard cloud.  
2. Il sistema verifica disponibilità delle materie prime.  
3. Il robot VGR preleva il pezzo dal magazzino o dall’input.  
4. Il pezzo passa attraverso il forno e la lavorazione meccanica.  
5. La linea SLD ne determina il colore e lo smista.  
6. La stazione DPS scrive sul tag NFC i dati di produzione.  
7. Il pezzo finito viene depositato nell’area di uscita.  
8. L’intero processo è visibile in tempo reale sul cloud.  

Questo flusso rappresenta un caso d’uso industriale completo: logistica → produzione → qualità → consegna.  

---

## 1.5 Componenti smart e tracciamento digitale
Ogni pezzo è dotato di un **tag NFC NTAG213**, che contiene un ID univoco e vari metadati (stato ordine, risultati lavorazione, timestamp, ecc.).  

Questo permette:

- tracciabilità completa del ciclo
- controllo qualità basato su contenuti del tag
- storicizzazione dei dati in cloud
- simulazione realistica di supply chain intelligenti

---

## 1.6 Dashboard e monitoraggio remoto
Il sistema include due dashboard ufficiali:

- **Fischertechnik Cloud Dashboard**  
  - ordini  
  - stato fabbrica  
  - produzione  
  - lettura NFC  
  - telecamera live  
  - dati ambientali  

- **Node-RED Dashboard (rete locale)**  
  - calibrazioni  
  - posizioni dei motori  
  - reset errori  
  - configurazione sensori  
  - overview HMI locale  

Entrambe sono parte centrale del modello Industry 4.0, in quanto mostrano l’integrazione tra dati fisici e sistemi informativi.  

---
