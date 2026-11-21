# Obsidian-Agent – Guía

Obsidian-Agent facilita la gestión de proyectos con Obsidian según **HERMES 2022** y **SAFe**. Funciona localmente en su ordenador; la interfaz se abre en el navegador en `http://127.0.0.1:5055`.

**Principio:** los archivos Markdown de su bóveda de Obsidian son la única fuente de verdad. El agente crea notas, lee la bóveda y mantiene una base de datos (`planer.db`) como índice. Puede trabajar directamente en Obsidian en cualquier momento; en la siguiente sincronización se vuelve a leer todo.

Idiomas: alemán, inglés, francés, italiano, español, rumano. El idioma se elige durante la instalación y en cualquier momento en *Ajustes*.

---

## 1. Instalación en Windows

1. Ejecutar `Obsidian-Agent-Setup-2.0.0.exe`.
   Si aparece *Windows protegió su PC*, hacer clic en **Más información** y después en **Ejecutar de todas formas**. El instalador no está firmado digitalmente.
2. Elegir el idioma. La instalación **no necesita derechos de administrador** y se realiza en `%LOCALAPPDATA%\Programs\Obsidian-Agent`.
3. Tras la copia se abre una ventana de consola que completa la configuración:
   - Se busca Python 3.10 o posterior. Si falta, se instala Python 3.12 con `winget`. Si no funciona, se abre python.org (marcar *Add python.exe to PATH* durante la instalación) y después se vuelve a ejecutar el instalador.
   - En el diálogo, elegir la **carpeta de su bóveda de Obsidian**.
   - Se crean dos tareas programadas: *Obsidian-Agent Weboberflaeche* (inicio en segundo plano en cada inicio de sesión) y *Obsidian-Agent Tages-Sync* (cada día a las 04:00; las ejecuciones perdidas se recuperan).
   - El navegador se abre con la interfaz. Pulse Intro para cerrar la ventana.
4. Más adelante, ábralo desde el menú Inicio o el acceso directo **Obsidian-Agent** del escritorio.

**Actualización:** instale simplemente el nuevo archivo de instalación encima. `config.json`, `planer.db` y el entorno de Python se conservan.

**Desinstalación:** *Configuración > Aplicaciones > Obsidian-Agent > Desinstalar* o menú Inicio > *Desinstalar Obsidian-Agent*. Se eliminan las tareas programadas y se guarda una copia de `config.json` y `planer.db` en la carpeta `obsidian-agent-backup_<fecha>` del escritorio. **Su bóveda no se modifica.**

Registro: `%LOCALAPPDATA%\obsidian-agent\gui.log`

## 2. Instalación en macOS

1. Abrir `Obsidian-Agent-2.0.0.pkg`.
   El paquete no está notarizado. Si macOS indica que *no se puede abrir*: clic derecho en el archivo > **Abrir** > **Abrir**. En versiones recientes de macOS, vaya a *Ajustes del Sistema > Privacidad y seguridad* y elija **Abrir igualmente**.
2. Siga los pasos del instalador. La app se instala en **Aplicaciones**.
3. Al final de la instalación, un diálogo pide la **carpeta de su bóveda de Obsidian**. El idioma sigue el idioma del sistema del Mac (se puede cambiar en *Ajustes*).
4. Se necesita **Python 3.9 o posterior**. Si falta, aparece un aviso y se abre python.org. Tras instalar Python, abra simplemente la app **Obsidian-Agent**; la configuración se ejecuta entonces en el Terminal.
5. A partir de entonces el agente se inicia en segundo plano en cada inicio de sesión y la sincronización diaria se ejecuta a las 04:00 (launchd, también sin la interfaz abierta).

Abrir: la app **Obsidian-Agent** en Aplicaciones o en Launchpad.

**Permisos de carpetas:** si la bóveda está en *Documentos*, en el *Escritorio* o en *iCloud Drive*, macOS puede bloquear el acceso en segundo plano. La configuración lo detecta y muestra el archivo que debe añadir en *Ajustes del Sistema > Privacidad y seguridad > Acceso total al disco*.

Datos del programa: `~/Library/Application Support/Obsidian-Agent` · Registros: `~/Library/Logs/obsidian-agent`

**Actualización:** instale el nuevo paquete y abra la app una vez. Los ajustes y la base de datos se conservan.

**Desinstalación:** haga doble clic en `~/Library/Application Support/Obsidian-Agent/mac/uninstall.command` (en el Finder con *Ir > Ir a la carpeta*). Después arrastre la app *Obsidian-Agent* desde *Aplicaciones* a la papelera. La copia de seguridad se guarda en el escritorio.

## 3. Linux

```bash
./start.sh                      # configura el entorno de Python e inicia la interfaz
python3 agent.py sprache es     # definir el idioma (de, en, fr, it, es, ro)
```
Sincronización diaria con `crontab -e`:
`0 4 * * * cd /ruta/obsidian-agent && .venv/bin/python agent.py sync --geplant`

---

## 4. Primeros pasos

1. **Ajustes:** comprobar el idioma y la ruta de la bóveda.
2. **Proyectos > Nuevo proyecto:** introducir un nombre y elegir un método:
   | Método | Qué se crea |
   |---|---|
   | Sin método | Estructura de carpetas y resumen del proyecto |
   | HERMES clásico | además 6 hitos: aprobación de la inicialización del proyecto, aprobación de la ejecución, aprobación de fase Realización, aprobación de fase Implantación, aprobación de fase Cierre, cierre del proyecto |
   | HERMES ágil | además 4 hitos: aprobación de la inicialización del proyecto, aprobación de la ejecución, aprobación de fase Cierre, cierre del proyecto |
   | SAFe | Estructura de carpetas; los Planning Intervals se crean en *SAFe* |
   | HERMES ágil + SAFe | Dirección según HERMES, la ejecución se realiza en PI |
3. **HERMES:** fijar las fechas previstas de los hitos.
4. Trabajar en Obsidian. **Actualizar ahora** lee la bóveda de inmediato; si no, cada día a las 04:00.

El método se puede cambiar en cualquier momento en *Proyectos* o en el *Cockpit*. Se añaden las carpetas y los hitos que faltan; no se sobrescribe ni se elimina nada.

## 5. Navegación

| Área | Contenido |
|---|---|
| **Hoy** | Tareas de hoy y atrasadas, hitos de los próximos días, riesgos altos |
| **Tareas** | Todas las tareas por grupo, cambiar el estado, mover un día, filtrar por proyecto |
| **Proyectos** | Crear proyectos con método, cambiar el método, completar la estructura |
| **Cockpit** | Todos los proyectos con método: fase, próximo hito, PI y previsibilidad, riesgos, decisiones, bloqueos |
| **HERMES** | Modelo de fases, hitos con fechas previstas/reales y estado, aprobaciones, hitos adicionales, documentos HERMES |
| **SAFe** | Planning Intervals, objetivos de PI con valor de negocio, previsibilidad, ART Planning Board, Inspect & Adapt, retrospectivas, ART Sync |
| **Riesgos** | Tablero ROAM con matriz de riesgos, medida como tarea, líneas marcadas en las notas |
| **Decisiones** | Registro de decisiones a partir de notas de decisión, decisiones técnicas (ADR) y líneas con `#entscheid` |
| **Reunión periódica** | Series de reuniones con próxima fecha y asuntos pendientes; cada nueva reunión incluye los pendientes abiertos |
| **Documentos** | Nueva nota a partir de cualquier plantilla de la carpeta de plantillas, también de las propias |
| **Equipo y partes interesadas** | Personas con rol, influencia, interés y sus tareas abiertas |
| **Búsqueda** | Búsqueda de texto completo en toda la bóveda |
| **Ajustes** | Idioma, bóveda, nombres de carpetas, hora de sincronización, registro |

Cada título es un enlace `obsidian://` que abre la nota directamente en Obsidian.

## 6. Estructura del proyecto

```
Proyectos/<Proyecto>/
├── _Proyecto_<Proyecto>.md        Resumen con tablas Dataview
├── 01_Mandato_del_proyecto/       Mandatos de inicialización y de ejecución, business case
├── 02_Planificacion/              Plan de gestión del proyecto, plazos, recursos, costes
├── 03_Direccion/                  Hitos, informes de estado, solicitudes de cambio, PI
│   ├── Hitos/
│   └── PI/<PI>/                   Nota del PI, ART Planning Board, objetivos, retros
├── 04_Riesgos_y_decisiones/
│   ├── Riesgos/
│   └── Decisiones/
├── 05_Reuniones/                  Comité de dirección, reuniones del proyecto, talleres
├── 06_Resultados/                 Conceptos, especificaciones, pruebas, recepciones
├── 07_Partes_interesadas/
└── 99_Archivo/
```
Los nombres de las carpetas siguen el idioma elegido durante la configuración. Un cambio posterior de idioma no renombra las carpetas existentes.

Otras carpetas de la bóveda: `Tareas/` (tareas sin proyecto), `Reuniones_periodicas/`, `Equipo/`, `Planificador/` (resumen del día, cockpit GP, registro de decisiones), `_Plantillas/`, `SAFe/` (PI sin proyecto).

## 7. HERMES

- **Fases:** clásico inicialización, concepto, realización, implantación, cierre; ágil inicialización, ejecución, cierre.
- **Hitos:** cada hito es una nota con puntos de control según HERMES 2022 para marcar. Son resultados típicos; adáptelos a su escenario.
- **Fase actual:** se deriva del primer hito principal abierto. Los estados *alcanzado*, *aprobado* y *suprimido* cuentan como finalizados.
- **Aprobar:** fija el estado y la fecha real y, si se desea, crea una decisión en el registro de decisiones vinculada al hito.
- **Hitos adicionales:** p. ej. aprobaciones de release, arquitectura de la solución, concepto de seguridad, recepción provisional, puesta en servicio.
- **Documentos:** plan de gestión del proyecto, informe de estado del proyecto, solicitud de cambio, acta del comité de dirección y evaluación final del proyecto se rellenan con la fase actual, los hitos, los riesgos abiertos y las decisiones de los últimos 30 días.

## 8. SAFe

- **Planning Interval:** nombre (p. ej. `2027-PI1`), inicio, número de iteraciones y semanas por iteración. La última iteración es la iteración IP (Innovation & Planning).
- **ART Planning Board:** se crea un archivo de lienzo de Obsidian con una columna por iteración y una fila por equipo. Arrastre las funcionalidades como tarjetas y conecte las dependencias con flechas.
- **Objetivos de PI:** por equipo, *committed* o *uncommitted*, valor de negocio planificado (1 a 10) y alcanzado.
- **Previsibilidad:** valor de negocio alcanzado de todos los objetivos dividido por el valor de negocio planificado de los objetivos committed. Rango objetivo del 80 al 100 %.
- **Inspect & Adapt:** con un clic al final del PI; la nota contiene la tabla de previsibilidad actual y una plantilla para el taller de resolución de problemas.

## 9. Riesgos, decisiones y marcas

**ROAM:** *Resolved* (ya no es un riesgo), *Owned* (una persona se encarga), *Accepted* (se acepta conscientemente), *Mitigated* (las medidas reducen el riesgo). La matriz muestra probabilidad e impacto. *Crear tarea* convierte la medida en una tarea y pasa un riesgo abierto a *Owned*.

**Marcas en cualquier nota:** una línea con `#entscheid` (decisión), `#risiko` (riesgo) o `#blocker` (bloqueo) se detecta en la sincronización (no en bloques de código). Las marcas son iguales en todos los idiomas.

```markdown
## Decisiones
- #entscheid Se ha aprobado el presupuesto de la fase 2.
## Riesgos
- #risiko El proveedor X anuncia retrasos -> [[Max Muster]] lo aclara.
- #blocker Falta el entorno de pruebas desde hace dos semanas.
```

La sincronización crea `Planificador/Cockpit-GP.md` y `Planificador/Registro-de-decisiones.md`, para ver el resumen también en Obsidian y en el móvil.

## 10. Campos del frontmatter

Los nombres y valores de los campos del frontmatter son **iguales en todos los idiomas** (identificadores en alemán). Así una bóveda sigue siendo intercambiable entre idiomas y las consultas Dataview funcionan en todas partes.

| typ | campos importantes |
|---|---|
| `task` (tarea) | `projekt` (proyecto), `start`, `ende` (fin), `status` (offen, in_arbeit, wartend, erledigt), `prioritaet` (hoch, mittel, niedrig), `verantwortlich` (responsable) |
| `meilenstein` (hito) | `projekt`, `phase`, `plan` (previsto), `ist` (real), `status` (geplant, gefaehrdet, verschoben, erreicht, freigegeben, entfallen), `kern` (principal: ja/nein) |
| `risiko` (riesgo) | `projekt`, `pi`, `wahrscheinlichkeit` (probabilidad), `auswirkung` (impacto), `roam` (offen, owned, mitigated, accepted, resolved), `verantwortlich`, `faellig` (vencimiento) |
| `entscheid` (decisión) | `projekt`, `datum`, `gremium` (órgano), `entscheid_durch` (decidido por), `status` (beantragt, entschieden, zurueckgestellt, abgelehnt) |
| `pi` | `start`, `ende`, `iterationen`, `iteration_wochen`, `art`, `status` |
| `pi_objective` | `pi`, `team`, `committed` (ja/nein), `bv_geplant` (VN planificado), `bv_ist` (VN alcanzado) |

Se considera tarea todo archivo `Task_*.md` o toda nota con `typ: task`. Sin `status`, una tarea se considera finalizada cuando todas las casillas están marcadas.

## 11. Plugins de Obsidian recomendados

- **Dataview** (plugin comunitario): muestra las tablas del resumen del proyecto, de la nota del PI y de Inspect & Adapt.
- Opcionales **Tasks**, **Templater** y **Kanban**. El agente no los necesita.

## 12. Adaptar las plantillas

Todas las plantillas están en `_Plantillas/` de la bóveda y se pueden modificar en Obsidian; el agente usa después su versión. Las plantillas eliminadas se vuelven a crear en la siguiente sincronización. Las plantillas propias aparecen en *Documentos*. Con `zielordner: 05_Reuniones` en el frontmatter se define la carpeta preseleccionada. Marcadores: `{{title}}`, `{{date}}`, `{{projekt}}`.

## 13. Línea de comandos

```
python agent.py sync                       leer la bóveda ahora
python agent.py heute                      mostrar las tareas de hoy
python agent.py projekt "Nombre" --methode hermes_klassisch   (hermes_agil, safe, hermes_safe)
python agent.py sprache es                 definir el idioma
```

## 14. Solución de problemas

| Problema | Solución |
|---|---|
| No aparece nada en 127.0.0.1:5055 | Windows: menú Inicio > *Obsidian-Agent*. macOS: abrir la app *Obsidian-Agent*. Revisar el registro. |
| Bóveda no encontrada | *Ajustes* > corregir la ruta de la bóveda de Obsidian |
| La sincronización en segundo plano no funciona en macOS | Conceder acceso total al disco al archivo de Python indicado (véase el apartado 2) |
| Las tablas Dataview están vacías | Instalar y activar el plugin comunitario Dataview en Obsidian |
| El puerto 5055 está ocupado | Cambiar el valor `port` en `config.json` y volver a ejecutar la configuración |
