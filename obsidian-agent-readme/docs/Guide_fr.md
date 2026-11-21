# Obsidian-Agent – Guide d'utilisation

Obsidian-Agent facilite la gestion de projet avec Obsidian selon **HERMES 2022** et **SAFe**. Il fonctionne localement sur votre ordinateur ; l'interface s'ouvre dans le navigateur à l'adresse `http://127.0.0.1:5055`.

**Principe :** les fichiers Markdown de votre coffre Obsidian sont la seule référence. L'agent crée des notes, lit le coffre et tient une base de données (`planer.db`) servant d'index. Vous pouvez travailler directement dans Obsidian à tout moment ; tout est relu lors de la synchronisation suivante.

Langues : allemand, anglais, français, italien, espagnol, roumain. La langue se choisit lors de l'installation et à tout moment sous *Paramètres*.

---

## 1. Installation sous Windows

1. Lancer `Obsidian-Agent-Setup-2.0.0.exe`.
   Si le message *Windows a protégé votre ordinateur* apparaît, cliquer sur **Informations complémentaires** puis **Exécuter quand même**. Le programme d'installation n'est pas signé numériquement.
2. Choisir la langue. L'installation ne nécessite **aucun droit d'administrateur** et s'effectue dans `%LOCALAPPDATA%\Programs\Obsidian-Agent`.
3. Après la copie, une fenêtre de console s'ouvre et termine la configuration :
   - Python 3.10 ou plus récent est recherché. S'il manque, Python 3.12 est installé avec `winget`. En cas d'échec, python.org s'ouvre (cocher *Add python.exe to PATH* lors de l'installation) et vous relancez ensuite le programme d'installation.
   - Dans la boîte de dialogue, choisir le **dossier de votre coffre Obsidian**.
   - Deux tâches planifiées sont créées : *Obsidian-Agent Weboberflaeche* (démarrage en arrière-plan à chaque ouverture de session) et *Obsidian-Agent Tages-Sync* (chaque jour à 04:00, les exécutions manquées sont rattrapées).
   - Le navigateur s'ouvre avec l'interface. Appuyer sur Entrée pour fermer la fenêtre.
4. Par la suite, ouvrir l'application depuis le menu Démarrer ou le raccourci **Obsidian-Agent** du bureau.

**Mise à jour :** installer simplement le nouveau fichier par-dessus. `config.json`, `planer.db` et l'environnement Python sont conservés.

**Désinstallation :** *Paramètres > Applications > Obsidian-Agent > Désinstaller* ou menu Démarrer > *Désinstaller Obsidian-Agent*. Les tâches planifiées sont supprimées, `config.json` et `planer.db` sont sauvegardés dans le dossier `obsidian-agent-backup_<date>` sur le bureau. **Votre coffre n'est pas modifié.**

Journal : `%LOCALAPPDATA%\obsidian-agent\gui.log`

## 2. Installation sous macOS

1. Ouvrir `Obsidian-Agent-2.0.0.pkg`.
   Le paquet n'est pas notarié. Si macOS indique qu'il *ne peut pas être ouvert* : clic droit sur le fichier > **Ouvrir** > **Ouvrir**. Sur les versions récentes de macOS, aller dans *Réglages Système > Confidentialité et sécurité* et choisir **Ouvrir quand même**.
2. Suivre les étapes du programme d'installation. L'app est installée dans **Applications**.
3. À la fin de l'installation, une boîte de dialogue demande le **dossier de votre coffre Obsidian**. La langue suit la langue du système du Mac (modifiable sous *Paramètres*).
4. **Python 3.9 ou plus récent** est requis. S'il manque, un message s'affiche et python.org s'ouvre. Après l'installation de Python, ouvrir simplement l'app **Obsidian-Agent** ; la configuration se déroule alors dans le Terminal.
5. L'agent démarre ensuite en arrière-plan à chaque ouverture de session et la synchronisation quotidienne s'exécute à 04:00 (launchd, même sans interface ouverte).

Ouvrir : l'app **Obsidian-Agent** dans Applications ou dans le Launchpad.

**Droits d'accès aux dossiers :** si le coffre se trouve dans *Documents*, sur le *Bureau* ou dans *iCloud Drive*, macOS peut bloquer l'accès en arrière-plan. La configuration le détecte et indique le fichier à ajouter sous *Réglages Système > Confidentialité et sécurité > Accès complet au disque*.

Données du programme : `~/Library/Application Support/Obsidian-Agent` · Journaux : `~/Library/Logs/obsidian-agent`

**Mise à jour :** installer le nouveau paquet et ouvrir une fois l'app. Les paramètres et la base de données sont conservés.

**Désinstallation :** double-cliquer sur `~/Library/Application Support/Obsidian-Agent/mac/uninstall.command` (dans le Finder via *Aller > Aller au dossier*). Glisser ensuite l'app *Obsidian-Agent* depuis *Applications* dans la corbeille. La sauvegarde est placée sur le bureau.

## 3. Linux

```bash
./start.sh                      # configure l'environnement Python et démarre l'interface
python3 agent.py sprache fr     # définir la langue (de, en, fr, it, es, ro)
```
Synchronisation quotidienne avec `crontab -e` :
`0 4 * * * cd /chemin/obsidian-agent && .venv/bin/python agent.py sync --geplant`

---

## 4. Premiers pas

1. **Paramètres :** vérifier la langue et le chemin du coffre.
2. **Projets > Nouveau projet :** saisir un nom et choisir une méthode :
   | Méthode | Ce qui est créé |
   |---|---|
   | Sans méthode | Structure de dossiers et aperçu du projet |
   | HERMES classique | plus 6 jalons : validation de l'initialisation du projet, validation de l'exécution, validation de phase Réalisation, validation de phase Déploiement, validation de phase Clôture, clôture du projet |
   | HERMES agile | plus 4 jalons : validation de l'initialisation du projet, validation de l'exécution, validation de phase Clôture, clôture du projet |
   | SAFe | Structure de dossiers ; les Planning Intervals se créent sous *SAFe* |
   | HERMES agile + SAFe | Pilotage HERMES, la mise en œuvre se fait en PI |
3. **HERMES :** saisir les dates prévues des jalons.
4. Travailler dans Obsidian. **Mettre à jour maintenant** lit le coffre immédiatement, sinon chaque jour à 04:00.

La méthode peut être changée à tout moment sous *Projets* ou dans le *Cockpit*. Les dossiers et jalons manquants sont ajoutés ; rien n'est écrasé ni supprimé.

## 5. Navigation

| Zone | Contenu |
|---|---|
| **Aujourd'hui** | Tâches du jour et en retard, jalons des prochains jours, risques élevés |
| **Tâches** | Toutes les tâches par groupe, changer le statut, décaler d'un jour, filtrer par projet |
| **Projets** | Créer des projets avec méthode, changer la méthode, compléter la structure |
| **Cockpit** | Tous les projets avec méthode : phase, prochain jalon, PI et prévisibilité, risques, décisions, blocages |
| **HERMES** | Modèle de phases, jalons avec dates prévues/effectives et statut, validations, jalons supplémentaires, documents HERMES |
| **SAFe** | Planning Intervals, objectifs de PI avec valeur métier, prévisibilité, ART Planning Board, Inspect & Adapt, rétrospectives, ART Sync |
| **Risques** | Tableau ROAM avec matrice des risques, mesure sous forme de tâche, lignes marquées dans les notes |
| **Décisions** | Journal des décisions issu des notes de décision, des décisions techniques (ADR) et des lignes marquées `#entscheid` |
| **Réunion périodique** | Séries de séances avec prochaine date et points en suspens ; chaque nouvelle séance reprend les points ouverts |
| **Documents** | Nouvelle note à partir de n'importe quel modèle du dossier des modèles, y compris les vôtres |
| **Équipe et parties prenantes** | Personnes avec rôle, influence, intérêt et leurs tâches ouvertes |
| **Recherche** | Recherche en texte intégral dans tout le coffre |
| **Paramètres** | Langue, coffre, noms des dossiers, heure de synchronisation, journal |

Chaque titre est un lien `obsidian://` qui ouvre la note directement dans Obsidian.

## 6. Structure d'un projet

```
Projets/<Projet>/
├── _Projet_<Projet>.md            Aperçu avec tableaux Dataview
├── 01_Mandat_de_projet/           Mandats d'initialisation et d'exécution, business case
├── 02_Planification/              Plan de gestion de projet, délais, ressources, coûts
├── 03_Pilotage/                   Jalons, rapports d'état, demandes de modification, PI
│   ├── Jalons/
│   └── PI/<PI>/                   Note de PI, ART Planning Board, objectifs, rétros
├── 04_Risques_et_decisions/
│   ├── Risques/
│   └── Decisions/
├── 05_Reunions/                   Comité de pilotage, séances de projet, ateliers
├── 06_Resultats/                  Concepts, spécifications, tests, réceptions
├── 07_Parties_prenantes/
└── 99_Archives/
```
Les noms des dossiers suivent la langue choisie lors de la configuration. Un changement ultérieur de langue ne renomme pas les dossiers existants.

Autres dossiers du coffre : `Taches/` (tâches sans projet), `Reunions_periodiques/`, `Equipe/`, `Planificateur/` (vue du jour, cockpit GP, journal des décisions), `_Modeles/`, `SAFe/` (PI sans projet).

## 7. HERMES

- **Phases :** classique initialisation, conception, réalisation, déploiement, clôture ; agile initialisation, mise en œuvre, clôture.
- **Jalons :** chaque jalon est une note avec des points de contrôle selon HERMES 2022 à cocher. Ce sont des résultats typiques ; adaptez-les à votre scénario.
- **Phase actuelle :** découle du premier jalon principal ouvert. Les statuts *atteint*, *validé* et *supprimé* sont considérés comme terminés.
- **Valider :** définit le statut et la date effective et crée, si souhaité, une décision dans le journal des décisions, liée au jalon.
- **Jalons supplémentaires :** p. ex. validations de release, architecture de la solution, concept SIPD, réception provisoire, mise en exploitation.
- **Documents :** plan de gestion de projet, rapport sur l'état du projet, demande de modification, procès-verbal du comité de pilotage et évaluation finale du projet sont préremplis avec la phase actuelle, les jalons, les risques ouverts et les décisions des 30 derniers jours.

## 8. SAFe

- **Planning Interval :** nom (p. ex. `2027-PI1`), début, nombre d'itérations et semaines par itération. La dernière itération est l'itération IP (Innovation & Planning).
- **ART Planning Board :** un fichier canevas Obsidian est créé, avec une colonne par itération et une ligne par équipe. Glisser les fonctionnalités sous forme de cartes et relier les dépendances par des flèches.
- **Objectifs de PI :** par équipe, *committed* ou *uncommitted*, valeur métier prévue (1 à 10) et atteinte.
- **Prévisibilité :** valeur métier atteinte de tous les objectifs divisée par la valeur métier prévue des objectifs committed. Plage cible 80 à 100 %.
- **Inspect & Adapt :** d'un clic à la fin du PI ; la note contient le tableau de prévisibilité actuel et un modèle pour l'atelier de résolution de problèmes.

## 9. Risques, décisions et marquages

**ROAM :** *Resolved* (n'est plus un risque), *Owned* (une personne s'en occupe), *Accepted* (accepté en connaissance de cause), *Mitigated* (des mesures réduisent le risque). La matrice montre probabilité et impact. *Créer une tâche* transforme la mesure en tâche et fait passer un risque ouvert à *Owned*.

**Marquages dans n'importe quelle note :** une ligne avec `#entscheid` (décision), `#risiko` (risque) ou `#blocker` (blocage) est détectée lors de la synchronisation (pas dans les blocs de code). Les marquages sont identiques dans toutes les langues.

```markdown
## Décisions
- #entscheid Le budget de la phase 2 a été validé.
## Risques
- #risiko Le fournisseur X annonce des retards -> [[Max Muster]] clarifie.
- #blocker L'environnement de test manque depuis deux semaines.
```

La synchronisation crée `Planificateur/Cockpit-GP.md` et `Planificateur/Journal-des-decisions.md`, pour consulter la vue d'ensemble aussi dans Obsidian et sur le téléphone.

## 10. Champs du frontmatter

Les noms et valeurs des champs du frontmatter sont **identiques dans toutes les langues** (identifiants allemands). Un coffre reste ainsi interchangeable entre les langues et les requêtes Dataview fonctionnent partout.

| typ | champs importants |
|---|---|
| `task` (tâche) | `projekt` (projet), `start`, `ende` (fin), `status` (offen, in_arbeit, wartend, erledigt), `prioritaet` (hoch, mittel, niedrig), `verantwortlich` (responsable) |
| `meilenstein` (jalon) | `projekt`, `phase`, `plan` (prévu), `ist` (effectif), `status` (geplant, gefaehrdet, verschoben, erreicht, freigegeben, entfallen), `kern` (principal : ja/nein) |
| `risiko` (risque) | `projekt`, `pi`, `wahrscheinlichkeit` (probabilité), `auswirkung` (impact), `roam` (offen, owned, mitigated, accepted, resolved), `verantwortlich`, `faellig` (échéance) |
| `entscheid` (décision) | `projekt`, `datum`, `gremium` (organe), `entscheid_durch` (décidé par), `status` (beantragt, entschieden, zurueckgestellt, abgelehnt) |
| `pi` | `start`, `ende`, `iterationen`, `iteration_wochen`, `art`, `status` |
| `pi_objective` | `pi`, `team`, `committed` (ja/nein), `bv_geplant` (VM prévue), `bv_ist` (VM atteinte) |

Est considéré comme tâche tout fichier `Task_*.md` ou toute note avec `typ: task`. Sans `status`, une tâche est terminée lorsque toutes les cases sont cochées.

## 11. Plugins Obsidian recommandés

- **Dataview** (plugin communautaire) : affiche les tableaux de l'aperçu du projet, de la note de PI et d'Inspect & Adapt.
- En option **Tasks**, **Templater** et **Kanban**. L'agent n'en a pas besoin.

## 12. Adapter les modèles

Tous les modèles se trouvent dans `_Modeles/` du coffre et peuvent être modifiés dans Obsidian ; l'agent utilise ensuite votre version. Les modèles supprimés sont recréés à la synchronisation suivante. Vos propres modèles apparaissent sous *Documents*. Avec `zielordner: 05_Reunions` dans le frontmatter, vous définissez le dossier présélectionné. Espaces réservés : `{{title}}`, `{{date}}`, `{{projekt}}`.

## 13. Ligne de commande

```
python agent.py sync                       lire le coffre maintenant
python agent.py heute                      afficher les tâches du jour
python agent.py projekt "Nom" --methode hermes_klassisch   (hermes_agil, safe, hermes_safe)
python agent.py sprache fr                 définir la langue
```

## 14. Dépannage

| Problème | Solution |
|---|---|
| Rien à l'adresse 127.0.0.1:5055 | Windows : menu Démarrer > *Obsidian-Agent*. macOS : ouvrir l'app *Obsidian-Agent*. Consulter le journal. |
| Coffre introuvable | *Paramètres* > corriger le chemin du coffre Obsidian |
| La synchronisation en arrière-plan ne fonctionne pas sous macOS | Accorder l'accès complet au disque au fichier Python indiqué (voir section 2) |
| Les tableaux Dataview sont vides | Installer et activer le plugin communautaire Dataview dans Obsidian |
| Le port 5055 est occupé | Modifier la valeur `port` dans `config.json` et relancer la configuration |
