# 04 – Protocolli e Rete

## 1. Architettura di comunicazione
La comunicazione della microfactory combina protocolli industriali e IoT. Il PLC gestisce il controllo di processo tramite OPC-UA, mentre Gateway, TXT e Cloud scambiano dati su MQTT. Il router TP-Link mantiene la rete locale e il collegamento verso Internet.

```mermaid
flowchart LR
    PLC[[PLC S7-1500]] -- OPC-UA --> GW[IoT Gateway]
    GW -- MQTT --> TXT[TXT Controller]
    TXT -- MQTT --> CLOUD[(Cloud)]
    PLC <-. LAN/WiFi .-> ROUTER
    GW <-. LAN/WiFi .-> ROUTER
    TXT <-. WiFi .-> ROUTER
```

## 2. OPC-UA (PLC → IoT Gateway)
OPC-UA gestisce lo scambio dati tra PLC e Gateway con modalità affidabile e strutturata. Il PLC espone variabili come posizioni, stati dei moduli, avanzamento del ciclo e segnalazioni d’errore. Il Gateway accede a questi dati tramite sessioni OPC-UA e li utilizza per sincronizzare configurazioni, aggiornare dashboard locali e inoltrare informazioni al resto della catena.

## 3. MQTT (Gateway ↔ TXT ↔ Cloud)
MQTT permette comunicazioni leggere e continue tra Gateway, TXT e Cloud. Il modello publish/subscribe assicura uno scambio dati costante senza appesantire la rete. Il TXT invia telemetrie (sensori, NFC, camera) e riceve ordini o aggiornamenti dal Cloud. I topic principali sono:
- `/ftFactory/env` – dati ambientali SSC
- `/ftFactory/nfc/read` – letture NFC
- `/ftFactory/nfc/write` – comandi dal Cloud
- `/ftFactory/status/plc` – sincronizzazioni tra Gateway e PLC
- `/ftFactory/order` – ordini di produzione dal cloud al TXT

## 4. Flusso dei dati
Il flusso seguente riassume come viaggiano comandi e informazioni tra i vari livelli.

```mermaid
sequenceDiagram
    participant PLC
    participant GW as Gateway
    participant TXT
    participant CLOUD

    PLC->>GW: Variabili OPC-UA
    GW->>TXT: Sync MQTT
    TXT->>CLOUD: Telemetrie
    CLOUD->>TXT: Ordini
    TXT->>GW: Aggiornamenti
    GW->>PLC: Scritture OPC-UA
```

## 5. Topologia di Rete e Indirizzi IP Ufficiali della Learning Factory 4.0

La rete della Learning Factory 4.0 segue una configurazione standardizzata che garantisce comunicazione affidabile tra PLC, Gateway IoT, TXT 4.0 e Cloud.  
Ogni dispositivo utilizza un indirizzo IP fisso, definito dal router interno TP-Link configurato in modalità **WISP**.

### **Assegnazione IP dei componenti**

| Componente                                | Indirizzo IP      | Ruolo nella rete                                                            |
| ----------------------------------------- | ----------------- | --------------------------------------------------------------------------- |
| **PLC Siemens S7-1500**                   | **192.168.0.1**   | Nodo OT principale, server OPC-UA                                           |
| **IoT Gateway (Raspberry Pi – Node-RED)** | **192.168.0.5**   | Adattatore OPC-UA → MQTT, dashboard locale                                  |
| **TXT Controller 4.0**                    | **192.168.0.10**  | MQTT broker, interfaccia cloud, gestione camera/NFC/sensori                 |
| **Router TP-Link WR802N**                 | **192.168.0.252** | DHCP, rete interna, connessione WAN verso Internet, router in modalità WISP |

Questa mappatura consente:

- connessione **OPC-UA** stabile tra PLC e Gateway;
- inoltro dei dati via **MQTT** dal Gateway al TXT;
- collegamento del TXT alla **fischertechnik Cloud** tramite il router;
- accesso locale al Node-RED tramite browser.
- Subnet: `255.255.255.0`

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


## 6. Porte utilizzate
| Servizio | Porta | Funzione |
|---------|--------|----------|
| OPC-UA | 4840 | Scambio PLC–Gateway |
| MQTT | 1883 | Telemetria e comandi |
| HTTP | 1880 | Dashboard Node-RED |

## 7. Sincronizzazione
La sincronizzazione tra livelli avviene in modo continuo:
1. Il Gateway carica `ConfigData.csv` e valuta parametri e posizioni.
2. Il PLC legge i valori tramite OPC-UA e aggiorna la logica di controllo.
3. Il TXT riceve ordini dal Cloud e invia telemetria in tempo reale.
4. Il Gateway traduce messaggi tra TXT e PLC, mantenendo allineati controllo, IoT e Cloud.

## 8. Sicurezza di rete
La rete è isolata all’interno della subnet locale. Il router utilizza WPA2/WPA3 e mantiene separato l’accesso Internet dai dispositivi industriali. Il PLC non è esposto direttamente, mentre il Cloud comunica solo tramite MQTT autenticato.

## 9. Struttura complessiva
L’integrazione tra OPC-UA e MQTT consente alla microfactory di combinare controllo industriale deterministico con funzionalità IoT e supervisione remota, mantenendo la rete semplice e facilmente estendibile.

