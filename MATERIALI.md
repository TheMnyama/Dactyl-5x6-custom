# Guida ai Materiali e Componenti: Tastiera Dactyl Manuform (Amoeba King v1.1)

Questo file contiene l'elenco completo dei materiali, dei componenti elettronici e delle specifiche tecniche utilizzate per la realizzazione della tastiera meccanica ergonomica split **Dactyl Manuform (64 tasti)** con basette **Amoeba King v1.1** e microcontrollori **RP2040 USB-C**.

---

## 📋 Tabella Riassuntiva dei Componenti

| Componente | Quantità | Tipo/Modello Consigliato | Caratteristica Principale | Ruolo nel Circuito |
| :--- | :---: | :--- | :--- | :--- |
| **Case 3D** | 2 pezzi | Dactyl Manuform 5x6 (Mano SX e DX) | Design ergonomico curvo e split | Struttura portante per switch ed elettronica |
| **Basette PCB** | 64 pezzi | Amoeba King v1.1 | Single-switch PCB (ibrida THT/SMD) | Alloggiamento modulare per ogni singolo tasto |
| **Socket Hotswap** | 64 pezzi | Kailh / Gateron Hotswap Sockets | Connettori a pressione neri | Permettono di cambiare gli switch senza saldare |
| **Diodi** | 64 pezzi | 1N4148 (DO-35) | Adattati a montaggio superficiale | Valvola di non ritorno (previene il *ghosting*) |
| **Switch Meccanici** | 64 pezzi | A scelta (Cherry, Gateron, Kailh, ecc.) | 3-pin o 5-pin meccanici | Il pulsante fisico premuto dalle dita |
| **Microcontrollori** | 2 pezzi | RP2040 USB-C (Clone Raspberry Pi Pico) | Dual-core ARM Cortex-M0+, USB-C | Il "cervello" della tastiera (gestisce il firmware) |
| **Connettori Jack** | 2 pezzi | PJ-320A (TRRS 3.5mm Femmina) | Blocchetto nero a 4 pin nudo | Presa per il collegamento tra le due metà |
| **Cavo di Interconnessione** | 1 pezzo | Cavo TRRS Maschio-Maschio (4 Poli) | 3 righine nere su entrambi gli spinotti | Trasferisce in sicurezza i 4 segnali tra le metà |
| **Filo per Colonne** | Q.b. | Filo di rame nudo o rigido | Conduttore rigido | Unisce verticalmente i pad **C** della matrice |
| **Filo per Righe** | Q.b. | Filo di rame isolato (con guaina) | Isolamento plastico esterno | Unisce orizzontalmente i pad **R** evitando cortocircuiti |

---

## 🔍 Analisi Dettagliata e Caratteristiche Fisiche

### 1. Case Stampato in 3D (Dactyl Manuform)
- **Caratteristiche fisiche:** Scocca curva stampata in 3D, divisa in due metà speculari (Split). Presenta sul retro due aperture per l'I/O: un foro rettangolare grande (originariamente pensato per prese RJ9) e un foro più piccolo ad arco.
- **Adattamento pratico:** Le aperture posteriori vengono adattate o completate con appositi supporti stampati per fissare stabilmente la porta USB-C del microcontrollore (sulla metà principale) e i jack femmina TRRS `PJ-320A` su entrambe le metà.

### 2. Basette Modulari Amoeba King v1.1
- **Caratteristiche fisiche:** Piccole PCB viola progettate per ospitare un singolo tasto alla volta. Questa versione (`v1.1`) è una scheda "ibrida": ha i fori passanti per i pin dello switch e del socket, mentre le piazzole per i diodi (`D1`) sono piatte (SMD).
- **Connessioni esterne:** Presenta sui bordi esterni pad chiaramente marchiati: **`C`** (Column) in alto e in basso (collegati internamente tra loro) e **`R`** (Row) ai lati per il passaggio della matrice.

### 3. Socket Hot-Swap Neri
- **Caratteristiche fisiche:** Connettori in plastica nera con alette metalliche flessibili.
- **Saldatura:** Vengono saldati sul retro dell'Amoeba King (con la parte liscia rivolta verso il basso, in corrispondenza del foro dello switch). Permettono l'inserimento rapido a pressione dei piedini metallici degli switch, eliminando la necessità di saldare lo switch stesso e facilitando la manutenzione futura.

### 4. Diodi di Vetro (1N4148)
- **Caratteristiche fisiche:** Piccoli cilindretti di vetro arancione/rosso dotati di due lunghe gambe metalliche. Presentano una **riga nera** su un'estremità che ne indica la polarità (Catodo).
- **Tecnica di montaggio:** I diodi vengono posizionati sopra le piazzole argentate `D1` subito vicine al socket; una volta saldati, le gambe metalliche in eccesso vengono tagliate a misura.
- **Orientamento (`COL2ROW`):** La riga nera (catodo) deve essere rivolta verso il lato collegato allo switch/riga **`R`**, mentre il lato senza banda (anodo) è collegato alla colonna **`C`** (con la basetta orientata in modo da leggere le scritte e con il socket in basso, la riga nera guarda verso il basso).

### 5. Microcontrollori RP2040 USB-C (Compatibili Raspberry Pi Pico)
- **Caratteristiche fisiche:** Scheda di sviluppo (PCB viola) basata sul chip Raspberry Pi RP2040, dotata di porta **USB-C** nativa e di un LED di stato verde integrato collegato al pin `GP25` secondo lo standard della Raspberry Pi Pico originale.
- **Funzione:** Riceve le linee delle 6 colonne (es. pin `GP2`–`GP7`) e delle 5 righe (es. pin `GP8`–`GP12`) di ogni metà, gestisce la comunicazione seriale tra le due metà ed elabora la pressione dei tasti tramite firmware (KMK o QMK) inviando i comandi al PC via USB.

### 6. Jack TRRS Femmina (PJ-320A)
- **Caratteristiche fisiche:** Blocchetto rettangolare nero nudo da saldare. Dispone di **4 pin metallici** che corrispondono internamente ai 4 poli dello spinotto audio da 3.5 mm.
- **Schema dei Segnali:** Indispensabile per far viaggiare in sicurezza i 4 fili necessari alla comunicazione inter-tastiera: **VCC (`3V3`)**, **GND (Terra)**, **Data 1 (`GP0`)** e **Data 2 (`GP1`)**.

### 7. Cavo TRRS 4 Poli (Maschio-Maschio)
- **Caratteristiche fisiche:** Cavo schermato che presenta su **entrambe le estremità** uno spinotto da 3.5 mm con **3 righine nere isolanti** (che dividono il connettore in 4 sezioni metalliche separate).
- **Importanza:** È l'unico tipo di cavo sicuro. I comuni cavi audio stereo (TRS a 3 poli, con solo 2 righine nere) causerebbero un cortocircuito immediato tra i pin di alimentazione e i pin dati all'interno del jack `PJ-320A` (collegare o scollegare il cavo TRRS sempre a tastiera scollegata dal PC).
