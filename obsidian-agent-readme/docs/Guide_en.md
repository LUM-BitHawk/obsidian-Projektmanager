# Obsidian-Agent – Guide

Obsidian-Agent supports project management with Obsidian according to **HERMES 2022** and **SAFe**. It runs locally on your computer; you open the interface in the browser at `http://127.0.0.1:5055`.

**Principle:** The Markdown files in your Obsidian vault are the single source of truth. The agent creates notes, reads the vault and keeps a database (`planer.db`) as an index. You can work directly in Obsidian at any time; everything is read again at the next sync.

Languages: German, English, French, Italian, Spanish, Romanian. You choose the language during installation and at any time under *Settings*.

---

## 1. Installation on Windows

1. Run `Obsidian-Agent-Setup-2.0.0.exe`.
   If *Windows protected your PC* appears, click **More info** and then **Run anyway**. The installer is not digitally signed.
2. Choose the language. The installation needs **no administrator rights** and installs to `%LOCALAPPDATA%\Programs\Obsidian-Agent`.
3. After copying, a console window opens and completes the setup:
   - Python 3.10 or newer is searched for. If it is missing, Python 3.12 is installed with `winget`. If that fails, python.org opens (tick *Add python.exe to PATH* during installation) and you run the installer again afterwards.
   - Choose the **folder of your Obsidian vault** in the folder dialog.
   - Two scheduled tasks are created: *Obsidian-Agent Weboberflaeche* (starts in the background at every login) and *Obsidian-Agent Tages-Sync* (daily at 04:00, missed runs are caught up).
   - The browser opens with the interface. Press Enter to close the window.
4. Later, open it from the Start menu or the **Obsidian-Agent** shortcut on the desktop.

**Update:** Simply install the new setup file over the old one. `config.json`, `planer.db` and the Python environment are kept.

**Uninstall:** *Settings > Apps > Obsidian-Agent > Uninstall* or Start menu > *Uninstall Obsidian-Agent*. The scheduled tasks are removed, `config.json` and `planer.db` are backed up to the folder `obsidian-agent-backup_<date>` on the desktop. **Your vault is not changed.**

Log: `%LOCALAPPDATA%\obsidian-agent\gui.log`

## 2. Installation on macOS

1. Open `Obsidian-Agent-2.0.0.pkg`.
   The package is not notarised. If macOS says it *cannot be opened*: right-click the file > **Open** > **Open**. On newer macOS versions use *System Settings > Privacy & Security* and choose **Open Anyway** there.
2. Follow the installer. The app is installed in **Applications**.
3. At the end of the installation a dialog asks for the **folder of your Obsidian vault**. The language follows the system language of the Mac (changeable under *Settings*).
4. **Python 3.9 or newer** is required. If it is missing, a notice appears and python.org opens. After installing Python, simply open the **Obsidian-Agent** app; the setup then runs in Terminal.
5. From then on the agent starts in the background at every login, and the daily sync runs at 04:00 (launchd, even when the interface is not open).

Open: the **Obsidian-Agent** app in Applications or Launchpad.

**Folder permissions:** If the vault is in *Documents*, on the *Desktop* or in *iCloud Drive*, macOS may block background access. The setup detects this and shows the file you need to add under *System Settings > Privacy & Security > Full Disk Access*.

Program data: `~/Library/Application Support/Obsidian-Agent` · Logs: `~/Library/Logs/obsidian-agent`

**Update:** Install the new package and open the app once. Settings and database are kept.

**Uninstall:** Double-click `~/Library/Application Support/Obsidian-Agent/mac/uninstall.command` (in Finder use *Go > Go to Folder*). Then drag the *Obsidian-Agent* app from *Applications* to the Bin. The backup is placed on the desktop.

## 3. Linux

```bash
./start.sh                      # sets up the Python environment and starts the interface
python3 agent.py sprache en     # set the language (de, en, fr, it, es, ro)
```
Daily sync with `crontab -e`:
`0 4 * * * cd /path/obsidian-agent && .venv/bin/python agent.py sync --geplant`

---

## 4. Getting started

1. **Settings:** check the language and vault path.
2. **Projects > New project:** enter a name and choose a method:
   | Method | What is created |
   |---|---|
   | No method | Folder structure and project overview |
   | HERMES classic | plus 6 milestones: project initiation release, execution release, phase release Implementation, phase release Deployment, phase release Closure, project closure |
   | HERMES agile | plus 4 milestones: project initiation release, execution release, phase release Closure, project closure |
   | SAFe | Folder structure; you create planning intervals under *SAFe* |
   | HERMES agile + SAFe | HERMES governance, execution runs in PIs |
3. **HERMES:** set the planned dates of the milestones.
4. Work in Obsidian. **Update now** reads the vault immediately, otherwise daily at 04:00.

The method can be changed at any time under *Projects* or in the *Cockpit*. Missing folders and milestones are added; nothing is overwritten or deleted.

## 5. Navigation

| Area | Content |
|---|---|
| **Today** | Today's and overdue tasks, milestones of the next days, high risks |
| **Tasks** | All tasks by group, change status, move by one day, filter by project |
| **Projects** | Create projects with a method, change the method, complete the structure |
| **Cockpit** | All projects with a method: phase, next milestone, PI and predictability, risks, decisions, blockers |
| **HERMES** | Phase model, milestones with planned/actual dates and status, approvals, additional milestones, HERMES documents |
| **SAFe** | Planning intervals, PI objectives with business value, predictability, ART Planning Board, Inspect & Adapt, retrospectives, ART Sync |
| **Risks** | ROAM board with risk matrix, measure as task, tagged lines from notes |
| **Decisions** | Decision log from decision notes, technical decisions (ADR) and lines tagged `#entscheid` |
| **Recurring meeting** | Meeting series with next date and action items; each new meeting contains the open action items |
| **Documents** | New note from any template in the templates folder, including your own |
| **Team and stakeholders** | People with role, influence, interest and their open tasks |
| **Search** | Full-text search across the whole vault |
| **Settings** | Language, vault, folder names, sync time, sync log |

Every title is an `obsidian://` link and opens the note directly in Obsidian.

## 6. Project structure

```
Projects/<Project>/
├── _Project_<Project>.md          Overview with Dataview tables
├── 01_Project_Mandate/            Initiation and execution order, business case
├── 02_Planning/                   Project management plan, schedule, resources, costs
├── 03_Control/                    Milestones, status reports, change requests, PIs
│   ├── Milestones/
│   └── PI/<PI>/                   PI note, ART Planning Board, objectives, retros
├── 04_Risks_and_Decisions/
│   ├── Risks/
│   └── Decisions/
├── 05_Meetings/                   Steering committee, project meetings, workshops
├── 06_Deliverables/               Concepts, specifications, tests, acceptances
├── 07_Stakeholders/
└── 99_Archive/
```
Folder names follow the language chosen during setup. A later change of language does not rename existing folders.

Other folders in the vault: `Tasks/` (tasks without a project), `Recurring_Meetings/`, `Team/`, `Planner/` (daily overview, PM cockpit, decision log), `_Templates/`, `SAFe/` (PIs without a project).

## 7. HERMES

- **Phases:** classic initiation, concept, implementation, deployment, closure; agile initiation, execution, closure.
- **Milestones:** Each milestone is a note with checkpoints according to HERMES 2022 to tick off. The checkpoints are typical outcomes; adapt them to your scenario.
- **Current phase:** derived from the first open core milestone. The statuses *achieved*, *approved* and *dropped* count as done.
- **Approve:** sets status and actual date and, if desired, creates a decision in the decision log linked to the milestone.
- **Additional milestones:** e.g. release approvals, solution architecture, ISDS concept, preliminary acceptance, start of operation.
- **Documents:** project management plan, project status report, change request, steering committee minutes and project closure assessment are prefilled with the current phase, milestones, open risks and the decisions of the last 30 days.

## 8. SAFe

- **Planning interval:** name (e.g. `2027-PI1`), start, number of iterations and weeks per iteration. The last iteration is the IP iteration (Innovation & Planning).
- **ART Planning Board:** an Obsidian canvas file with one column per iteration and one row per team is created. Drag features as cards into the cells and connect dependencies with arrows.
- **PI objectives:** per team, *committed* or *uncommitted*, planned (1 to 10) and achieved business value.
- **Predictability:** achieved business value of all objectives divided by the planned business value of the committed objectives. Target range 80 to 100 %.
- **Inspect & Adapt:** one click at the end of the PI; the note contains the current predictability table and a template for the problem-solving workshop.

## 9. Risks, decisions and tags

**ROAM:** *Resolved* (no longer a risk), *Owned* (someone takes care of it), *Accepted* (knowingly accepted), *Mitigated* (measures reduce the risk). The matrix shows probability and impact. *Create task* turns the measure into a task and sets an open risk to *Owned*.

**Tags in any note:** A line with `#entscheid` (decision), `#risiko` (risk) or `#blocker` is detected at the sync (not in code blocks). The tags are the same in all languages.

```markdown
## Decisions
- #entscheid Budget for phase 2 has been approved.
## Risks
- #risiko Supplier X reports delays -> [[Max Muster]] is clarifying.
- #blocker Test environment has been missing for two weeks.
```

The sync creates `Planner/PM-Cockpit.md` and `Planner/Decision-Log.md`, so you can see the overview in Obsidian and on your phone as well.

## 10. Frontmatter fields

Field names and values in the frontmatter are **the same in all languages** (they are German identifiers). This keeps a vault interchangeable between languages, and the Dataview queries work everywhere.

| typ | important fields |
|---|---|
| `task` | `projekt` (project), `start`, `ende` (end), `status` (offen, in_arbeit, wartend, erledigt), `prioritaet` (hoch, mittel, niedrig), `verantwortlich` (responsible) |
| `meilenstein` (milestone) | `projekt`, `phase`, `plan`, `ist` (actual), `status` (geplant, gefaehrdet, verschoben, erreicht, freigegeben, entfallen), `kern` (core: ja/nein) |
| `risiko` (risk) | `projekt`, `pi`, `wahrscheinlichkeit` (probability), `auswirkung` (impact), `roam` (offen, owned, mitigated, accepted, resolved), `verantwortlich`, `faellig` (due) |
| `entscheid` (decision) | `projekt`, `datum`, `gremium` (body), `entscheid_durch` (decided by), `status` (beantragt, entschieden, zurueckgestellt, abgelehnt) |
| `pi` | `start`, `ende`, `iterationen`, `iteration_wochen`, `art`, `status` |
| `pi_objective` | `pi`, `team`, `committed` (ja/nein), `bv_geplant` (planned BV), `bv_ist` (achieved BV) |

A task is any file `Task_*.md` or any note with `typ: task`. Without `status`, a task counts as done when all checkboxes are ticked.

## 11. Recommended Obsidian plugins

- **Dataview** (community plugin): shows the tables in the project overview, PI note and Inspect & Adapt.
- Optional **Tasks**, **Templater** and **Kanban**. They are not required by the agent.

## 12. Customising templates

All templates are in `_Templates/` in the vault and can be edited in Obsidian; the agent then uses your version. Deleted templates are recreated at the next sync. Your own templates appear under *Documents*. With `zielordner: 05_Meetings` in the frontmatter you set the preselected folder. Placeholders: `{{title}}`, `{{date}}`, `{{projekt}}`.

## 13. Command line

```
python agent.py sync                       read the vault now
python agent.py heute                      show today's tasks
python agent.py projekt "Name" --methode hermes_klassisch   (hermes_agil, safe, hermes_safe)
python agent.py sprache en                 set the language
```

## 14. Troubleshooting

| Problem | Solution |
|---|---|
| Nothing at 127.0.0.1:5055 | Windows: Start menu > *Obsidian-Agent*. macOS: open the *Obsidian-Agent* app. Check the log. |
| Vault not found | *Settings* > correct the path to the Obsidian vault |
| Background sync on macOS does not run | Grant Full Disk Access to the Python file shown (see section 2) |
| Dataview tables are empty | Install and enable the Dataview community plugin in Obsidian |
| Port 5055 in use | Change the value `port` in `config.json` and run the setup again |
