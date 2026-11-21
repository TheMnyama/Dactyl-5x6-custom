# La mia Dactyl Manuform 5x6 Custom Keyboard ⌨️

Benvenuto nel repository della mia versione della tastiera meccanica ergonomica split **Dactyl Manuform 5x6**. 
Questo progetto include i file 3D, l'elenco dei materiali, le istruzioni di cablaggio e il firmware utilizzato.

[FOTO PRINCIPALE]

## Caratteristiche
- **Layout:** 5x6 Split Ergonomico (64 tasti)
- **Switch:** *[Inserisci qui il modello dei tuoi switch, es. Gateron Yellow / Cherry MX Brown]*
- **PCB / Cablaggio:** Handwired modulare con basette singole **Amoeba King v1.1** (Hot-Swap)
- **Microcontrollore:** 2x RP2040 USB-C (variante compatibile *Raspberry Pi Pico*)
- **Firmware:** KMK Firmware su CircuitPython `10.3.2`

---

## Istruzioni per la Stampa 3D
Tutti i file `.stl` per stampare le due metà della scocca e i coperchi inferiori si trovano nella cartella `/stl`.

---

## Specifiche Hardware e Preparazione Firmware

### 1. Microcontrollori (MCU)
Per gestire le due metà della tastiera (configurazione Split) vengono utilizzati due microcontrollori basati su chip **RP2040** (variante compatibile *Raspberry Pi Pico* con porta USB-C e PCB viola):

- **Architettura:** Dual-core Arm Cortex-M0+ (Raspberry Pi RP2040)
- **Bootloader:** `UF2 Bootloader v3.0` (`Model: Raspberry Pi RP2`, `Board-ID: RPI-RP2`)
- **Firmware base:** CircuitPython `10.3.2` (build ufficiale per **Raspberry Pi Pico**, lingua `ENGLISH-US`)
- **Firmware tastiera:** KMK Firmware (comunicazione split via UART)

> **Nota tecnica sulla compatibilità:** Nonostante l'aspetto estetico simile alle schede *YD-RP2040*, queste schede utilizzano la mappatura pin standard della **Raspberry Pi Pico** (con LED di stato integrato su `GP25`). È quindi necessario installare la build di CircuitPython specifica per `Raspberry Pi Pico` affinché il pinout di sistema corrisponda correttamente.

### 2. Inizializzazione e Validazione Preliminare
Prima dell'assemblaggio fisico, entrambe le schede sono state inizializzate e testate singolarmente per escludere difetti hardware:

1. **Reset della memoria Flash:** Avvio in modalità bootloader (`RPI-RP2`) tramite pulsante `BOOTSEL` e pulizia completa della memoria flash mediante `nuke_universal.uf2`.
2. **Installazione CircuitPython:** Caricamento del file `.uf2` di CircuitPython 10.3.2 (*Raspberry Pi Pico*) fino alla comparsa dell'unità `CIRCUITPY`.
3. **Test hardware su pin `GP25`:** Verifica del corretto funzionamento del runtime Python e del LED di stato verde integrato. Per eseguire la prova, copiare il codice di uno degli script presenti nella cartella [`/firmware/test_led/`](firmware/test_led/) all'interno del file `code.py` nell'unità `CIRCUITPY` e salvare:
   - **[`led_fisso.py`](firmware/test_led/led_fisso.py):** Accensione continua del LED verde su `board.GP25` tramite modulo `digitalio`.
   - **[`led_lampeggiante.py`](firmware/test_led/led_lampeggiante.py):** Ciclo di accensione e spegnimento con intervallo di `0.5s`.
   - 
---

## Procedimento di Costruzione

### 1. Stampa 3D e Rifinitura del Case
Stampa delle due metà del case in 3D e rifinitura manuale con carta vetrata per eliminare eventuali sbavature o imperfezioni (in particolare all'interno degli alloggiamenti quadrati degli switch, per garantire un incastro preciso senza forzare le basette).

*[IMMAGINE CASE 3D]*

### 2. Verifica dei Microcontrollori
Inizializzazione delle due schede RP2040 USB-C con `nuke_universal.uf2`, installazione di CircuitPython (`Raspberry Pi Pico`) ed esecuzione degli script di test sul LED `GP25` come descritto nella sezione precedente.

### 3. Preparazione Basette Singolo Switch (Amoeba King v1.1)
Per prevenire fenomeni di *ghosting* nella matrice 5x6, ogni switch è montato su una basetta individuale **Amoeba King v1.1** equipaggiata con un socket hot-swap e un diodo **1N4148**. La configurazione adottata per la matrice è **`COL2ROW`** (flusso di corrente dalle Colonne verso le Righe).

#### 3.1 Individuazione delle piazzole del diodo con il multimetro
Prima di saldare, è stato verificato il tracciato interno del PCB impostando il multimetro in **modalità continuità (cicalino)** per identificare il collegamento corretto in configurazione `COL2ROW`:

1. **Identificazione del collegamento tra Switch e Riga (`R`):** Si posiziona un puntale sulla piazzola **`R`** e si toccano le due piazzole del socket dello switch. La piazzola del socket che emette il segnale di continuità è collegata direttamente alla riga.
2. **Individuazione del punto di arrivo del diodo (Catodo / Banda nera):** Spostando un puntale sulla **seconda piazzola del socket** (quella non collegata direttamente a `R`), si cercano con l'altro puntale le piazzole dedicate al diodo fino a rilevare continuità. Quello è il punto in cui termina il diodo e dove va orientata la **banda nera**.
3. **Individuazione del punto di partenza del diodo (Anodo):** Posizionando un puntale sulla piazzola **`C` (Colonna)**, si tocca la restante piazzola del diodo fino a rilevare continuità. Quello è il punto di ingresso del diodo, dove va saldato il **lato senza banda**.

Il percorso elettrico risultante per ogni singolo tasto è quindi:
`Colonna (C) → Diodo (Anodo → Banda nera) → Switch → Riga (R)`

#### 3.2 Saldatura di Socket Hot-Swap e Diodi 1N4148
Si procede saldando prima i socket hot-swap sulle basette Amoeba King v1.1 e successivamente i diodi `1N4148` nelle apposite piazzole `D1`, prestando attenzione al verso della banda nera.

*[IMMAGINE AMOEBA, SOCKET E DIODI SALDATI]*

**Accortezze di montaggio ed errori comuni da evitare:**
- **Orientamento corretto del socket hot-swap:** Il socket deve essere posizionato con la **parte liscia** (priva di protuberanze) rivolta verso il basso, in corrispondenza del foro centrale per lo switch. Montandolo al contrario, la sporgenza ostruisce la sede e impedisce il corretto inserimento dei pin dello switch.
- **Gestione del calore tra una saldatura e l'altra:** Non saldare il diodo e il socket in rapida sequenza immediata. È necessario attendere qualche secondo tra una saldatura e la successiva affinché la basetta si raffreddi: un eccessivo accumulo di calore può sollevare o danneggiare irreversibilmente le piazzole in rame del PCB.
- **Taglio dei reofori (gambe) dei diodi:** Le gambe in eccesso dei diodi `1N4148` devono essere tagliate **solo dopo** aver completato la saldatura sulla Amoeba King. In questo modo è più semplice tenere fermo il componente durante la saldatura e si può regolare con precisione la lunghezza residua in base agli ingombri interni del case.

#### 3.3 Verifica della polarità e test del singolo tasto
Una volta saldati sia il diodo `1N4148` sia il socket hot-swap sulla basetta, si inserisce fisicamente uno switch meccanico nel socket per testare l'intero singolo tasto prima di procedere con il cablaggio della matrice.

Impostando il multimetro in **modalità Prova Diodi**:
- **Conduzione diretta (test di attivazione tasto):** Si appoggia il puntale **rosso (+)** sulla piazzola **`C`** e il puntale **nero (-)** sulla piazzola **`R`**. Tenendo i puntali in posizione, si **preme lo switch**: il circuito si chiude e sul display del multimetro il valore cambia istantaneamente, rilevando il passaggio di corrente attraverso il diodo (caduta di tensione tipica di ~0.55V – 0.70V). Rilasciando il tasto, il circuito torna aperto.
- **Blocco inverso:** Invertendo i puntali (puntale **rosso** su **`R`** e puntale **nero** su **`C`**) e premendo lo switch, il valore sul display non cambia (resta a `0` / circuito aperto): il diodo blocca correttamente il flusso inverso, garantendo l'isolamento contro i tasti fantasma (*ghosting*).

> **Nota di compatibilità firmware:** Il cablaggio fisico in configurazione `COL2ROW` è uno standard universale. Sebbene questo progetto utilizzi **KMK Firmware** su CircuitPython, la matrice hardware così saldata è già predisposta per essere utilizzata senza alcuna modifica fisica anche con **QMK** (inclusi VIA/VIAL), **PRK** o **ZMK**.
