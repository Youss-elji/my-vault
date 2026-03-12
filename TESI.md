1. capitolo INTRO 2-3 pagine, introduci il lavoro che è stato fatto a grandi linee con cosa ho lavorato e che cosa ho fatto, senza dettagli tecnici solo infarinatura
2. Stato dell'Arte 10-15 pagine, bisogna individuare le tecnologie le tecniche protocolli utilizzate: PLC OPCUA MQTT node red etc..
3. architecture 15-20 pagine, descrizione del setup dal punto di vista teorico che tipo di fishertecnik usato lato fisico, lato software l'algoritmo della tracciabilità del pezzo, racconto del lavoro
4. implementazione 10-15 pagine, riprendo nel capitolo preced. ma lo mostro nel dettaglio tecnico accio vedere le parti di codice notevoli nore red tia portal python etc..; si discute tecnicamente di ciò che ho relaizzato
5. espirimenti e usecase 5 pagine, raccolta dati grafici da definire metriche etc..
6.  conclusioni 2-3 pagine riprendo il capitolo 1 e ripeto ciò che ho fatto riassumendo

mi verrà dato un template da rispettare con formatazione su overleaf, fare profilo, verrà dato una tesi d'esempio, esportare grafici con pdf e immagini nitidie più possibile, disegnare diagrammi draw.io e mermaid


ricordati di documentare il ![[Pasted image 20260306110625.png]]
che ha risolto il problema del blocco del vgr e dei ordini e del smistamento

![[Pasted image 20260306125305.png]]

C:\Users\Youss\PycharmProjects\Analisi_dati_OPCUA_MQTT\.venv\Scripts\python.exe C:\Users\Youss\PycharmProjects\Analisi_dati_OPCUA_MQTT\factory_tracker_v6.py 
Connessione a opc.tcp://192.168.0.1:4840 ...
WARNING:asyncua.client.client:Requested session timeout to be 3600000ms, got 30000ms instead
Connesso!

======================================================================
  SNAPSHOT  –  06/03/2026 13:16:43
======================================================================

  📥 DSI
    DSI.x_active                           = False
    DSI.i_code                             = 1  (⚪ IDLE – pronto / in attesa)
    DSI.s_station                          = dsi
    DSI.s_target                           = 
    DSI.s_description                      = 

  📦 DSO
    DSO.x_active                           = False
    DSO.i_code                             = 1  (⚪ IDLE – pronto / in attesa)
    DSO.s_station                          = dso
    DSO.s_target                           = 
    DSO.s_description                      = 

  🏭 HBW
    HBW.x_active                           = False
    HBW.i_code                             = 1  (⚪ IDLE – pronto / in attesa)
    HBW.s_station                          = hbw
    HBW.s_target                           = 
    HBW.s_description                      = 

  ⚙️  MPO
    MPO.x_active                           = False
    MPO.i_code                             = 1  (⚪ IDLE – pronto / in attesa)
    MPO.s_station                          = mpo
    MPO.s_target                           = 
    MPO.s_description                      = 

  🎨 SLD
    SLD.x_active                           = False
    SLD.i_code                             = 1  (⚪ IDLE – pronto / in attesa)
    SLD.s_station                          = sld
    SLD.s_target                           = 
    SLD.s_description                      = 

  🤖 VGR
    VGR.x_active                           = False
    VGR.i_code                             = 1  (⚪ IDLE – pronto / in attesa)
    VGR.s_station                          = vgr
    VGR.s_target                           = 
    VGR.s_description                      = 

  📋 Order
    Order.s_state                          = WAITING_FOR_ORDER
    Order.s_type                           = 


  ┌──────────────── MAGAZZINO HBW ──────────────────────┐
  │          C0              C1              C2          │
  │  Riga 0:  📦BLUE(RAW)      📦RED(RAW)       ⬜VUOTO()      │
  │  Riga 1:  📦WHITE(RAW)     📦WHITE(RAW)     📦RED(RAW)     │
  │  Riga 2:  ⬜VUOTO()        ⬜VUOTO()        ⬜VUOTO()      │
  └─────────────────────────────────────────────────────┘

WARNING:asyncua.client.client:Revised values returned differ from subscription values: CreateSubscriptionResult(SubscriptionId=2064233501, RevisedPublishingInterval=1000.0, RevisedLifetimeCount=300, RevisedMaxKeepAliveCount=30)
Sottoscritti: 68/68 nodi

──────────────────────────────────────────────────────────────────────
  MONITOR v6  │  factory_events.csv + piece_log.csv  │  Ctrl+C
──────────────────────────────────────────────────────────────────────

[13:16:44.495] Stock           │ Stock.s_location                    —              → 
                  │  ↳ 📍 HBW posizione: 
[13:16:55.496] HBW             │ HBW.i_code                          1              → 2
                  │  ↳ 🏭 HBW stato → 🟡 BUSY – in lavorazione
[13:16:55.496] VGR             │ VGR.i_code                          1              → 2
                  │  ↳ 🤖 VGR stato → 🟡 BUSY – in lavorazione
[13:16:55.496] Order  [#WXGZ]  │ Order.s_state                       WAITING_FOR_OR → ORDERED
                  │  ↳ 📋 ORDINE RICEVUTO – #WXGZ in coda  [tipo: ]
[13:16:55.496] Order  [#WXGZ]  │ Order.s_type                                       → BLUE
                  │  ↳ 📋 Tipo ordinato → 'BLUE'
[13:17:09.500] HBW    [#WXGZ]  │ HBW.x_active                        False          → True
                  │  ↳ 🏭 HBW RECUPERA da scaffale [#WXGZ]
[13:17:10.499] Stock  [#WXGZ]  │ Stock.[0,0].s_id                    048484ca341290 → 
[13:17:10.500] Stock  [#WXGZ]  │ Stock.[0,0].s_type                  BLUE           → 

  ┌──────────────── MAGAZZINO HBW ──────────────────────┐
  │          C0              C1              C2          │
  │  Riga 0:  ⬜VUOTO()        📦RED(RAW)       ⬜VUOTO()      │
  │  Riga 1:  📦WHITE(RAW)     📦WHITE(RAW)     📦RED(RAW)     │
  │  Riga 2:  ⬜VUOTO()        ⬜VUOTO()        ⬜VUOTO()      │
  └─────────────────────────────────────────────────────┘

[13:17:10.500] Stock  [#WXGZ]  │ Stock.[0,0].s_state                 RAW            → 
                  │  ↳ 🏭 Slot [0,0] → VUOTO (prelievo)
[13:17:21.498] HBW    [#WXGZ]  │ HBW.i_code                          2              → 1
                  │  ↳ 🏭 HBW stato → ⚪ IDLE – pronto / in attesa
[13:17:23.499] MPO             │ MPO.i_code                          1              → 2
                  │  ↳ ⚙️  MPO stato → 🟡 BUSY – in lavorazione
[13:17:24.504] HBW    [#WXGZ]  │ HBW.x_active                        True           → False
                  │  ↳ 🏭 HBW: operazione COMPLETATA
[13:17:24.504] HBW    [#WXGZ]  │ HBW.i_code                          1              → 2
                  │  ↳ 🏭 HBW stato → 🟡 BUSY – in lavorazione
[13:17:29.500] VGR    [#WXGZ]  │ VGR.x_active                        False          → True
                  │  ↳ 🤖 VGR PRELEVA → — [#WXGZ]
[13:17:29.500] VGR    [#WXGZ]  │ VGR.s_target                                       → mpo
                  │  ↳ 🤖 VGR destinazione: MPO (Lavorazione)
[13:17:44.501] MPO    [#WXGZ]  │ MPO.x_active                        False          → True
                  │  ↳ ⚙️  MPO: LAVORAZIONE AVVIATA [#WXGZ]
[13:17:44.502] Order  [#WXGZ]  │ Order.s_state                       ORDERED        → IN_PROCESS
                  │  ↳ 🔄 Ordine IN LAVORAZIONE
[13:17:45.502] Stock  [#WXGZ]  │ Stock.[0,0].s_id                                   → 0
[13:17:53.500] HBW             │ HBW.i_code                          2              → 1
                  │  ↳ 🏭 HBW stato → ⚪ IDLE – pronto / in attesa
[13:17:58.501] HBW             │ HBW.i_code                          1              → 2
                  │  ↳ 🏭 HBW stato → 🟡 BUSY – in lavorazione
[13:17:58.501] VGR             │ VGR.x_active                        True           → False
                  │  ↳ 🤖 VGR DEPOSITA → MPO (Lavorazione)
[13:17:58.502] VGR             │ VGR.s_target                        mpo            → 
[13:17:58.502] Order  [#QKYC]  │ Order.s_state                       IN_PROCESS     → ORDERED
                  │  ↳ 📋 ORDINE RICEVUTO – #QKYC in coda  [tipo: BLUE]
[13:17:58.502] Order  [#QKYC]  │ Order.s_type                        BLUE           → RED
                  │  ↳ 📋 Tipo ordinato → 'RED'
[13:18:18.509] HBW    [#QKYC]  │ HBW.x_active                        False          → True
                  │  ↳ 🏭 HBW RECUPERA da scaffale [#QKYC]
[13:18:19.509] Stock  [#WXGZ]  │ Stock.[0,1].s_id                    044078ca341291 → 
[13:18:19.509] Stock  [#WXGZ]  │ Stock.[0,1].s_type                  RED            → 

  ┌──────────────── MAGAZZINO HBW ──────────────────────┐
  │          C0              C1              C2          │
  │  Riga 0:  ⬜VUOTO()        ⬜VUOTO()        ⬜VUOTO()      │
  │  Riga 1:  📦WHITE(RAW)     📦WHITE(RAW)     📦RED(RAW)     │
  │  Riga 2:  ⬜VUOTO()        ⬜VUOTO()        ⬜VUOTO()      │
  └─────────────────────────────────────────────────────┘

[13:18:19.509] Stock  [#WXGZ]  │ Stock.[0,1].s_state                 RAW            → 
                  │  ↳ 🏭 Slot [0,1] → VUOTO (prelievo)
[13:18:27.513] MPO    [#WXGZ]  │ MPO.x_active                        True           → False
                  │  ↳ ⚙️  MPO: LAVORAZIONE COMPLETATA
[13:18:27.514] SLD    [#WXGZ]  │ SLD.x_active                        False          → True
                  │  ↳ 🎨 SLD: SMISTAMENTO colore [#WXGZ]
[13:18:27.514] SLD    [#WXGZ]  │ SLD.i_code                          1              → 2
                  │  ↳ 🎨 SLD stato → 🟡 BUSY – in lavorazione
[13:18:32.512] MPO             │ MPO.i_code                          2              → 1
                  │  ↳ ⚙️  MPO stato → ⚪ IDLE – pronto / in attesa
[13:18:34.516] HBW    [#QKYC]  │ HBW.i_code                          2              → 1
                  │  ↳ 🏭 HBW stato → ⚪ IDLE – pronto / in attesa
[13:18:36.515] HBW    [#QKYC]  │ HBW.x_active                        True           → False
                  │  ↳ 🏭 HBW: operazione COMPLETATA
[13:18:36.516] HBW    [#QKYC]  │ HBW.i_code                          1              → 2
                  │  ↳ 🏭 HBW stato → 🟡 BUSY – in lavorazione
[13:18:36.516] MPO             │ MPO.i_code                          1              → 2
                  │  ↳ ⚙️  MPO stato → 🟡 BUSY – in lavorazione
[13:18:36.516] SLD    [#WXGZ]  │ SLD.i_code                          2              → 1
                  │  ↳ 🎨 SLD stato → ⚪ IDLE – pronto / in attesa
[13:18:41.515] VGR    [#QKYC]  │ VGR.x_active                        False          → True
                  │  ↳ 🤖 VGR PRELEVA → — [#QKYC]
[13:18:41.515] VGR    [#QKYC]  │ VGR.s_target                                       → mpo
                  │  ↳ 🤖 VGR destinazione: MPO (Lavorazione)
[13:18:57.516] MPO    [#QKYC]  │ MPO.x_active                        False          → True
                  │  ↳ ⚙️  MPO: LAVORAZIONE AVVIATA [#QKYC]
[13:18:57.516] Order  [#WXGZ]  │ Order.s_state                       ORDERED        → IN_PROCESS
                  │  ↳ 🔄 Ordine IN LAVORAZIONE
[13:19:02.519] Stock  [#WXGZ]  │ Stock.[0,1].s_id                                   → 0
[13:19:11.522] VGR             │ VGR.x_active                        True           → False
                  │  ↳ 🤖 VGR DEPOSITA → MPO (Lavorazione)
[13:19:11.522] VGR             │ VGR.s_target                        mpo            → 
[13:19:15.523] HBW             │ HBW.i_code                          2              → 1
                  │  ↳ 🏭 HBW stato → ⚪ IDLE – pronto / in attesa
[13:19:40.527] SLD    [#WXGZ]  │ SLD.x_active                        True           → False
                  │  ↳ 🎨 SLD: smistamento COMPLETATO
[13:19:42.528] VGR    [#WXGZ]  │ VGR.x_active                        False          → True
                  │  ↳ 🤖 VGR PRELEVA → — [#WXGZ]
[13:19:42.528] VGR    [#WXGZ]  │ VGR.s_target                                       → dso
                  │  ↳ 🤖 VGR destinazione: DSO (Uscita)
[13:19:51.533] MPO    [#QKYC]  │ MPO.x_active                        True           → False
                  │  ↳ ⚙️  MPO: LAVORAZIONE COMPLETATA
[13:19:51.534] SLD    [#QKYC]  │ SLD.x_active                        False          → True
                  │  ↳ 🎨 SLD: SMISTAMENTO colore [#QKYC]
[13:19:51.534] SLD    [#QKYC]  │ SLD.i_code                          1              → 2
                  │  ↳ 🎨 SLD stato → 🟡 BUSY – in lavorazione
[13:19:56.534] MPO             │ MPO.i_code                          2              → 1
                  │  ↳ ⚙️  MPO stato → ⚪ IDLE – pronto / in attesa
[13:19:59.536] SLD    [#QKYC]  │ SLD.i_code                          2              → 1
                  │  ↳ 🎨 SLD stato → ⚪ IDLE – pronto / in attesa
[13:20:19.540] DSO             │ DSO.i_code                          1              → 0
                  │  ↳ 📦 DSO stato → 🔴 FAULT – errore / vuoto
[13:20:20.539] DSO    [#WXGZ]  │ DSO.x_active                        False          → True
                  │  ↳ 📦 DSO: PEZZO CONSEGNATO in uscita [#WXGZ]
[13:20:20.539] Order  [#WXGZ]  │ Order.s_state                       IN_PROCESS     → SHIPPED
                  │  ↳ 🚚 Ordine SPEDITO – pezzo sul DSO
[13:20:20.540] Order  [#WXGZ]  │ Order.s_type                        RED            → BLUE
                  │  ↳ 📋 Tipo ordinato → 'BLUE'
[13:20:21.540] VGR             │ VGR.x_active                        True           → False
                  │  ↳ 🤖 VGR DEPOSITA → DSO (Uscita)
[13:20:21.540] VGR             │ VGR.s_target                        dso            → 
[13:20:25.541] DSO    [#WXGZ]  │ DSO.x_active                        True           → False
[13:20:25.542] Order  [#WXGZ]  │ Order.s_state                       SHIPPED        → WAITING_FOR_ORDER
                  │  ↳ ⏳ Fabbrica in ATTESA di nuovo ordine
[13:20:25.542] Order  [#WXGZ]  │ Order.s_type                        BLUE           → 

========================================================
  PEZZO #WXGZ  |  Tipo: BLUE
  Durata totale: 3m 34s  (13:16:55 -> 13:20:29)
========================================================
  ┌──────────────────┬──────────┬──────────┬──────────┐
  │     Stazione     │  Arrivo  │ Partenza │  Durata  │
  ├──────────────────┼──────────┼──────────┼──────────┤
  │ HBW (Magazzino)  │ 13:17:09 │ 13:17:29 │   20s    │
  │VGR (Robot Ventos │ 13:17:29 │ 13:17:44 │   15s    │
  │MPO (Lavorazione) │ 13:17:44 │ 13:18:27 │   43s    │
  │SLD (Smistamento) │ 13:18:27 │ 13:19:42 │  1m 15s  │
  │VGR (Robot Ventos │ 13:19:42 │ 13:20:20 │   38s    │
  │   DSO (Uscita)   │ 13:20:20 │ 13:20:29 │    9s    │
  └──────────────────┴──────────┴──────────┴──────────┘
========================================================

[13:20:29.544] DSO             │ DSO.i_code                          0              → 1
                  │  ↳ ✅ DSO SVUOTATA – #WXGZ [BLUE] 3m 34s – ID dimenticato
[13:20:46.544] SLD    [#QKYC]  │ SLD.x_active                        True           → False
                  │  ↳ 🎨 SLD: smistamento COMPLETATO
[13:20:47.545] VGR    [#QKYC]  │ VGR.x_active                        False          → True
                  │  ↳ 🤖 VGR PRELEVA → — [#QKYC]
[13:20:47.546] VGR    [#QKYC]  │ VGR.s_target                                       → dso
                  │  ↳ 🤖 VGR destinazione: DSO (Uscita)
[13:21:08.547] DSI    [#Y2C8]  │ DSI.x_active                        False          → True
                  │  ↳ 📥 DSI: PEZZO RILEVATO – ID #Y2C8 assegnato
[13:21:08.547] DSI    [#Y2C8]  │ DSI.i_code                          1              → 0
                  │  ↳ 📥 DSI stato → 🔴 FAULT – errore / vuoto
[13:21:24.548] DSO    [#QKYC]  │ DSO.x_active                        False          → True
                  │  ↳ 📦 DSO: PEZZO CONSEGNATO in uscita [#QKYC]
[13:21:24.548] DSO    [#QKYC]  │ DSO.i_code                          1              → 0
                  │  ↳ 📦 DSO stato → 🔴 FAULT – errore / vuoto
[13:21:24.548] Order  [#QKYC]  │ Order.s_state                       WAITING_FOR_OR → SHIPPED
                  │  ↳ 🚚 Ordine SPEDITO – pezzo sul DSO
[13:21:24.548] Order  [#QKYC]  │ Order.s_type                                       → RED
                  │  ↳ 📋 Tipo ordinato → 'RED'
[13:21:25.558] VGR             │ VGR.x_active                        True           → False
                  │  ↳ 🤖 VGR DEPOSITA → DSO (Uscita)
[13:21:25.558] VGR             │ VGR.s_target                        dso            → 
[13:21:29.548] DSO    [#QKYC]  │ DSO.x_active                        True           → False
[13:21:29.548] Order  [#QKYC]  │ Order.s_state                       SHIPPED        → WAITING_FOR_ORDER
                  │  ↳ ⏳ Fabbrica in ATTESA di nuovo ordine
[13:21:29.548] Order  [#QKYC]  │ Order.s_type                        RED            → 

========================================================
  PEZZO #QKYC  |  Tipo: RED
  Durata totale: 3m 33s  (13:17:58 -> 13:21:31)
========================================================
  ┌──────────────────┬──────────┬──────────┬──────────┐
  │     Stazione     │  Arrivo  │ Partenza │  Durata  │
  ├──────────────────┼──────────┼──────────┼──────────┤
  │ HBW (Magazzino)  │ 13:18:18 │ 13:18:41 │   23s    │
  │VGR (Robot Ventos │ 13:18:41 │ 13:18:57 │   16s    │
  │MPO (Lavorazione) │ 13:18:57 │ 13:19:51 │   54s    │
  │SLD (Smistamento) │ 13:19:51 │ 13:20:47 │   56s    │
  │VGR (Robot Ventos │ 13:20:47 │ 13:21:24 │   37s    │
  │   DSO (Uscita)   │ 13:21:24 │ 13:21:31 │    7s    │
  └──────────────────┴──────────┴──────────┴──────────┘
========================================================

[13:21:31.550] DSO             │ DSO.i_code                          0              → 1
                  │  ↳ ✅ DSO SVUOTATA – #QKYC [RED] 3m 33s – ID dimenticato
[13:21:34.548] HBW             │ HBW.i_code                          1              → 2
                  │  ↳ 🏭 HBW stato → 🟡 BUSY – in lavorazione
[13:21:44.550] DSI    [#Y2C8]  │ DSI.x_active                        True           → False
                  │  ↳ 📤 DSI: stazione LIBERA
[13:21:44.550] DSI    [#Y2C8]  │ DSI.i_code                          0              → 1
                  │  ↳ 📥 DSI stato → ⚪ IDLE – pronto / in attesa
[13:21:45.548] VGR    [#Y2C8]  │ VGR.x_active                        False          → True
                  │  ↳ 🤖 VGR PRELEVA → — [#Y2C8]
[13:21:45.549] VGR    [#Y2C8]  │ VGR.s_target                                       → hbw
                  │  ↳ 🤖 VGR destinazione: HBW (Magazzino)
[13:21:50.550] Stock  [#Y2C8]  │ Stock.[0,0].s_id                    0              → 
[13:22:00.548] DSI    [#4OVR]  │ DSI.x_active                        False          → True
                  │  ↳ 📥 DSI: PEZZO RILEVATO – ID #4OVR assegnato
[13:22:00.548] DSI    [#4OVR]  │ DSI.i_code                          1              → 0
                  │  ↳ 📥 DSI stato → 🔴 FAULT – errore / vuoto
[13:22:00.548] HBW             │ HBW.i_code                          2              → 1
                  │  ↳ 🏭 HBW stato → ⚪ IDLE – pronto / in attesa
[13:23:02.558] HBW    [#Y2C8]  │ HBW.x_active                        False          → True
                  │  ↳ 🏭 HBW RICEVE da VGR (stoccaggio) [#Y2C8]
[13:23:02.558] HBW    [#Y2C8]  │ HBW.i_code                          1              → 2
                  │  ↳ 🏭 HBW stato → 🟡 BUSY – in lavorazione
[13:23:04.557] VGR             │ VGR.x_active                        True           → False
                  │  ↳ 🤖 VGR DEPOSITA → HBW (Magazzino)
[13:23:04.557] VGR             │ VGR.s_target                        hbw            → 
[13:23:20.558] HBW    [#Y2C8]  │ HBW.x_active                        True           → False
                  │  ↳ 🏭 HBW: operazione COMPLETATA
[13:23:21.557] Stock  [#Y2C8]  │ Stock.[0,0].s_id                                   → 048484ca341290
[13:23:21.557] Stock  [#Y2C8]  │ Stock.[0,0].s_type                                 → BLUE

  ┌──────────────── MAGAZZINO HBW ──────────────────────┐
  │          C0              C1              C2          │
  │  Riga 0:  📦BLUE(RAW)      ⬜VUOTO()        ⬜VUOTO()      │
  │  Riga 1:  📦WHITE(RAW)     📦WHITE(RAW)     📦RED(RAW)     │
  │  Riga 2:  ⬜VUOTO()        ⬜VUOTO()        ⬜VUOTO()      │
  └─────────────────────────────────────────────────────┘


========================================================
  PEZZO #Y2C8  |  Tipo: N/D
  Durata totale: 2m 13s  (13:21:08 -> 13:23:21)
========================================================
  ┌──────────────────┬──────────┬──────────┬──────────┐
  │     Stazione     │  Arrivo  │ Partenza │  Durata  │
  ├──────────────────┼──────────┼──────────┼──────────┤
  │  DSI (Ingresso)  │ 13:21:08 │ 13:21:45 │   37s    │
  │VGR (Robot Ventos │ 13:21:45 │ 13:23:02 │  1m 17s  │
  │ HBW (Magazzino)  │ 13:23:02 │ 13:23:21 │   18s    │
  └──────────────────┴──────────┴──────────┴──────────┘
========================================================

[13:23:21.557] Stock  [#4OVR]  │ Stock.[0,0].s_state                                → RAW
                  │  ↳ 🏭 Slot [0,0] → RAW  ✅ #Y2C8 [?] stoccato in 2m 13s – ID dimenticato
[13:23:30.558] DSI    [#4OVR]  │ DSI.x_active                        True           → False
                  │  ↳ 📤 DSI: stazione LIBERA
[13:23:30.559] DSI    [#4OVR]  │ DSI.i_code                          0              → 1
                  │  ↳ 📥 DSI stato → ⚪ IDLE – pronto / in attesa
[13:23:31.561] VGR    [#4OVR]  │ VGR.x_active                        False          → True
                  │  ↳ 🤖 VGR PRELEVA → — [#4OVR]
[13:23:31.561] VGR    [#4OVR]  │ VGR.s_target                                       → hbw
                  │  ↳ 🤖 VGR destinazione: HBW (Magazzino)
[13:23:45.560] Stock  [#4OVR]  │ Stock.[2,0].s_id                    0              → 
[13:23:56.560] HBW             │ HBW.i_code                          2              → 1
                  │  ↳ 🏭 HBW stato → ⚪ IDLE – pronto / in attesa
[13:24:51.565] HBW    [#4OVR]  │ HBW.x_active                        False          → True
                  │  ↳ 🏭 HBW RICEVE da VGR (stoccaggio) [#4OVR]
[13:24:51.565] HBW    [#4OVR]  │ HBW.i_code                          1              → 2
                  │  ↳ 🏭 HBW stato → 🟡 BUSY – in lavorazione
[13:24:52.568] VGR             │ VGR.x_active                        True           → False
                  │  ↳ 🤖 VGR DEPOSITA → HBW (Magazzino)
[13:24:52.569] VGR             │ VGR.s_target                        hbw            → 
[13:25:08.581] HBW    [#4OVR]  │ HBW.x_active                        True           → False
                  │  ↳ 🏭 HBW: operazione COMPLETATA
[13:25:08.581] Stock  [#4OVR]  │ Stock.[2,0].s_id                                   → 044078ca341291
[13:25:08.582] Stock  [#4OVR]  │ Stock.[2,0].s_type                                 → RED

  ┌──────────────── MAGAZZINO HBW ──────────────────────┐
  │          C0              C1              C2          │
  │  Riga 0:  📦BLUE(RAW)      ⬜VUOTO()        ⬜VUOTO()      │
  │  Riga 1:  📦WHITE(RAW)     📦WHITE(RAW)     📦RED(RAW)     │
  │  Riga 2:  📦RED(RAW)       ⬜VUOTO()        ⬜VUOTO()      │
  └─────────────────────────────────────────────────────┘


========================================================
  PEZZO #4OVR  |  Tipo: N/D
  Durata totale: 3m 08s  (13:22:00 -> 13:25:08)
========================================================
  ┌──────────────────┬──────────┬──────────┬──────────┐
  │     Stazione     │  Arrivo  │ Partenza │  Durata  │
  ├──────────────────┼──────────┼──────────┼──────────┤
  │  DSI (Ingresso)  │ 13:22:00 │ 13:23:31 │  1m 31s  │
  │VGR (Robot Ventos │ 13:23:31 │ 13:24:51 │  1m 20s  │
  │ HBW (Magazzino)  │ 13:24:51 │ 13:25:08 │   17s    │
  └──────────────────┴──────────┴──────────┴──────────┘
========================================================

[13:25:08.582] Stock           │ Stock.[2,0].s_state                                → RAW
                  │  ↳ 🏭 Slot [2,0] → RAW  ✅ #4OVR [?] stoccato in 3m 08s – ID dimenticato
[13:25:09.568] VGR             │ VGR.i_code                          2              → 1
                  │  ↳ 🤖 VGR stato → ⚪ IDLE – pronto / in attesa
[13:25:20.570] HBW             │ HBW.i_code                          2              → 1
                  │  ↳ 🏭 HBW stato → ⚪ IDLE – pronto / in attesa
WARNING:asyncua.client.ua_client.UASocketProtocol:ServiceFault (BadNoSubscription, diagnostics: DiagnosticInfo(SymbolicId=None, NamespaceURI=None, Locale=None, LocalizedText=None, AdditionalInfo=None, InnerStatusCode=None, InnerDiagnosticInfo=None)) from server received  in response to PublishRequest
Log salvati: factory_events.csv  +  piece_log.csv

Process finished with exit code 0
