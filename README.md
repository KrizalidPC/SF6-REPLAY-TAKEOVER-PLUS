# SF6 Final Takeover Tool 🤖⚔️

[![Python Version](https://shields.io)](https://python.org)
[![Platform](https://shields.io)](https://microsoft.com)
[![License](https://shields.io)](LICENSE)

Il **SF6 Final Takeover Tool** è un assistente didattico esterno, non invasivo e guidato dall'Intelligenza Visiva, progettato per affiancare i giocatori di *Street Fighter 6* durante l'utilizzo della funzione nativa **Replay Takeover**. 

Ispirato al sistema automatico di suggerimenti (*Replay Tips*) di *Tekken 8*, questo strumento analizza lo schermo in tempo reale e proietta un **Metronomo Visivo e Acustico** sincronizzato al millisecondo (60 FPS) sopra il gioco. Aiuta a sviluppare la memoria muscolare insegnando il tempismo esatto per interruzioni, punizioni e combo, agendo in totale sicurezza e **senza alcuna iniezione di input o rischio di ban** (100% Anti-Cheat Safe).

---

## 🏛️ Architettura del Software

Il progetto è strutturato secondo un'architettura modulare a bassissima latenza per non impattare sulle prestazioni del gioco:

1. **Modulo 1: CV Background Monitor (`sf6_cv_monitor.py`)**  
   Sfrutta la libreria C-based `mss` per catturare porzioni chirurgiche dello schermo (ROI) a frequenza stabilita (~30Hz) e applica algoritmi di *Template Matching* tramite `OpenCV` per intercettare lo stato di pausa del replay e decodificare la cronologia degli input avversari.
2. **Modulo 2: Engine del Frame Data (`sf6_frame_engine.py`)**  
   Il "cervello" logico del tool. Contiene un database ottimizzato mappato su tabelle hash (O(1)) con l'intero roster di gioco. Converte i frame di svantaggio e le finestre di *link/cancel* in sequenze temporali ritmiche (`combo_beats`).
3. **Modulo 3: Overlay Multi-Flash (`sf6_metronome_overlay.py`)**  
   Un'interfaccia grafica trasparente sviluppata in `PyQt6`. Sfrutta le proprietà *Always-on-Top* e *Click-Through* (il mouse e i comandi passano attraverso senza rubare il focus al gioco). Sincronizza i cerchi concentrici geometrici a un impulso acustico percussivo ad alta reattività (**"AH!"** via `winsound`).
4. **Pannello di Calibrazione (`sf6_settings_ui.py`)**  
   Una GUI di configurazione che permette all'utente di calibrare i pixel delle ROI in base alla propria risoluzione (1080p, 2K, 4K) e regolare uno slider di *Lag Compensation* per sincronizzare monitor e schede audio.

---

## 🛠️ Funzionalità Principali (Stile Tekken 8)

* **Block Punish Hint:** Identifica una mossa pesante bloccata, calcola lo svantaggio e attiva il metronomo indicando la migliore mossa di risposta dal Punish Counter.
* **Gap Interrupt Hint:** Rileva le stringhe nate da un *Drive Rush* avversario, calcola l'estensione dei frame positivi e segnala la finestra esatta per interrompere la pressione con un tasto da 4 frame o un *Perfect Parry*.
* **Multi-Flash Combo Trainer:** Non si ferma al primo colpo. Guida le dita del giocatore lungo tutta l'estensione ritmica di una combo (es. *Normal ➔ Link ➔ Special Cancel ➔ Super Art*).
* **Burnout Emergency Protocol:** Se rileva lo stato di *Burnout*, esclude automaticamente le mosse OD dal suggerimento e rimodula l'overlay per mostrare unicamente le opzioni di sopravvivenza valide (Super invincibili o salto per i command grab).

---

## 📂 Struttura della Repository

```text
SF6_Final_Takeover_Tool/
├── main.py                     # Entry point dell'applicazione ed orchestratore dei moduli
├── sf6_cv_monitor.py           # Modulo di Computer Vision (Cattura & Template Matching)
├── sf6_frame_engine.py         # Motore logico e Database omnicomprensivo del roster
├── sf6_metronome_overlay.py    # UI dell'Overlay trasparente (Metronomo grafico e audio)
├── sf6_settings_ui.py          # GUI del Pannello Impostazioni per calibrazione lag e ROI
├── sf6_tool_config.json        # File di configurazione locale generato dall'utente
└── templates/                  # Screenshot campione per il riconoscimento d'immagine (.png)
    ├── pause_indicator.png     # Template di riferimento della scritta "Pausa"
    └── ken_shoryuken_log.png   # Template di riferimento dell'input log (es. Ken)
```

---

## 🚀 Installazione e Utilizzo

### Pre-requisiti
Assicurarsi di avere installato un ambiente Python (versione 3.10 o superiore) su sistema operativo Windows.

1. **Clonare la repository:**
   ```bash
   git clone https://github.com
   cd SF6_Final_Takeover_Tool
   ```

2. **Installare le dipendenze:**
   ```bash
   pip install opencv-python numpy mss PyQt6
   ```

3. **Configurare i Template grafici:**
   Avviare *Street Fighter 6*, entrare nella modalità replay, mettere in pausa e scattare uno screenshot. Ritagliare l'area esatta della scritta "PAUSA" e salvarla all'interno della cartella `templates/` con il nome `pause_indicator.png`. Fare lo stesso per le icone della cronologia input laterale che si desidera mappare.

4. **Eseguire l'applicazione:**
   ```bash
   python main.py
   ```
   All'avvio, il Pannello Impostazioni permetterà di verificare le coordinate delle ROI in pixel e calibrare il lag. Cliccando su **Salva e Avvia**, la GUI si chiuderà e il tool si metterà in ascolto in background pronto ad attivarsi ad ogni Replay Takeover.

---

## 🤝 Contributi

I contributi sono caldamente benvenuti! Se desideri mappare nuove stringhe di combo, aggiornare il frame data del database nel Modulo 2 per i nuovi personaggi dei Season Pass, o migliorare gli algoritmi di stabilità della Computer Vision, sentiti libero di aprire una **Pull Request** o segnalare un **Issue**.

---

## 📝 Licenza

Questo progetto è rilasciato sotto i termini della licenza MIT. Consultare il file `LICENSE` per ulteriori dettagli.

*Disclaimer: Questo è un tool esterno a scopo puramente didattico volto a migliorare l'apprendimento del gioco offline. Non interagisce con i file di gioco, non modifica la memoria del processo di Street Fighter 6 e non invia input automatici.*
