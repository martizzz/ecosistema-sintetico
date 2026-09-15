## 0. Instrucciones para Codex antes de empezar
 
Vas a desarrollar un plugin de Codex para un equipo de UX Research. Antes de tocar ningún archivo:
 
1. Lee este documento entero.
2. Consulta la documentación vigente de plugins, skills y hooks de Codex (enlaces en la sección 11). Si algo de este documento contradice la documentación, **gana la documentación**: aplícala y anota la discrepancia en tu informe.
3. Resume en 10 líneas como máximo qué vas a construir, qué queda fuera y qué dudas tienes.
4. **Detente y espera confirmación.** No empieces la Fase A sin un "adelante".
Trabaja por fases (sección 9). Al terminar cada una, detente y entrega el informe de fase (sección 10).
 
---
 
## 1. Objetivo
 
Construir el MVP de un plugin llamado `synthetic-research-lab` que permita a UX Researchers:
 
- crear **usuarios sintéticos** (personas) en dos modos: anclado en datos reales y desde descripción;
- generar **transcripciones sintéticas de entrevista** en bruto a partir de esas personas y de una guía de entrevista;
- generar **respuestas sintéticas de encuesta** para probar instrumentos y pipelines cuantitativos;
- generar **sesiones sintéticas de workshop** para ensayar dinámicas, materiales y análisis;
- mantener todo lo sintético **separado, marcado y trazable**, de forma que nunca pueda confundirse con evidencia real.
El MVP es la base de un ecosistema más amplio (encuestas, workshops, netnografía) que se construirá después sobre el mismo contrato de datos.
 
---
 
## 2. Contexto metodológico
 
El plugin operacionaliza un workflow de research en dos fases:
 
- **Exploración** (lineal): Explorar → Necesidades de usuario → Insights → Priorizar → Conceptualizar → Hipótesis → Prototipar & testear → Lanzar.
- **Validación** (ciclo): Lanzar → Evaluar → Idear → Experimentar & testear → Implementar → Lanzar. Los aprendizajes vuelven a la exploración. Research Ops es una capa transversal.
Los usuarios sintéticos tienen seis usos previstos:
 
| # | Momento del workflow | Uso |
|---|---|---|
| 1 | Explorar | Pre-exploración de actitudes y motivaciones para enfocar la investigación con usuarios reales |
| 2 | Priorizar | Retar hipótesis de negocio y propuestas de valor |
| 3 | Conceptualizar | Detección temprana de fricciones y barreras |
| 4 | Conceptualizar | Chequeos rápidos de elementos tácticos (naming, microcopy) |
| 5 | Prototipar & testear | Pretest de conceptos, copies, narrativas y campañas |
| 6 | Idear | Personas conversacionales para ejercicios de ideación |
 
**Principio rector:** lo sintético sirve para **generar y filtrar** en decisiones tempranas, baratas y reversibles. **Nunca valida.** Las respuestas las dan usuarios reales.
 
**Por qué importa para el diseño.** Los modelos que simulan usuarios tienden a ser complacientes con los conceptos que se les presentan, reducen la variabilidad de las respuestas, eliminan contradicciones y arrastran un sesgo cultural anglosajón. Los agentes anclados en entrevistas reales se comportan mejor que los construidos solo con una descripción demográfica. El plugin debe contrarrestar activamente estas tendencias, no solo evitarlas.
 
---
 
## 3. Reglas no negociables
 
Se aplican a todo el código, las skills y los outputs. Cópialas en el `AGENTS.md` del repositorio durante la Fase A.
 
1. **Marca visible y legible por máquina.** Todo artefacto sintético lleva un bloque `provenance` con `synthetic: true` y, al inicio, el banner de texto `SINTÉTICO — No es evidencia de usuarios reales`. El banner no puede depender solo de un emoji o de un color.
2. **Nomenclatura separada.** Las referencias sintéticas usan el prefijo `SYN-`: `[SYN-E02, min 04:10]`, `[SYN-P03]`. Nunca se reutilizan los formatos de referencia reales (`[E2, min 4:10]`, `[Enc, P7, n=45]`).
3. **Estatus epistémico fijo.** Todo output sintético tiene `evidence_status: "Hipótesis"`. Nada sintético puede etiquetarse como Hallazgo ni como Patrón.
4. **`research/real/` es de solo lectura para el plugin.** Ninguna skill escribe ahí. Todo output va a `research/synthetic/`.
5. **Trazabilidad por atributo.** En modo anclado, cada rasgo de una persona indica su origen: referencia a una fuente real concreta, supuesto de la descripción o inferencia del modelo. **No inventes referencias a fuentes.** Si un rasgo no tiene respaldo, se marca como inferencia.
6. **Nombrar lo que falta.** Cada persona incluye un campo de lagunas con lo que los datos no cubren. El silencio en los datos no se interpreta.
7. **Preservar tensiones.** Las contradicciones entre participantes, o dentro de un mismo participante, se conservan como campos propios. No se promedian.
8. **Datos personales.** Los outputs sintéticos no reproducen nombres, lugares identificables ni citas literales largas de participantes reales. Las fuentes reales se referencian por código, no se copian.
9. **Colectivos vulnerables y personas con discapacidad.** Si se pide simular estos perfiles, la skill solo lo hace en modo anclado o avisa explícitamente del riesgo alto de estereotipo y lo registra en `usage_limits`. Nunca se presenta como sustituto de investigación con esas personas.
10. **No inventar metadatos.** Si no conoces el modelo o un parámetro, escribe `"desconocido"`.
11. **Idioma.** Contenido dirigido a personas en español. Claves JSON, nombres de archivo e identificadores en inglés, sin tildes.
---
 
## 4. Alcance del MVP
 
**Dentro:**
 
- manifiesto del plugin y marketplace local de pruebas;
- contrato de datos: schemas y validador;
- skills `lab-setup`, `persona-builder`, `interview-synth`, `survey-synth`, `workshop-synth` y `governance-check`;
- hooks de protección de `research/real/` y de validación de `research/synthetic/`;
- tests, fixtures y evals;
- `README.md` con notas de portabilidad.
**Fuera (no construir ahora):** `netnography-synth`, `workflow-uses`, servidores MCP, interfaz visual, publicación en el directorio público de plugins y versión para Claude Code.
 
Diseña el MVP de forma que añadir las piezas que siguen fuera de alcance después no obligue a cambiar el contrato común de procedencia de la sección 7.
 
---
 
## 5. Requisitos técnicos de Codex
 
Verifica cada punto contra la documentación vigente antes de aplicarlo.
 
- **Manifiesto portátil** obligatorio en `plugin.json`, en la raíz del plugin, siguiendo el schema de Agent Plugins. Se mantiene `.codex-plugin/plugin.json` como overlay de compatibilidad con Codex. Dentro de `.codex-plugin/` solo va ese archivo; `skills/`, `hooks/` y `assets/` van en la raíz del plugin.
- **Rutas** del manifiesto relativas a la raíz del plugin y empezando por `./`.
- **Nombre** estable en kebab-case: `synthetic-research-lab`. Versión inicial `0.1.0`.
- **Hooks** en `hooks/hooks.json` (ruta por defecto). Los comandos usan `${PLUGIN_ROOT}`. Codex también define `CLAUDE_PLUGIN_ROOT`, lo que facilita la portabilidad futura.
- **Confianza en hooks.** Los hooks de un plugin no se ejecutan hasta que el usuario los revisa y aprueba (`/hooks` en la CLI). Documéntalo en el README. Trata los hooks como una salvaguarda, no como una frontera de seguridad completa: las reglas también deben estar en las skills y en el validador.
- **Andamiaje.** Puedes usar `@plugin-creator` para generar el esqueleto y el marketplace local. Si lo haces, revisa el resultado y ajústalo a este documento.
- **Marketplace de pruebas** en `.agents/plugins/marketplace.json` de este repo, con `source.path` apuntando a `./plugins/synthetic-research-lab` e incluyendo `policy.installation`, `policy.authentication` y `category`.
- **Portabilidad.** El frontmatter de cada `SKILL.md` lleva solo `name` y `description`. Nada específico de Codex dentro del `SKILL.md`. Si hace falta configuración específica, va en archivos aparte y se documenta en el README.
- **Scripts** en Python 3 con biblioteca estándar. Si una dependencia externa es imprescindible, decláralo en el README y haz que el script falle con un mensaje claro si falta. Tests con `unittest`.
---
 
## 6. Estructura objetivo
 
### 6.1 Repositorio del plugin
 
```
synthetic-research-lab/                  ← raíz del repo
├── AGENTS.md                            ← reglas de la sección 3 para Codex
├── PLAN.md                              ← este documento
├── README.md
├── .agents/plugins/marketplace.json     ← marketplace local de pruebas
├── plugins/
│   └── synthetic-research-lab/
│       ├── plugin.json                  ← manifiesto portátil canónico
│       ├── .codex-plugin/plugin.json     ← compatibilidad con Codex
│       ├── skills/
│       │   ├── lab-setup/
│       │   │   ├── SKILL.md
│       │   │   └── assets/              ← plantilla de carpetas y AGENTS.md de proyecto
│       │   ├── persona-builder/
│       │   │   ├── SKILL.md
│       │   │   ├── references/          ← schemas, ejemplo, guía de anclaje
│       │   │   └── scripts/check_refs.py
│       │   ├── interview-synth/
│       │   │   ├── SKILL.md
│       │   │   └── references/          ← formato de transcripción, niveles de realismo
│       │   ├── survey-synth/
│       │   │   ├── SKILL.md
│       │   │   └── references/          ← formato de respuestas y variabilidad
│       │   ├── workshop-synth/
│       │   │   ├── SKILL.md
│       │   │   └── references/          ← formato de sesión, roles y dinámica
│       │   └── governance-check/
│       │       ├── SKILL.md
│       │       ├── references/          ← schemas
│       │       └── scripts/validate.py
│       ├── hooks/
│       │   ├── hooks.json
│       │   ├── guard_real.py
│       │   └── validate_synthetic.py
│       └── assets/
├── schemas/                             ← fuente única de los schemas
├── scripts/sync_schemas.py              ← copia schemas/ a las references/ de cada skill
├── tests/
│   ├── fixtures/                        ← datos inventados, marcados como fixture
│   └── test_*.py
└── evals/
    └── evals.json
```
 
Cada skill debe funcionar de forma autónoma, así que lleva su propia copia de los schemas que usa. La fuente única está en `schemas/`, y `scripts/sync_schemas.py` sincroniza las copias. Un test comprueba que no divergen.
 
### 6.2 Proyecto del researcher (lo crea `lab-setup`)
 
```
<proyecto>/
├── AGENTS.md                  ← reglas de uso de lo sintético en este proyecto
└── research/
    ├── real/                  ← el researcher deposita aquí sus datos; el plugin solo lee
    │   ├── interviews/
    │   ├── surveys/
    │   └── other/
    └── synthetic/             ← único destino de escritura del plugin
        ├── personas/
        ├── transcripts/
        ├── surveys/
        ├── workshops/
        └── _log.jsonl         ← registro append-only de cada generación
```
 
---
 
## 7. Contrato de datos
 
### 7.1 Bloque `provenance` (común a todos los artefactos)
 
```json
{
  "provenance": {
    "synthetic": true,
    "artifact_type": "persona | transcript | survey | workshop",
    "artifact_id": "SYN-P03",
    "mode": "anchored | description",
    "anchored_sources": ["E3", "E5", "Enc-P7"],
    "workflow_use": "pre_exploration | challenge_hypotheses | friction_scan | tactical_check | pretest | ideation | instrument_testing",
    "generated_by": "synthetic-research-lab/persona-builder@0.1.0",
    "model": "desconocido",
    "generated_at": "2026-09-10T10:00:00Z",
    "parameters": {},
    "evidence_status": "Hipótesis",
    "usage_limits": ["No usar para validar ni para priorizar decisiones finales"]
  }
}
```
 
Reglas del bloque:
 
- `synthetic` siempre es `true`.
- `evidence_status` siempre es `"Hipótesis"`.
- `anchored_sources` va vacío si `mode` es `description`, y no puede ir vacío si es `anchored`.
- Encuestas y workshops heredan el modo de sus personas y solo usan `anchored` cuando todas las entradas necesarias tienen fuentes reales trazables.
- `usage_limits` tiene al menos un elemento.
- Los valores de `workflow_use` corresponden a los seis usos de la sección 2, más `instrument_testing` para probar guías, cuestionarios y pipelines de análisis.
### 7.2 Persona
 
Cada persona se guarda en dos archivos: `research/synthetic/personas/SYN-P03.json` (fuente de verdad) y `SYN-P03.md` (lectura humana).
 
Campos mínimos del JSON:
 
| Campo | Contenido |
|---|---|
| `provenance` | Bloque 7.1 |
| `id` | `SYN-P03` |
| `display_name` | Nombre ficticio, marcado como tal |
| `segment` | Segmento o perfil |
| `context_of_use` | Contexto en que usa el producto o servicio |
| `behaviors`, `motivations`, `frictions`, `expectations` | Listas de rasgos (formato abajo) |
| `tensions` | Contradicciones conservadas, con los rasgos o fuentes implicados |
| `gaps` | Lo que los datos no cubren |
| `voice_notes` | Cómo habla: registro, vocabulario, nivel de detalle. Sin copiar citas reales |
 
Formato de cada rasgo:
 
```json
{
  "text": "Revisa el saldo varias veces al día desde el móvil",
  "kind": "declared_behavior | stated_desire | attitude",
  "origin": {
    "type": "real_source | description_assumption | model_inference",
    "ref": "E3, min 12:40"
  }
}
```
 
- `kind` distingue lo que el participante dice que hace (`declared_behavior`) de lo que dice que querría o haría (`stated_desire`).
- Si `origin.type` es `real_source`, `ref` es obligatorio y debe apuntar a un archivo existente en `research/real/`. En los demás casos, `ref` es `null`.
La versión `.md` lleva el banner de sintético, una jerarquía de encabezados sin saltos de nivel, tablas con fila de cabecera y el origen de cada rasgo visible en texto.
 
### 7.3 Transcripción sintética
 
Se guarda en `research/synthetic/transcripts/SYN-E01.md`. Es una transcripción **en bruto**, con hablantes identificados, para que pueda pasar por las herramientas de limpieza y análisis que el equipo usa con transcripciones reales.
 
```markdown
---
provenance:
  synthetic: true
  artifact_type: transcript
  artifact_id: SYN-E01
  # ...resto del bloque 7.1
persona_id: SYN-P03
guide: research/real/other/guia_onboarding.md
realism: medio
---
 
> SINTÉTICO — No es evidencia de usuarios reales. Transcripción generada para pruebas.
 
[00:00] Entrevistador: Hola, ¿me oyes bien?
[00:04] Participante: Sí, sí, perfecto. Bueno, hay un poco de eco, pero bien.
```
 
- Cada turno lleva timestamp `[mm:ss]`, para poder referenciar `[SYN-E01, min 04:10]`.
- Las etiquetas de hablante son fijas: `Entrevistador:` y `Participante:`.
 
### 7.4 Encuesta sintética

Cada ejecución se guarda en `research/synthetic/surveys/SYN-S01.json` como fuente de verdad y en `SYN-S01.csv` para probar herramientas cuantitativas. El JSON incluye `provenance`, `id`, `instrument`, `persona_ids`, `responses`, `gaps` y `quality_notes`. Cada respuesta conserva el identificador sintético de quien responde y distingue respuestas directas, supuestos e inferencias. El CSV incluye una columna `synthetic_banner` cuyo valor legible es `SINTÉTICO — No es evidencia de usuarios reales`; no se presenta como estimación poblacional ni se usan porcentajes como evidencia.

### 7.5 Workshop sintético

Cada sesión se guarda en `research/synthetic/workshops/SYN-W01.md`. Incluye frontmatter con `provenance`, `persona_ids`, `guide`, `activity` y `duration_minutes`, seguido del banner obligatorio. El cuerpo identifica facilitación y participantes sintéticos, conserva desacuerdos, registra decisiones como hipótesis y separa observaciones generadas de preguntas que deben validarse con personas reales.
 
---
 
## 8. Especificación de las skills
 
Pautas comunes para todas las skills:
 
- Escribe la `description` en español y en tercera persona. Debe decir qué hace la skill **y cuándo activarse**, con las frases que usaría un researcher. Sé explícito: es preferible que la skill se active de más a que no se active cuando hace falta.
- Escribe el cuerpo del `SKILL.md` en imperativo, por debajo de 500 líneas. Explica el porqué de las reglas, no solo la regla, y remite a `references/` para el detalle.
- Todas las skills que generan artefactos terminan ejecutando `governance-check` y añadiendo una línea a `_log.jsonl`.
### 8.1 `lab-setup`
 
- **Hace:** crea la estructura de la sección 6.2 en el proyecto actual, con un `AGENTS.md` de proyecto que explica las reglas de la sección 3 en lenguaje para researchers, y un `_log.jsonl` vacío.
- **Comportamiento:** si `research/` ya existe, no sobrescribe nada. Informa de lo que falta y pregunta antes de crearlo.
- **Borrador de `description`:** "Prepara un proyecto de UX Research para trabajar con usuarios y datos sintéticos, creando carpetas separadas para datos reales y sintéticos y las reglas de uso del proyecto. Úsala cuando el usuario quiera empezar a usar usuarios sintéticos en un proyecto, configurar el laboratorio sintético o preparar la estructura de carpetas, y siempre antes de usar persona-builder o interview-synth en un proyecto nuevo."
### 8.2 `persona-builder`
 
**Entradas:** modo (`anchored` o `description`); segmento u objetivo; en modo anclado, qué archivos de `research/real/` usar; uso previsto del workflow (sección 2); número de personas.
 
**Antes de generar, para y pregunta** si falta alguno de estos datos: objetivo, uso previsto o, en modo anclado, las fuentes. En modo anclado, pregunta también si las transcripciones no tienen hablantes identificados o tienen fragmentos ilegibles. Con menos de 3 fuentes reales, avisa de que el anclaje es débil antes de continuar.
 
**Modo anclado:**
 
- Extrae los rasgos con su referencia exacta a la fuente.
- Conserva las tensiones y los rasgos minoritarios. No construyas un "usuario medio".
- Si generas varias personas, cada una refleja una combinación distinta de rasgos reales, e indica qué participantes reales la sostienen.
**Modo descripción:**
 
- Marca todos los rasgos como `description_assumption` o `model_inference`.
- Añade a `usage_limits` que la persona no tiene respaldo empírico.
- Introduce diversidad de forma explícita: al menos un rasgo contraintuitivo o una tensión por persona.
- Avisa si el segmento descrito es propenso a estereotipos.
**Contexto cultural:** si el segmento es de España o de otro contexto no estadounidense, anótalo en `usage_limits`, porque la fidelidad cultural de los modelos es menor fuera de Estados Unidos.
 
**Salidas:** `.json` y `.md` por persona, validados, más una línea en `_log.jsonl`.
 
**Script:** `scripts/check_refs.py` verifica que cada `ref` de tipo `real_source` apunta a un archivo existente en `research/real/`.
 
### 8.3 `interview-synth`
 
**Entradas:** una o varias personas `SYN-P*`, una guía de entrevista, nivel de realismo (`bajo`, `medio` o `alto`) y duración aproximada.
 
**Genera** la transcripción en el formato 7.3, con estas reglas:
 
- El participante responde **como la persona**, respetando sus tensiones y lagunas.
- Si la guía pregunta por algo que la persona no cubre (`gaps`), el participante responde con vaguedad o duda, no con una respuesta segura inventada.
- **Realismo.** El nivel controla la proporción de muletillas, saludos, comprobaciones técnicas ("¿se me oye?"), interrupciones, digresiones, autocorrecciones y respuestas vagas. En `alto` incluye al menos una contradicción del participante y un momento en que no entiende la pregunta. Documenta los niveles en `references/realism.md`.
- **Anticomplacencia.** Si la guía presenta un concepto o una propuesta, el participante expresa al menos una objeción o duda coherente con sus fricciones. El entrevistador sintético no hace preguntas sugestivas salvo que estén en la guía.
**Usos:** probar una guía antes de ir a campo y probar el pipeline de análisis del equipo, incluida la limpieza de transcripciones.
 
**Salidas:** el `.md` validado y una línea en `_log.jsonl`.
 
### 8.4 `survey-synth`

**Entradas:** una o varias personas `SYN-P*`, un cuestionario o instrumento, número de respuestas y uso previsto.

**Genera** respuestas sintéticas variadas para probar redacción, lógica, exportación y pipelines de análisis. Nunca estima prevalencia real ni presenta porcentajes como evidencia. Preserva tensiones de las personas, introduce no respuesta cuando falte contexto y evita completar de forma complaciente opciones sugestivas.

**Salidas:** `SYN-S*.json` y `SYN-S*.csv`, validados, más una línea en `_log.jsonl`.

### 8.5 `workshop-synth`

**Entradas:** personas `SYN-P*`, guía de workshop, actividad, duración aproximada y uso previsto.

**Genera** una sesión con facilitación, aportaciones por participante, desacuerdos, bloqueos y preguntas abiertas. No fuerza consenso ni convierte decisiones del grupo sintético en hallazgos. Los materiales y las instrucciones usan lenguaje claro, neutral e inclusivo y contemplan necesidades de accesibilidad.

**Salida:** `SYN-W*.md` validado y una línea en `_log.jsonl`.

### 8.6 `governance-check`
 
**Hace:** valida uno o varios artefactos contra los schemas y las reglas de la sección 3, y devuelve un informe con errores (bloquean) y avisos (no bloquean).
 
**Errores:**
 
- falta el bloque `provenance`;
- `synthetic` distinto de `true` o `evidence_status` distinto de `"Hipótesis"`;
- falta el banner de texto;
- referencias sintéticas sin prefijo `SYN-`;
- `ref` de tipo `real_source` que no existe;
- artefacto sintético dentro de `research/real/`.
**Avisos:**
 
- persona sin tensiones o sin lagunas;
- modo descripción sin rasgos de diversidad;
- `usage_limits` genérico;
- fragmentos que parecen copiados literalmente de una fuente real (más de 12 palabras seguidas coincidentes).
**Script:** `scripts/validate.py <ruta>` devuelve código de salida 0 si no hay errores y 1 si los hay. Lo reutilizan los hooks.
 
**Borrador de `description`:** "Comprueba que los artefactos sintéticos de research (personas, transcripciones) están correctamente marcados, trazados y separados de los datos reales. Úsala siempre después de generar un artefacto sintético, cuando el usuario pida revisar, auditar o validar datos sintéticos, y antes de compartir material sintético con otras personas."
 
### 8.7 Hooks
 
- **`PreToolUse` sobre `apply_patch`, `Edit` y `Write`:** `guard_real.py` extrae las rutas del parche y **deniega** cualquier escritura en `research/real/`, con un motivo claro para el usuario.
- **`PreToolUse` sobre `Bash`:** el mismo script detecta de forma heurística redirecciones, `cp`, `mv`, `tee` o `rm` hacia `research/real/` y las deniega. Documenta que es una detección heurística.
- **`PostToolUse` sobre `apply_patch`, `Edit` y `Write` en `research/synthetic/`:** `validate_synthetic.py` ejecuta el validador. Si falla, devuelve un bloqueo con la lista de errores para que Codex los corrija.
- **Formato de salida:** el que indique la documentación de hooks de Codex (por ejemplo, `hookSpecificOutput.permissionDecision: "deny"`, o código de salida 2 con el motivo en `stderr`). Verifícalo antes de implementarlo.
- **Tests:** tests unitarios que simulen la entrada JSON de cada evento.
---
 
## 9. Fases de ejecución
 
Al final de cada fase: ejecuta los tests, entrega el informe de la sección 10 y **detente**.
 
### Fase A — Verificación y esqueleto
 
Consulta la documentación. Crea `AGENTS.md`, el manifiesto, el marketplace local y las carpetas de la sección 6.1.
 
**Listo cuando:** el plugin aparece en el directorio de plugins de Codex tras reiniciar, aunque todavía no tenga skills funcionales.
 
### Fase B — Contrato de datos
 
Escribe los JSON Schemas (provenance, persona, transcript), `validate.py`, `sync_schemas.py` y sus tests.
 
Crea las fixtures en `tests/fixtures/`, inventadas y marcadas con `fixture: true` en su cabecera:
 
- un proyecto de prueba en `tests/fixtures/project/` cuya carpeta `research/real/` contiene 3 transcripciones cortas de ejemplo que hacen de "datos reales", con al menos una tensión entre participantes;
- una persona válida;
- varias personas inválidas, una por cada tipo de error de la sección 8.4.
Las fixtures nunca se copian a un proyecto real; indícalo en su README.
 
**Listo cuando:** el validador acepta la persona válida, rechaza cada inválida con el error esperado y los tests pasan.
 
### Fase C — `lab-setup`
 
**Listo cuando:** en una carpeta temporal crea la estructura completa de la sección 6.2 y, ejecutada una segunda vez, no sobrescribe nada.
 
### Fase D — `persona-builder`
 
**Listo cuando**, usando el proyecto de prueba:
 
- en modo anclado genera dos personas en las que cada rasgo tiene origen, cada `ref` existe y se conserva al menos una tensión;
- en modo descripción genera una persona con todos los rasgos marcados como supuesto o inferencia;
- todo pasa `governance-check`.
### Fase E — `interview-synth`
 
**Listo cuando:**
 
- genera una transcripción de realismo `medio` y otra de `alto` para la misma persona y la misma guía;
- las dos pasan el validador;
- la de `alto` contiene al menos una contradicción, una comprobación técnica y una objeción al concepto;
- el participante no afirma con seguridad nada que la persona tenga en `gaps`.
### Fase F — `survey-synth`

**Listo cuando:**

- genera respuestas JSON y CSV para un mismo instrumento;
- representa variación, no respuesta y al menos una tensión;
- no formula resultados como prevalencia ni evidencia;
- ambos artefactos pasan `governance-check`.

### Fase G — `workshop-synth`

**Listo cuando:**

- genera una sesión con al menos tres participantes sintéticos;
- conserva un desacuerdo sin forzar consenso;
- registra hipótesis y preguntas pendientes por separado;
- el artefacto pasa `governance-check`.

### Fase H — Hooks
 
**Listo cuando** los tests demuestran que:
 
- una escritura en `research/real/` se deniega vía `apply_patch` y en los casos típicos de `Bash`;
- un artefacto sintético sin `provenance` en `research/synthetic/` provoca el bloqueo de `PostToolUse`.
### Fase I — Evals
 
Crea `evals/evals.json` con 3 prompts realistas por skill, escritos como los escribiría un researcher, en español, junto con el resultado esperado de cada uno.
 
Incluye al menos un caso trampa por skill, por ejemplo:
 
- pedir que lo sintético se etiquete como Hallazgo;
- pedir una persona "del usuario medio";
- aportar fuentes con hablantes sin identificar;
- aportar menos de 3 fuentes en modo anclado.
Ejecuta los prompts y guarda los outputs en `evals/runs/`.
 
**Listo cuando:** hay outputs para todos los prompts y una tabla que indica, para cada uno, si cumple lo esperado y por qué.
 
### Fase J — Documentación
 
Escribe el `README.md` con:
 
- instalación local y aprobación de hooks (`/hooks`);
- flujo de uso típico: `lab-setup` → `persona-builder` → (`interview-synth` | `survey-synth` | `workshop-synth`) → `governance-check`;
- las reglas de la sección 3 en lenguaje para researchers, incluidos los límites de uso;
- una sección "Portabilidad a Claude Code" que indique qué se reutiliza tal cual y qué habría que añadir.
**Listo cuando:** una persona que no ha visto el proyecto puede instalarlo y generar su primera persona siguiendo solo el README.
 
---
 
## 10. Informe de fase
 
Al terminar cada fase, entrega este informe:
 
```
Fase X — [nombre]
 
Hecho:              [qué se construyó; archivos creados o modificados]
Verificado:         [tests ejecutados y resultado; criterios de "listo" cumplidos o no]
Discrepancias:      [diferencias entre este documento y la documentación vigente, y qué decidiste]
Dudas / decisiones: [lo que necesitas que decida el equipo]
Siguiente:          [qué harás en la próxima fase]
```
 
No pases a la siguiente fase sin confirmación. Si un criterio de "listo" no se cumple, dilo claramente; no lo des por cumplido.
 
---
 
## 11. Referencias
 
- Construir plugins en Codex: https://developers.openai.com/codex/plugins/build
- Empaquetar un plugin: https://developers.openai.com/plugins/build/plugins
- Hooks de Codex: https://developers.openai.com/codex/hooks
- Skills en Codex: https://developers.openai.com/codex/skills
- Ejemplos oficiales de plugins: https://github.com/openai/plugins
- Estándar abierto de Agent Skills: https://agentskills.io
