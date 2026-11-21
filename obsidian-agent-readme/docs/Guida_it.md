# Obsidian-Agent – Guida

Obsidian-Agent supporta la gestione dei progetti con Obsidian secondo **HERMES 2022** e **SAFe**. Funziona localmente sul computer; l'interfaccia si apre nel browser all'indirizzo `http://127.0.0.1:5055`.

**Principio:** i file Markdown del vault Obsidian sono l'unica fonte di verità. L'agente crea note, legge il vault e tiene una banca dati (`planer.db`) come indice. Si può lavorare direttamente in Obsidian in qualsiasi momento; alla sincronizzazione successiva tutto viene riletto.

Lingue: tedesco, inglese, francese, italiano, spagnolo, rumeno. La lingua si sceglie durante l'installazione e in qualsiasi momento sotto *Impostazioni*.

---

## 1. Installazione su Windows

1. Avviare `Obsidian-Agent-Setup-2.0.0.exe`.
   Se compare *Windows ha protetto il PC*, fare clic su **Ulteriori informazioni** e poi su **Esegui comunque**. Il programma di installazione non è firmato digitalmente.
2. Scegliere la lingua. L'installazione **non richiede diritti di amministratore** e avviene in `%LOCALAPPDATA%\Programs\Obsidian-Agent`.
3. Dopo la copia si apre una finestra di console che completa la configurazione:
   - Viene cercato Python 3.10 o più recente. Se manca, Python 3.12 viene installato con `winget`. Se non riesce, si apre python.org (durante l'installazione spuntare *Add python.exe to PATH*) e poi si riavvia il programma di installazione.
   - Nella finestra di dialogo scegliere la **cartella del vault Obsidian**.
   - Vengono create due attività pianificate: *Obsidian-Agent Weboberflaeche* (avvio in background a ogni accesso) e *Obsidian-Agent Tages-Sync* (ogni giorno alle 04:00, le esecuzioni mancate vengono recuperate).
   - Il browser si apre con l'interfaccia. Premere Invio per chiudere la finestra.
4. In seguito aprirlo dal menu Start o dal collegamento **Obsidian-Agent** sul desktop.

**Aggiornamento:** installare semplicemente il nuovo file di setup sopra quello vecchio. `config.json`, `planer.db` e l'ambiente Python vengono mantenuti.

**Disinstallazione:** *Impostazioni > App > Obsidian-Agent > Disinstalla* oppure menu Start > *Disinstallare Obsidian-Agent*. Le attività pianificate vengono rimosse, `config.json` e `planer.db` vengono salvati nella cartella `obsidian-agent-backup_<data>` sul desktop. **Il vault non viene modificato.**

Protocollo: `%LOCALAPPDATA%\obsidian-agent\gui.log`

## 2. Installazione su macOS

1. Aprire `Obsidian-Agent-2.0.0.pkg`.
   Il pacchetto non è notarizzato. Se macOS segnala che *non può essere aperto*: clic destro sul file > **Apri** > **Apri**. Nelle versioni recenti di macOS aprire *Impostazioni di Sistema > Privacy e sicurezza* e scegliere **Apri comunque**.
2. Seguire i passi del programma di installazione. L'app viene installata in **Applicazioni**.
3. Alla fine dell'installazione una finestra chiede la **cartella del vault Obsidian**. La lingua segue la lingua di sistema del Mac (modificabile sotto *Impostazioni*).
4. È necessario **Python 3.9 o più recente**. Se manca, compare un avviso e si apre python.org. Dopo aver installato Python, aprire semplicemente l'app **Obsidian-Agent**; la configurazione si svolge nel Terminale.
5. Da quel momento l'agente si avvia in background a ogni accesso e la sincronizzazione quotidiana avviene alle 04:00 (launchd, anche senza interfaccia aperta).

Aprire: l'app **Obsidian-Agent** in Applicazioni o nel Launchpad.

**Autorizzazioni delle cartelle:** se il vault si trova in *Documenti*, sulla *Scrivania* o in *iCloud Drive*, macOS può bloccare l'accesso in background. La configurazione lo rileva e indica il file da aggiungere in *Impostazioni di Sistema > Privacy e sicurezza > Accesso completo al disco*.

Dati del programma: `~/Library/Application Support/Obsidian-Agent` · Protocolli: `~/Library/Logs/obsidian-agent`

**Aggiornamento:** installare il nuovo pacchetto e aprire l'app una volta. Impostazioni e banca dati vengono mantenute.

**Disinstallazione:** fare doppio clic su `~/Library/Application Support/Obsidian-Agent/mac/uninstall.command` (nel Finder con *Vai > Vai alla cartella*). Poi trascinare l'app *Obsidian-Agent* da *Applicazioni* nel cestino. Il backup viene salvato sulla scrivania.

## 3. Linux

```bash
./start.sh                      # configura l'ambiente Python e avvia l'interfaccia
python3 agent.py sprache it     # impostare la lingua (de, en, fr, it, es, ro)
```
Sincronizzazione quotidiana con `crontab -e`:
`0 4 * * * cd /percorso/obsidian-agent && .venv/bin/python agent.py sync --geplant`

---

## 4. Primi passi

1. **Impostazioni:** verificare lingua e percorso del vault.
2. **Progetti > Nuovo progetto:** inserire un nome e scegliere un metodo:
   | Metodo | Cosa viene creato |
   |---|---|
   | Senza metodo | Struttura di cartelle e panoramica del progetto |
   | HERMES classico | in più 6 pietre miliari: liberazione dell'inizializzazione del progetto, liberazione dell'esecuzione, liberazione della fase Realizzazione, liberazione della fase Introduzione, liberazione della fase Chiusura, chiusura del progetto |
   | HERMES agile | in più 4 pietre miliari: liberazione dell'inizializzazione del progetto, liberazione dell'esecuzione, liberazione della fase Chiusura, chiusura del progetto |
   | SAFe | Struttura di cartelle; i Planning Interval si creano sotto *SAFe* |
   | HERMES agile + SAFe | Conduzione HERMES, l'attuazione avviene in PI |
3. **HERMES:** impostare le date pianificate delle pietre miliari.
4. Lavorare in Obsidian. **Aggiorna ora** legge subito il vault, altrimenti ogni giorno alle 04:00.

Il metodo si può cambiare in qualsiasi momento sotto *Progetti* o nel *Cockpit*. Cartelle e pietre miliari mancanti vengono aggiunte; nulla viene sovrascritto o eliminato.

## 5. Navigazione

| Area | Contenuto |
|---|---|
| **Oggi** | Task di oggi e in ritardo, pietre miliari dei prossimi giorni, rischi alti |
| **Task** | Tutti i task per gruppo, cambiare stato, spostare di un giorno, filtrare per progetto |
| **Progetti** | Creare progetti con metodo, cambiare il metodo, completare la struttura |
| **Cockpit** | Tutti i progetti con metodo: fase, prossima pietra miliare, PI e prevedibilità, rischi, decisioni, bloccanti |
| **HERMES** | Modello delle fasi, pietre miliari con date pianificate/effettive e stato, liberazioni, pietre miliari supplementari, documenti HERMES |
| **SAFe** | Planning Interval, obiettivi PI con valore di business, prevedibilità, ART Planning Board, Inspect & Adapt, retrospettive, ART Sync |
| **Rischi** | Bacheca ROAM con matrice dei rischi, misura come task, righe contrassegnate nelle note |
| **Decisioni** | Registro delle decisioni da note di decisione, decisioni tecniche (ADR) e righe con `#entscheid` |
| **Riunione periodica** | Serie di sedute con prossima data e pendenze; ogni nuova seduta riprende le pendenze aperte |
| **Documenti** | Nuova nota da qualsiasi modello della cartella dei modelli, anche da quelli propri |
| **Team e stakeholder** | Persone con ruolo, influenza, interesse e i loro task aperti |
| **Ricerca** | Ricerca a testo pieno nell'intero vault |
| **Impostazioni** | Lingua, vault, nomi delle cartelle, ora di sincronizzazione, protocollo |

Ogni titolo è un link `obsidian://` che apre la nota direttamente in Obsidian.

## 6. Struttura del progetto

```
Progetti/<Progetto>/
├── _Progetto_<Progetto>.md        Panoramica con tabelle Dataview
├── 01_Mandato_di_progetto/        Mandati di inizializzazione e di esecuzione, business case
├── 02_Pianificazione/             Piano di gestione del progetto, scadenze, risorse, costi
├── 03_Conduzione/                 Pietre miliari, rapporti sullo stato, richieste di modifica, PI
│   ├── Pietre_miliari/
│   └── PI/<PI>/                   Nota PI, ART Planning Board, obiettivi, retro
├── 04_Rischi_e_decisioni/
│   ├── Rischi/
│   └── Decisioni/
├── 05_Riunioni/                   Comitato di progetto, sedute di progetto, workshop
├── 06_Risultati/                  Concetti, specifiche, test, collaudi
├── 07_Stakeholder/
└── 99_Archivio/
```
I nomi delle cartelle seguono la lingua scelta durante la configurazione. Un cambio di lingua successivo non rinomina le cartelle esistenti.

Altre cartelle del vault: `Task/` (task senza progetto), `Riunioni_periodiche/`, `Team/`, `Pianificatore/` (panoramica del giorno, cockpit GP, registro delle decisioni), `_Modelli/`, `SAFe/` (PI senza progetto).

## 7. HERMES

- **Fasi:** classico inizializzazione, concetto, realizzazione, introduzione, chiusura; agile inizializzazione, attuazione, chiusura.
- **Pietre miliari:** ogni pietra miliare è una nota con punti di controllo secondo HERMES 2022 da spuntare. Sono risultati tipici; adattarli al proprio scenario.
- **Fase attuale:** deriva dalla prima pietra miliare principale aperta. Gli stati *raggiunto*, *liberato* e *soppresso* valgono come evasi.
- **Liberare:** imposta stato e data effettiva e, se desiderato, crea una decisione nel registro delle decisioni collegata alla pietra miliare.
- **Pietre miliari supplementari:** p. es. liberazioni di release, architettura della soluzione, concetto SIPD, collaudo provvisorio, messa in esercizio.
- **Documenti:** piano di gestione del progetto, rapporto sullo stato del progetto, richiesta di modifica, verbale del comitato di progetto e valutazione finale del progetto vengono precompilati con fase attuale, pietre miliari, rischi aperti e decisioni degli ultimi 30 giorni.

## 8. SAFe

- **Planning Interval:** nome (p. es. `2027-PI1`), inizio, numero di iterazioni e settimane per iterazione. L'ultima iterazione è l'iterazione IP (Innovation & Planning).
- **ART Planning Board:** viene creato un file canvas di Obsidian con una colonna per iterazione e una riga per team. Trascinare le feature come schede e collegare le dipendenze con frecce.
- **Obiettivi PI:** per team, *committed* o *uncommitted*, valore di business pianificato (da 1 a 10) e raggiunto.
- **Prevedibilità:** valore di business raggiunto da tutti gli obiettivi diviso per il valore di business pianificato degli obiettivi committed. Intervallo obiettivo da 80 a 100 %.
- **Inspect & Adapt:** con un clic alla fine del PI; la nota contiene la tabella di prevedibilità attuale e un modello per il workshop di problem solving.

## 9. Rischi, decisioni e contrassegni

**ROAM:** *Resolved* (non è più un rischio), *Owned* (una persona se ne occupa), *Accepted* (accettato consapevolmente), *Mitigated* (le misure riducono il rischio). La matrice mostra probabilità e impatto. *Creare task* trasforma la misura in un task e porta un rischio aperto a *Owned*.

**Contrassegni in qualsiasi nota:** una riga con `#entscheid` (decisione), `#risiko` (rischio) o `#blocker` (bloccante) viene rilevata alla sincronizzazione (non nei blocchi di codice). I contrassegni sono uguali in tutte le lingue.

```markdown
## Decisioni
- #entscheid Il budget per la fase 2 è stato liberato.
## Rischi
- #risiko Il fornitore X segnala ritardi -> [[Max Muster]] chiarisce.
- #blocker L'ambiente di test manca da due settimane.
```

La sincronizzazione crea `Pianificatore/Cockpit-GP.md` e `Pianificatore/Registro-decisioni.md`, per vedere la panoramica anche in Obsidian e sul telefono.

## 10. Campi del frontmatter

Nomi e valori dei campi del frontmatter sono **uguali in tutte le lingue** (identificatori tedeschi). Così un vault resta intercambiabile tra le lingue e le query Dataview funzionano ovunque.

| typ | campi importanti |
|---|---|
| `task` | `projekt` (progetto), `start`, `ende` (fine), `status` (offen, in_arbeit, wartend, erledigt), `prioritaet` (hoch, mittel, niedrig), `verantwortlich` (responsabile) |
| `meilenstein` (pietra miliare) | `projekt`, `phase`, `plan` (pianificato), `ist` (effettivo), `status` (geplant, gefaehrdet, verschoben, erreicht, freigegeben, entfallen), `kern` (principale: ja/nein) |
| `risiko` (rischio) | `projekt`, `pi`, `wahrscheinlichkeit` (probabilità), `auswirkung` (impatto), `roam` (offen, owned, mitigated, accepted, resolved), `verantwortlich`, `faellig` (scadenza) |
| `entscheid` (decisione) | `projekt`, `datum`, `gremium` (organo), `entscheid_durch` (deciso da), `status` (beantragt, entschieden, zurueckgestellt, abgelehnt) |
| `pi` | `start`, `ende`, `iterationen`, `iteration_wochen`, `art`, `status` |
| `pi_objective` | `pi`, `team`, `committed` (ja/nein), `bv_geplant` (BV pianificato), `bv_ist` (BV raggiunto) |

È un task ogni file `Task_*.md` o ogni nota con `typ: task`. Senza `status` un task vale come evaso quando tutte le caselle sono spuntate.

## 11. Plugin Obsidian consigliati

- **Dataview** (plugin della community): mostra le tabelle nella panoramica del progetto, nella nota PI e in Inspect & Adapt.
- Facoltativi **Tasks**, **Templater** e **Kanban**. L'agente non ne ha bisogno.

## 12. Adattare i modelli

Tutti i modelli si trovano in `_Modelli/` nel vault e si possono modificare in Obsidian; l'agente usa poi la versione modificata. I modelli eliminati vengono ricreati alla sincronizzazione successiva. I modelli propri compaiono sotto *Documenti*. Con `zielordner: 05_Riunioni` nel frontmatter si definisce la cartella preselezionata. Segnaposto: `{{title}}`, `{{date}}`, `{{projekt}}`.

## 13. Riga di comando

```
python agent.py sync                       leggere il vault ora
python agent.py heute                      mostrare i task di oggi
python agent.py projekt "Nome" --methode hermes_klassisch   (hermes_agil, safe, hermes_safe)
python agent.py sprache it                 impostare la lingua
```

## 14. Risoluzione dei problemi

| Problema | Soluzione |
|---|---|
| Nulla all'indirizzo 127.0.0.1:5055 | Windows: menu Start > *Obsidian-Agent*. macOS: aprire l'app *Obsidian-Agent*. Controllare il protocollo. |
| Vault non trovato | *Impostazioni* > correggere il percorso del vault Obsidian |
| La sincronizzazione in background su macOS non funziona | Concedere l'accesso completo al disco al file Python indicato (vedi sezione 2) |
| Le tabelle Dataview sono vuote | Installare e attivare il plugin della community Dataview in Obsidian |
| La porta 5055 è occupata | Modificare il valore `port` in `config.json` e rieseguire la configurazione |
