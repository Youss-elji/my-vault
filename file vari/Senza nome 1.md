## 3.1 Ruolo del TXT 4.0 come MQTT broker e interfaccia verso il Cloud

Il controller **TXT 4.0** svolge un duplice ruolo all’interno della Learning Factory 4.0:

1. **Broker MQTT locale**  
   Il TXT ospita un broker MQTT che funge da punto di raccolta per tutti i messaggi scambiati tra il livello OT/Edge e il livello Cloud.  
   - L’**IoT Gateway (Node-RED)** si connette al TXT come *client MQTT* e pubblica i dati di produzione (stato dei moduli, avanzamento ordini, allarmi, valori di processo).  
   - Gli stessi topic vengono utilizzati per trasferire i comandi provenienti dal cloud (nuovi ordini, reset, impostazioni) verso il gateway e, di conseguenza, verso il PLC.

2. **Interfaccia verso la fischertechnik Cloud**  
   Il TXT si connette alla **fischertechnik Cloud** utilizzando la configurazione cloud integrata nel firmware. Dopo il pairing (tramite QR code o codice di associazione), il TXT:
   - inoltra al cloud i dati ricevuti dal gateway e dai sensori locali (telecamera, stazione ambientale, NFC);
   - riceve dal cloud gli ordini di produzione e le impostazioni definite nella dashboard (ad es. creazione di nuovi ordini o reset della fabbrica).

Sul TXT viene eseguita l’applicazione **Gateway_PLC.py**, che implementa la logica di collegamento tra il broker MQTT locale e i servizi cloud. In questo modo il TXT 4.0 rappresenta il nodo centrale del dominio IoT: aggrega i dati della fabbrica, li espone al cloud e rende possibile il monitoraggio e il controllo remoto del sistema.

---

## 3.2 Topologia di Rete e Indirizzi IP Ufficiali della Learning Factory 4.0

La rete della Learning Factory 4.0 segue una configurazione standardizzata che garantisce comunicazione affidabile tra PLC, Gateway IoT, TXT 4.0 e Cloud.  
Ogni dispositivo utilizza un indirizzo IP fisso, definito dal router interno TP-Link configurato in modalità **WISP**.

### **Assegnazione IP dei componenti**

| Componente | Indirizzo IP | Ruolo nella rete |
|------------|--------------|------------------|
| **PLC Siemens S7-1500** | **192.168.0.1** | Nodo OT principale, server OPC-UA |
| **IoT Gateway (Raspberry Pi – Node-RED)** | **192.168.0.5** | Adattatore OPC-UA → MQTT, dashboard locale |
| **TXT Controller 4.0** | **192.168.0.10** | MQTT broker, interfaccia cloud, gestione camera/NFC/sensori |
| **Router TP-Link WR802N** | **192.168.0.252** | DHCP, rete interna, connessione WAN verso Internet |

Questa mappatura consente:

- connessione **OPC-UA** stabile tra PLC e Gateway;
- inoltro dei dati via **MQTT** dal Gateway al TXT;
- collegamento del TXT alla **fischertechnik Cloud** tramite il router;
- accesso locale al Node-RED tramite browser.

### **Schema semplificato della topologia di rete**

```text 
Internet 
│
TP-Link WR802N (192.168.0.252) 
├── PLC S7-1500 (192.168.0.1)
├── IoT Gateway – Node-RED (192.168.0.5) 
└── TXT 4.0 Controller (192.168.0.10)
```

Questa struttura rappresenta il backbone della comunicazione OT/IT, assicurando che ogni nodo della fabbrica sia raggiungibile e sincronizzato con gli altri componenti e con il cloud.

---

## 3.3 I Tre Livelli di Controllo della Learning Factory 4.0

La Learning Factory 4.0 si basa su un’architettura multilivello in cui ogni componente svolge un ruolo distinto nel processo di controllo e supervisione. La documentazione ufficiale definisce chiaramente tre livelli funzionali che cooperano per garantire il funzionamento integrato OT/IT.

### **Livello 1 – Controllo Operativo (OT): PLC Siemens S7-1500**

Il PLC costituisce il cuore del livello OT e governa direttamente:
- sensori e attuatori dei moduli fisici (HBW, VGR, MPO, SLD, DPS, SSC);
- sequenze di movimentazione e logiche interbloccate;
- sicurezza operativa, gestione errori e sincronizzazione tra moduli.

Il PLC espone i dati in formato **OPC-UA**, rendendo possibile l'integrazione con i livelli superiori.

---

### **Livello 2 – Edge Gateway: IoT Gateway con Node-RED**

Il Gateway funge da ponte tra OT e IT.  
Le sue responsabilità principali includono:
- acquisizione dei dati dal PLC tramite **OPC-UA**;
- conversione e pubblicazione di tali dati verso MQTT (TXT 4.0);
- implementazione della dashboard locale Node-RED per:
  - calibrazioni (HBW, VGR, SSC, SLD),
  - reset errori,
  - monitoraggio in tempo reale,
  - gestione dei file di configurazione.

Grazie a questo livello intermedio, l’OT può dialogare con i servizi cloud attraverso protocolli nativamente orientati all’IoT.

---

### **Livello 3 – Dominio IT/Cloud: TXT 4.0 e Fischertechnik Cloud**

Il terzo livello comprende la parte di supervisione, servizi digitali e tracciamento.

**Il TXT 4.0 svolge funzioni fondamentali:**
- broker MQTT del sistema;
- gestione dei sensori locali (ambientali, luminosità, NFC);
- gestione della telecamera USB;
- sincronizzazione degli stati con la fischertechnik Cloud.

La **Cloud fischertechnik** fornisce infine:
- dashboard remota (cliente, fornitore, produzione);
- gestione ordini e tracciamento dei pezzi tramite NFC;
- monitoraggio dei dati ambientali e delle immagini della camera;
- storicizzazione dei processi.

---

Questa suddivisione in tre livelli consente una chiara separazione tra controllo fisico, adattamento protocollare e gestione dei servizi digitali, garantendo scalabilità, sicurezza e allineamento ai principi di Industria 4.0.
