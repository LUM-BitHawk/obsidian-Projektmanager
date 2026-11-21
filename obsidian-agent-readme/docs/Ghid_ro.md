# Obsidian-Agent – Ghid de utilizare

Obsidian-Agent sprijină managementul proiectelor cu Obsidian conform **HERMES 2022** și **SAFe**. Rulează local pe calculator; interfața se deschide în browser la `http://127.0.0.1:5055`.

**Principiu:** fișierele Markdown din seiful Obsidian sunt singura sursă de adevăr. Agentul creează note, citește seiful și ține o bază de date (`planer.db`) ca index. Puteți lucra direct în Obsidian oricând; la următoarea sincronizare totul este citit din nou.

Limbi: germană, engleză, franceză, italiană, spaniolă, română. Limba se alege la instalare și oricând în *Setări*.

---

## 1. Instalare pe Windows

1. Porniți `Obsidian-Agent-Setup-2.0.0.exe`.
   Dacă apare *Windows v-a protejat PC-ul*, faceți clic pe **Mai multe informații** și apoi pe **Executare oricum**. Programul de instalare nu este semnat digital.
2. Alegeți limba. Instalarea **nu necesită drepturi de administrator** și se face în `%LOCALAPPDATA%\Programs\Obsidian-Agent`.
3. După copiere se deschide o fereastră de consolă care finalizează configurarea:
   - Se caută Python 3.10 sau mai nou. Dacă lipsește, Python 3.12 este instalat cu `winget`. Dacă nu reușește, se deschide python.org (la instalare bifați *Add python.exe to PATH*) și apoi porniți din nou programul de instalare.
   - În dialog alegeți **dosarul seifului Obsidian**.
   - Se creează două sarcini programate: *Obsidian-Agent Weboberflaeche* (pornire în fundal la fiecare autentificare) și *Obsidian-Agent Tages-Sync* (zilnic la 04:00; rulările ratate sunt recuperate).
   - Browserul se deschide cu interfața. Apăsați Enter pentru a închide fereastra.
4. Ulterior deschideți-l din meniul Start sau din scurtătura **Obsidian-Agent** de pe desktop.

**Actualizare:** instalați pur și simplu noul fișier de instalare peste cel vechi. `config.json`, `planer.db` și mediul Python se păstrează.

**Dezinstalare:** *Setări > Aplicații > Obsidian-Agent > Dezinstalare* sau meniul Start > *Dezinstalează Obsidian-Agent*. Sarcinile programate sunt eliminate, iar `config.json` și `planer.db` sunt salvate în dosarul `obsidian-agent-backup_<dată>` de pe desktop. **Seiful nu este modificat.**

Jurnal: `%LOCALAPPDATA%\obsidian-agent\gui.log`

## 2. Instalare pe macOS

1. Deschideți `Obsidian-Agent-2.0.0.pkg`.
   Pachetul nu este notarizat. Dacă macOS afișează că *nu poate fi deschis*: clic dreapta pe fișier > **Deschide** > **Deschide**. Pe versiunile mai noi de macOS mergeți la *Setări de sistem > Confidențialitate și securitate* și alegeți **Deschide oricum**.
2. Urmați pașii programului de instalare. Aplicația este instalată în **Aplicații**.
3. La sfârșitul instalării, un dialog cere **dosarul seifului Obsidian**. Limba urmează limba sistemului Mac (se poate schimba în *Setări*).
4. Este necesar **Python 3.9 sau mai nou**. Dacă lipsește, apare un mesaj și se deschide python.org. După instalarea Python, deschideți pur și simplu aplicația **Obsidian-Agent**; configurarea rulează apoi în Terminal.
5. De atunci agentul pornește în fundal la fiecare autentificare, iar sincronizarea zilnică rulează la 04:00 (launchd, chiar și fără interfața deschisă).

Deschidere: aplicația **Obsidian-Agent** din Aplicații sau din Launchpad.

**Permisiuni pentru dosare:** dacă seiful se află în *Documente*, pe *Birou* sau în *iCloud Drive*, macOS poate bloca accesul în fundal. Configurarea detectează acest lucru și arată fișierul pe care trebuie să îl adăugați în *Setări de sistem > Confidențialitate și securitate > Acces complet la disc*.

Datele programului: `~/Library/Application Support/Obsidian-Agent` · Jurnale: `~/Library/Logs/obsidian-agent`

**Actualizare:** instalați noul pachet și deschideți aplicația o dată. Setările și baza de date se păstrează.

**Dezinstalare:** faceți dublu clic pe `~/Library/Application Support/Obsidian-Agent/mac/uninstall.command` (în Finder cu *Mergi > Mergi la dosar*). Apoi trageți aplicația *Obsidian-Agent* din *Aplicații* în coșul de gunoi. Copia de rezervă este salvată pe birou.

## 3. Linux

```bash
./start.sh                      # configurează mediul Python și pornește interfața
python3 agent.py sprache ro     # setarea limbii (de, en, fr, it, es, ro)
```
Sincronizare zilnică cu `crontab -e`:
`0 4 * * * cd /cale/obsidian-agent && .venv/bin/python agent.py sync --geplant`

---

## 4. Primii pași

1. **Setări:** verificați limba și calea seifului.
2. **Proiecte > Proiect nou:** introduceți un nume și alegeți o metodă:
   | Metodă | Ce se creează |
   |---|---|
   | Fără metodă | Structura de dosare și prezentarea proiectului |
   | HERMES clasic | în plus 6 jaloane: aprobarea inițializării proiectului, aprobarea execuției, aprobarea fazei Realizare, aprobarea fazei Implementare, aprobarea fazei Încheiere, încheierea proiectului |
   | HERMES agil | în plus 4 jaloane: aprobarea inițializării proiectului, aprobarea execuției, aprobarea fazei Încheiere, încheierea proiectului |
   | SAFe | Structura de dosare; Planning Intervals se creează la *SAFe* |
   | HERMES agil + SAFe | Coordonare HERMES, execuția are loc în PI |
3. **HERMES:** setați datele planificate ale jaloanelor.
4. Lucrați în Obsidian. **Actualizează acum** citește seiful imediat; altfel zilnic la 04:00.

Metoda poate fi schimbată oricând la *Proiecte* sau în *Cockpit*. Dosarele și jaloanele lipsă sunt adăugate; nimic nu este suprascris sau șters.

## 5. Navigare

| Zonă | Conținut |
|---|---|
| **Azi** | Sarcinile de azi și cele întârziate, jaloanele din zilele următoare, riscuri ridicate |
| **Sarcini** | Toate sarcinile pe grupuri, schimbarea stării, mutare cu o zi, filtrare după proiect |
| **Proiecte** | Crearea proiectelor cu metodă, schimbarea metodei, completarea structurii |
| **Cockpit** | Toate proiectele cu metodă: fază, următorul jalon, PI și predictibilitate, riscuri, decizii, blocaje |
| **HERMES** | Modelul fazelor, jaloane cu date planificate/efective și stare, aprobări, jaloane suplimentare, documente HERMES |
| **SAFe** | Planning Intervals, obiective PI cu valoare de business, predictibilitate, ART Planning Board, Inspect & Adapt, retrospective, ART Sync |
| **Riscuri** | Panou ROAM cu matricea riscurilor, măsura ca sarcină, rânduri marcate în note |
| **Decizii** | Registrul deciziilor din note de decizie, decizii tehnice (ADR) și rânduri cu `#entscheid` |
| **Ședință periodică** | Serii de ședințe cu următoarea dată și puncte deschise; fiecare ședință nouă preia punctele deschise |
| **Documente** | Notă nouă din orice șablon din dosarul de șabloane, inclusiv din cele proprii |
| **Echipă și părți interesate** | Persoane cu rol, influență, interes și sarcinile lor deschise |
| **Căutare** | Căutare în text integral în tot seiful |
| **Setări** | Limbă, seif, numele dosarelor, ora sincronizării, jurnal |

Fiecare titlu este un link `obsidian://` care deschide nota direct în Obsidian.

## 6. Structura proiectului

```
Proiecte/<Proiect>/
├── _Proiect_<Proiect>.md          Prezentare cu tabele Dataview
├── 01_Mandatul_proiectului/       Mandate de inițializare și execuție, business case
├── 02_Planificare/                Planul de management al proiectului, termene, resurse, costuri
├── 03_Coordonare/                 Jaloane, rapoarte de stare, cereri de modificare, PI
│   ├── Jaloane/
│   └── PI/<PI>/                   Nota PI, ART Planning Board, obiective, retrospective
├── 04_Riscuri_si_decizii/
│   ├── Riscuri/
│   └── Decizii/
├── 05_Sedinte/                    Comitetul de coordonare, ședințe de proiect, ateliere
├── 06_Rezultate/                  Concepte, specificații, teste, recepții
├── 07_Parti_interesate/
└── 99_Arhiva/
```
Numele dosarelor urmează limba aleasă la configurare. O schimbare ulterioară a limbii nu redenumește dosarele existente.

Alte dosare din seif: `Sarcini/` (sarcini fără proiect), `Sedinte_periodice/`, `Echipa/`, `Planificator/` (prezentarea zilei, cockpit MP, registrul deciziilor), `_Sabloane/`, `SAFe/` (PI fără proiect).

## 7. HERMES

- **Faze:** clasic inițializare, concept, realizare, implementare, încheiere; agil inițializare, execuție, încheiere.
- **Jaloane:** fiecare jalon este o notă cu puncte de control conform HERMES 2022 de bifat. Sunt rezultate tipice; adaptați-le scenariului dumneavoastră.
- **Faza curentă:** rezultă din primul jalon principal deschis. Stările *atins*, *aprobat* și *anulat* sunt considerate finalizate.
- **Aprobă:** setează starea și data efectivă și, la cerere, creează o decizie în registrul deciziilor, legată de jalon.
- **Jaloane suplimentare:** de ex. aprobări de release, arhitectura soluției, conceptul de securitate, recepția provizorie, punerea în funcțiune.
- **Documente:** planul de management al proiectului, raportul de stare a proiectului, cererea de modificare, procesul-verbal al comitetului de coordonare și evaluarea finală a proiectului sunt precompletate cu faza curentă, jaloanele, riscurile deschise și deciziile din ultimele 30 de zile.

## 8. SAFe

- **Planning Interval:** nume (de ex. `2027-PI1`), început, numărul de iterații și săptămâni pe iterație. Ultima iterație este iterația IP (Innovation & Planning).
- **ART Planning Board:** se creează un fișier canvas Obsidian cu o coloană pe iterație și un rând pe echipă. Trageți funcționalitățile ca fișe și legați dependențele cu săgeți.
- **Obiective PI:** pe echipă, *committed* sau *uncommitted*, valoare de business planificată (1 până la 10) și atinsă.
- **Predictibilitate:** valoarea de business atinsă de toate obiectivele împărțită la valoarea de business planificată a obiectivelor committed. Interval țintă 80 până la 100 %.
- **Inspect & Adapt:** cu un clic la sfârșitul PI; nota conține tabelul actual de predictibilitate și un șablon pentru atelierul de rezolvare a problemelor.

## 9. Riscuri, decizii și marcaje

**ROAM:** *Resolved* (nu mai este un risc), *Owned* (o persoană se ocupă de el), *Accepted* (acceptat în mod conștient), *Mitigated* (măsurile reduc riscul). Matricea arată probabilitatea și impactul. *Creează sarcină* transformă măsura într-o sarcină și trece un risc deschis la *Owned*.

**Marcaje în orice notă:** un rând cu `#entscheid` (decizie), `#risiko` (risc) sau `#blocker` (blocaj) este detectat la sincronizare (nu în blocurile de cod). Marcajele sunt aceleași în toate limbile.

```markdown
## Decizii
- #entscheid Bugetul pentru faza 2 a fost aprobat.
## Riscuri
- #risiko Furnizorul X anunță întârzieri -> [[Max Muster]] clarifică.
- #blocker Mediul de testare lipsește de două săptămâni.
```

Sincronizarea creează `Planificator/Cockpit-MP.md` și `Planificator/Registrul-deciziilor.md`, ca să vedeți prezentarea generală și în Obsidian și pe telefon.

## 10. Câmpurile din frontmatter

Numele și valorile câmpurilor din frontmatter sunt **aceleași în toate limbile** (identificatori în germană). Astfel un seif rămâne interschimbabil între limbi, iar interogările Dataview funcționează peste tot.

| typ | câmpuri importante |
|---|---|
| `task` (sarcină) | `projekt` (proiect), `start`, `ende` (sfârșit), `status` (offen, in_arbeit, wartend, erledigt), `prioritaet` (hoch, mittel, niedrig), `verantwortlich` (responsabil) |
| `meilenstein` (jalon) | `projekt`, `phase`, `plan` (planificat), `ist` (efectiv), `status` (geplant, gefaehrdet, verschoben, erreicht, freigegeben, entfallen), `kern` (principal: ja/nein) |
| `risiko` (risc) | `projekt`, `pi`, `wahrscheinlichkeit` (probabilitate), `auswirkung` (impact), `roam` (offen, owned, mitigated, accepted, resolved), `verantwortlich`, `faellig` (termen) |
| `entscheid` (decizie) | `projekt`, `datum`, `gremium` (organ), `entscheid_durch` (decis de), `status` (beantragt, entschieden, zurueckgestellt, abgelehnt) |
| `pi` | `start`, `ende`, `iterationen`, `iteration_wochen`, `art`, `status` |
| `pi_objective` | `pi`, `team`, `committed` (ja/nein), `bv_geplant` (VB planificată), `bv_ist` (VB atinsă) |

Este sarcină orice fișier `Task_*.md` sau orice notă cu `typ: task`. Fără `status`, o sarcină este finalizată când toate casetele sunt bifate.

## 11. Pluginuri Obsidian recomandate

- **Dataview** (plugin al comunității): afișează tabelele din prezentarea proiectului, nota PI și Inspect & Adapt.
- Opțional **Tasks**, **Templater** și **Kanban**. Agentul nu are nevoie de ele.

## 12. Adaptarea șabloanelor

Toate șabloanele se află în `_Sabloane/` din seif și pot fi modificate în Obsidian; agentul folosește apoi versiunea dumneavoastră. Șabloanele șterse sunt create din nou la următoarea sincronizare. Șabloanele proprii apar la *Documente*. Cu `zielordner: 05_Sedinte` în frontmatter stabiliți dosarul preselectat. Substituenți: `{{title}}`, `{{date}}`, `{{projekt}}`.

## 13. Linia de comandă

```
python agent.py sync                       citește seiful acum
python agent.py heute                      afișează sarcinile de azi
python agent.py projekt "Nume" --methode hermes_klassisch   (hermes_agil, safe, hermes_safe)
python agent.py sprache ro                 setează limba
```

## 14. Depanare

| Problemă | Soluție |
|---|---|
| Nu apare nimic la 127.0.0.1:5055 | Windows: meniul Start > *Obsidian-Agent*. macOS: deschideți aplicația *Obsidian-Agent*. Verificați jurnalul. |
| Seiful nu a fost găsit | *Setări* > corectați calea către seiful Obsidian |
| Sincronizarea în fundal nu funcționează pe macOS | Acordați acces complet la disc fișierului Python afișat (vezi secțiunea 2) |
| Tabelele Dataview sunt goale | Instalați și activați pluginul comunității Dataview în Obsidian |
| Portul 5055 este ocupat | Modificați valoarea `port` în `config.json` și rulați din nou configurarea |
