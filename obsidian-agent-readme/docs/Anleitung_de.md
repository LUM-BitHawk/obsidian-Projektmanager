# Obsidian-Agent – Anleitung

Obsidian-Agent unterstützt Projektmanagement mit Obsidian nach **HERMES 2022** und **SAFe**. Er läuft lokal auf deinem Rechner, die Oberfläche öffnest du im Browser unter `http://127.0.0.1:5055`.

**Grundprinzip:** Die Markdown-Dateien in deinem Obsidian-Vault sind die einzige Wahrheit. Der Agent legt Notizen an, liest den Vault ein und führt daraus eine Datenbank (`planer.db`) als Index. Du kannst jederzeit direkt in Obsidian arbeiten; beim nächsten Sync wird alles wieder eingelesen.

Sprachen: Deutsch, Englisch, Französisch, Italienisch, Spanisch, Rumänisch. Die Sprache wählst du bei der Installation und jederzeit unter *Einstellungen*.

---

## 1. Installation unter Windows

1. `Obsidian-Agent-Setup-2.0.0.exe` starten.
   Erscheint die Meldung *Der Computer wurde durch Windows geschützt*, auf **Weitere Informationen** und dann **Trotzdem ausführen** klicken. Der Installer ist nicht digital signiert.
2. Sprache wählen. Die Installation braucht **keine Administratorrechte** und installiert nach `%LOCALAPPDATA%\Programs\Obsidian-Agent`.
3. Nach dem Kopieren öffnet sich ein Konsolenfenster, das die Einrichtung übernimmt:
   - Python 3.10 oder neuer wird gesucht. Fehlt es, wird Python 3.12 über `winget` installiert. Klappt das nicht, öffnet sich python.org (bei der Installation das Häkchen *Add python.exe to PATH* setzen) und du startest den Installer danach erneut.
   - Im Ordner-Dialog den **Ordner deines Obsidian-Vaults** wählen.
   - Es werden zwei geplante Aufgaben angelegt: *Obsidian-Agent Weboberflaeche* (Start bei jeder Anmeldung im Hintergrund) und *Obsidian-Agent Tages-Sync* (täglich um 04:00, verpasste Läufe werden nachgeholt).
   - Der Browser öffnet sich mit der Oberfläche. Mit Enter schliesst du das Fenster.
4. Öffnen später über das Startmenü oder die Verknüpfung **Obsidian-Agent** auf dem Desktop.

**Aktualisieren:** Neue Setup-Datei einfach darüber installieren. `config.json`, `planer.db` und die Python-Umgebung bleiben erhalten.

**Deinstallieren:** *Einstellungen > Apps > Obsidian-Agent > Deinstallieren* oder Startmenü > *Obsidian-Agent deinstallieren*. Die geplanten Aufgaben werden entfernt, `config.json` und `planer.db` werden in den Ordner `obsidian-agent-backup_<Datum>` auf dem Desktop gesichert. **Dein Vault wird nicht verändert.**

Protokoll: `%LOCALAPPDATA%\obsidian-agent\gui.log`

## 2. Installation unter macOS

1. `Obsidian-Agent-2.0.0.pkg` öffnen.
   Das Paket ist nicht notarisiert. Meldet macOS *kann nicht geöffnet werden*: Rechtsklick auf die Datei > **Öffnen** > **Öffnen**. Unter neueren macOS-Versionen stattdessen *Systemeinstellungen > Datenschutz & Sicherheit* und dort **Dennoch öffnen** wählen.
2. Den Schritten des Installers folgen. Die App wird unter **Programme** installiert.
3. Am Ende der Installation erscheint ein Dialog für den **Ordner deines Obsidian-Vaults**. Die Sprache folgt der Systemsprache des Mac (änderbar unter *Einstellungen*).
4. Voraussetzung ist **Python 3.9 oder neuer**. Fehlt es, erscheint ein Hinweis und python.org öffnet sich. Nach der Installation von Python einfach die App **Obsidian-Agent** öffnen; die Einrichtung läuft dann im Terminal.
5. Danach startet der Agent bei jeder Anmeldung im Hintergrund, der tägliche Sync läuft um 04:00 (launchd, auch ohne geöffnete Oberfläche).

Öffnen: App **Obsidian-Agent** im Ordner Programme oder im Launchpad.

**Hinweis zu Ordnerrechten:** Liegt der Vault in *Dokumente*, auf dem *Schreibtisch* oder in *iCloud Drive*, kann macOS den Zugriff im Hintergrund blockieren. Die Einrichtung erkennt das und zeigt die Datei, die du unter *Systemeinstellungen > Datenschutz & Sicherheit > Festplattenvollzugriff* hinzufügen musst.

Programmdaten: `~/Library/Application Support/Obsidian-Agent` · Protokolle: `~/Library/Logs/obsidian-agent`

**Aktualisieren:** Neues Paket installieren und die App einmal öffnen. Einstellungen und Datenbank bleiben erhalten.

**Deinstallieren:** `~/Library/Application Support/Obsidian-Agent/mac/uninstall.command` per Doppelklick ausführen (im Finder mit *Gehe zu > Gehe zum Ordner*). Danach die App *Obsidian-Agent* aus *Programme* in den Papierkorb ziehen. Die Sicherung landet auf dem Schreibtisch.

## 3. Linux

```bash
./start.sh                      # richtet die Python-Umgebung ein und startet die Oberfläche
python3 agent.py sprache de     # Sprache setzen (de, en, fr, it, es, ro)
```
Täglicher Sync mit `crontab -e`:
`0 4 * * * cd /pfad/obsidian-agent && .venv/bin/python agent.py sync --geplant`

---

## 4. Erste Schritte

1. **Einstellungen:** Sprache und Vault-Pfad prüfen.
2. **Projekte > Neues Projekt:** Name eingeben und Methode wählen:
   | Methode | Was angelegt wird |
   |---|---|
   | Ohne Methode | Ordnerstruktur und Projektübersicht |
   | HERMES klassisch | dazu 6 Meilensteine: Projektinitialisierungsfreigabe, Durchführungsfreigabe, Phasenfreigabe Realisierung, Phasenfreigabe Einführung, Phasenfreigabe Abschluss, Projektabschluss |
   | HERMES agil | dazu 4 Meilensteine: Projektinitialisierungsfreigabe, Durchführungsfreigabe, Phasenfreigabe Abschluss, Projektabschluss |
   | SAFe | Ordnerstruktur, Planning Intervals legst du unter *SAFe* an |
   | HERMES agil + SAFe | HERMES-Steuerung, die Umsetzung läuft in PIs |
3. **HERMES:** Plandaten der Meilensteine setzen.
4. In Obsidian arbeiten. Mit **Jetzt aktualisieren** liest der Agent den Vault sofort ein, sonst täglich um 04:00.

Die Methode lässt sich jederzeit unter *Projekte* oder im *Cockpit* ändern. Fehlende Ordner und Meilensteine werden ergänzt, nichts wird überschrieben oder gelöscht.

## 5. Navigation

| Bereich | Inhalt |
|---|---|
| **Heute** | Heutige und überfällige Tasks, Meilensteine der nächsten Tage, hohe Risiken |
| **Tasks** | Alle Tasks nach Gruppen, Status ändern, um einen Tag verschieben, Filter nach Projekt |
| **Projekte** | Projekte mit Methode anlegen, Methode ändern, Struktur ergänzen |
| **Cockpit** | Alle Projekte mit Methode: Phase, nächster Meilenstein, PI und Predictability, Risiken, Entscheide, Blocker |
| **HERMES** | Phasenmodell, Meilensteine mit Plan/Ist und Status, Freigaben, zusätzliche Meilensteine, HERMES-Dokumente |
| **SAFe** | Planning Intervals, PI Objectives mit Business Value, Predictability, ART Planning Board, Inspect & Adapt, Retrospektiven, ART Sync |
| **Risiken** | ROAM-Board mit Risikomatrix, Massnahme als Task, markierte Zeilen aus Notizen |
| **Entscheide** | Entscheid-Log aus Entscheid-Notizen, technischen Entscheidungen (ADR) und Zeilen mit `#entscheid` |
| **Jour Fixe** | Sitzungsserien mit nächstem Termin und Pendenzen; jede neue Sitzung enthält die offenen Pendenzen |
| **Dokumente** | Neue Notiz aus jeder Vorlage im Vorlagen-Ordner, auch aus eigenen |
| **Team und Stakeholder** | Personen mit Rolle, Einfluss, Interesse und ihren offenen Tasks |
| **Suche** | Volltextsuche über den ganzen Vault |
| **Einstellungen** | Sprache, Vault, Ordnernamen, Sync-Zeit, Sync-Protokoll |

Jeder Titel ist ein `obsidian://`-Link und öffnet die Notiz direkt in Obsidian.

## 6. Projektstruktur

```
Projekte/<Projekt>/
├── _Projekt_<Projekt>.md          Übersicht mit Dataview-Tabellen
├── 01_Projektauftrag/             Initialisierungs- und Durchführungsauftrag, Business Case
├── 02_Planung/                    Projektmanagementplan, Termine, Ressourcen, Kosten
├── 03_Steuerung/                  Meilensteine, Statusberichte, Änderungsanträge, PIs
│   ├── Meilensteine/
│   └── PI/<PI>/                   PI-Notiz, ART Planning Board, Objectives, Retros
├── 04_Risiken_und_Entscheide/
│   ├── Risiken/
│   └── Entscheide/
├── 05_Meetings/                   Projektausschuss, Projektsitzungen, Workshops
├── 06_Ergebnisse/                 Konzepte, Spezifikationen, Tests, Abnahmen
├── 07_Stakeholder/
└── 99_Archiv/
```
Die Ordnernamen folgen der Sprache, die bei der Einrichtung gewählt wurde. Ein späterer Sprachwechsel benennt bestehende Ordner nicht um.

Weitere Ordner im Vault: `Tasks/` (Tasks ohne Projekt), `JourFixe/`, `Team/`, `Planer/` (Tagesübersicht, PM-Cockpit, Entscheid-Log), `_Vorlagen/`, `SAFe/` (PIs ohne Projekt).

## 7. HERMES

- **Phasen:** klassisch Initialisierung, Konzept, Realisierung, Einführung, Abschluss; agil Initialisierung, Umsetzung, Abschluss.
- **Meilensteine:** Jeder Meilenstein ist eine Notiz mit Prüfpunkten nach HERMES 2022 zum Abhaken. Die Prüfpunkte sind typische Ergebnisse; passe sie an dein Szenario an.
- **Aktuelle Phase:** ergibt sich aus dem ersten offenen Kern-Meilenstein. Die Status *erreicht*, *freigegeben* und *entfallen* gelten als erledigt.
- **Freigeben:** setzt Status und Ist-Datum und legt auf Wunsch einen Entscheid im Entscheid-Log an, der mit dem Meilenstein verlinkt ist.
- **Zusätzliche Meilensteine:** z.B. Releasefreigaben, Lösungsarchitektur, ISDS-Konzept, Vorabnahme, Betriebsaufnahme.
- **Dokumente:** Projektmanagementplan, Projektstatusbericht, Änderungsantrag, Projektausschuss-Protokoll und Projektschlussbeurteilung werden mit aktueller Phase, Meilensteinen, offenen Risiken und den Entscheiden der letzten 30 Tage vorbefüllt.

## 8. SAFe

- **Planning Interval:** Name (z.B. `2027-PI1`), Start, Anzahl Iterationen und Wochen pro Iteration. Die letzte Iteration ist die IP-Iteration (Innovation & Planning).
- **ART Planning Board:** Beim Anlegen entsteht eine Obsidian-Canvas-Datei mit Spalten je Iteration und Zeilen je Team. Features als Karten in die Felder ziehen, Abhängigkeiten mit Pfeilen verbinden.
- **PI Objectives:** je Team, *committed* oder *uncommitted*, Business Value geplant (1 bis 10) und erreicht.
- **Predictability:** erreichter Business Value aller Objectives geteilt durch den geplanten Business Value der committed Objectives. Zielbereich 80 bis 100 %.
- **Inspect & Adapt:** per Knopf am Ende des PI; die Notiz enthält die aktuelle Predictability-Tabelle und eine Vorlage für den Problem-Solving-Workshop.

## 9. Risiken, Entscheide und Markierungen

**ROAM:** *Resolved* (kein Risiko mehr), *Owned* (eine Person kümmert sich darum), *Accepted* (bewusst in Kauf genommen), *Mitigated* (Massnahmen reduzieren das Risiko). Die Matrix zeigt Wahrscheinlichkeit und Auswirkung. *Task erstellen* macht aus der Massnahme einen Task und setzt ein offenes Risiko auf *Owned*.

**Markierungen in beliebigen Notizen:** Eine Zeile mit `#entscheid`, `#risiko` oder `#blocker` wird beim Sync erkannt (nicht in Code-Blöcken). Die Tags sind in allen Sprachen gleich.

```markdown
## Entscheide
- #entscheid Budget für Phase 2 wurde freigegeben.
## Risiken
- #risiko Lieferant X meldet Verzögerungen -> [[Max Muster]] klärt das ab.
- #blocker Testumgebung fehlt seit zwei Wochen.
```

Beim Sync entstehen `Planer/PM-Cockpit.md` und `Planer/Entscheid-Log.md`, damit du die Übersicht auch in Obsidian und auf dem Handy siehst.

## 10. Felder im Frontmatter

Die Feldnamen und Werte im Frontmatter sind **in allen Sprachen gleich**. So bleibt ein Vault zwischen Sprachen austauschbar und die Dataview-Abfragen funktionieren überall.

| typ | wichtige Felder |
|---|---|
| `task` | `projekt`, `start`, `ende`, `status` (offen, in_arbeit, wartend, erledigt), `prioritaet` (hoch, mittel, niedrig), `verantwortlich` |
| `meilenstein` | `projekt`, `phase`, `plan`, `ist`, `status` (geplant, gefaehrdet, verschoben, erreicht, freigegeben, entfallen), `kern` (ja/nein) |
| `risiko` | `projekt`, `pi`, `wahrscheinlichkeit`, `auswirkung`, `roam` (offen, owned, mitigated, accepted, resolved), `verantwortlich`, `faellig` |
| `entscheid` | `projekt`, `datum`, `gremium`, `entscheid_durch`, `status` (beantragt, entschieden, zurueckgestellt, abgelehnt) |
| `pi` | `start`, `ende`, `iterationen`, `iteration_wochen`, `art`, `status` |
| `pi_objective` | `pi`, `team`, `committed` (ja/nein), `bv_geplant`, `bv_ist` |

Als Task gilt jede Datei `Task_*.md` oder jede Notiz mit `typ: task`. Ohne `status` gilt ein Task als erledigt, wenn alle Checkboxen abgehakt sind.

## 11. Empfohlene Obsidian-Plugins

- **Dataview** (Community-Plugin): zeigt die Tabellen in Projektübersicht, PI-Notiz und Inspect & Adapt.
- Optional **Tasks**, **Templater** und **Kanban**. Für den Agent sind sie nicht nötig.

## 12. Vorlagen anpassen

Alle Vorlagen liegen in `_Vorlagen/` im Vault und lassen sich in Obsidian ändern; der Agent nutzt danach deine Version. Gelöschte Vorlagen werden beim nächsten Sync neu angelegt. Eigene Vorlagen erscheinen unter *Dokumente*. Mit `zielordner: 05_Meetings` im Frontmatter bestimmst du den vorgewählten Ordner. Platzhalter: `{{title}}`, `{{date}}`, `{{projekt}}`.

## 13. Kommandozeile

```
python agent.py sync                       Vault jetzt einlesen
python agent.py heute                      Heutige Tasks anzeigen
python agent.py projekt "Name" --methode hermes_klassisch   (hermes_agil, safe, hermes_safe)
python agent.py sprache fr                 Sprache setzen
```

## 14. Fehlerbehebung

| Problem | Lösung |
|---|---|
| Browser zeigt nichts unter 127.0.0.1:5055 | Windows: Startmenü > *Obsidian-Agent*. macOS: App *Obsidian-Agent* öffnen. Protokoll prüfen. |
| Vault nicht gefunden | *Einstellungen* > Pfad zum Obsidian-Vault korrigieren |
| macOS-Sync im Hintergrund läuft nicht | Festplattenvollzugriff für die angezeigte Python-Datei erteilen (siehe Abschnitt 2) |
| Dataview-Tabellen sind leer | Community-Plugin Dataview in Obsidian installieren und aktivieren |
| Port 5055 belegt | In `config.json` den Wert `port` ändern und die Einrichtung erneut ausführen |
