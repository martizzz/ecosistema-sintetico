# Synthetic Research Lab

> SINTÉTICO — Este repositorio contiene herramientas para generar material sintético. No produce evidencia de usuarios reales.

Synthetic Research Lab es un complemento local para Codex. Está pensado para equipos de investigación de experiencia de usuario (UX Research).

Su objetivo es ayudar a preparar y probar investigaciones antes de trabajar con personas reales. Podrá crear personas ficticias, entrevistas simuladas, respuestas de encuesta y sesiones de workshop.

No necesitas saber programar para entender esta guía. Cuando aparezca una instrucción técnica, encontrarás una explicación y un texto que puedes copiar.

## Estado actual del proyecto

El proyecto está en desarrollo. La **Fase A está terminada**.

En este momento puedes:

- añadir el catálogo local a Codex;
- ver Synthetic Research Lab en el directorio de plugins;
- revisar la estructura y las reglas del proyecto.

Todavía no puedes generar personas, entrevistas, encuestas o workshops con el plugin. Estas funciones se añadirán en las siguientes fases. Los controles automáticos también están pendientes.

## Para qué servirá

Cuando el desarrollo esté completo, el complemento permitirá:

- preparar una carpeta de trabajo separada para datos reales y sintéticos;
- crear personas sintéticas desde una descripción o a partir de fuentes reales;
- simular entrevistas para probar una guía;
- generar respuestas para probar un cuestionario o un proceso de análisis;
- ensayar una dinámica de workshop;
- comprobar que cada archivo sintético esté marcado y sea trazable.

El material sintético sirve para explorar, ensayar y detectar problemas tempranos. **No sirve para validar una decisión ni sustituye la investigación con personas reales.**

## Conceptos básicos

### Qué es Codex

Codex es la aplicación donde puedes pedir tareas con lenguaje normal. Por ejemplo: “Prepara un laboratorio de research sintético”.

### Qué es un plugin

Un plugin, o complemento, añade capacidades a Codex. Synthetic Research Lab reúne instrucciones, reglas y controles para trabajar con material sintético.

### Qué es un repositorio

Es la carpeta principal de este proyecto. Contiene la documentación y los archivos necesarios para construir el complemento.

### Qué significa sintético

Significa que el contenido ha sido simulado. No procede directamente de una persona participante y no es evidencia real.

## Cómo probar lo que ya existe

Necesitas la aplicación de escritorio de ChatGPT con Codex y acceso a su terminal integrada.

### 1. Abre el repositorio en Codex

Abre esta carpeta como proyecto:

`Ecosistema Sintético`

### 2. Abre la terminal integrada

La terminal es un cuadro donde puedes pegar una instrucción. No necesitas entender el comando para esta prueba.

### 3. Añade el catálogo local

Copia y pega este comando. Después, pulsa Intro:

```text
codex plugin marketplace add "/Users/martdiaz/Documents/ChatGPT/Ecosistema Sintético"
```

Este paso informa a Codex de dónde debe leer el catálogo del proyecto. No publica el complemento en Internet.

### 4. Reinicia la aplicación

Cierra y vuelve a abrir la aplicación de escritorio. El reinicio permite que Codex vuelva a leer el catálogo.

### 5. Busca el complemento

Abre el directorio de plugins, elige la fuente **Personal** y busca **Synthetic Research Lab**.

Si aparece, la prueba de la Fase A ha terminado correctamente. Instalarlo ahora no activa las funciones de generación, porque todavía no están desarrolladas.

## Cómo se utilizará cuando esté completo

Esta sección describe el flujo previsto. **Aún no está disponible.**

Podrás escribir peticiones normales en español. No será necesario usar comandos técnicos.

### Paso 1. Preparar el espacio de trabajo

Petición de ejemplo:

> Prepara este proyecto para trabajar con research sintético.

La función `lab-setup` creará dos zonas separadas:

- `research/real/`: datos de participantes reales. El complemento solo podrá leerlos.
- `research/synthetic/`: personas, entrevistas, encuestas y workshops simulados.

### Paso 2. Crear una persona sintética

Habrá dos modos:

- **Modo anclado:** usa fuentes reales como referencia. Cada rasgo indica su origen.
- **Modo descripción:** parte de la descripción que tú aportes. No tiene respaldo empírico.

Petición de ejemplo:

> Crea dos personas sintéticas ancladas en las entrevistas que están en `research/real/interviews/`. Quiero probar una guía de onboarding.

Antes de generar, Codex pedirá la información esencial que falte. Si hay pocas fuentes o riesgo de estereotipos, mostrará un aviso.

### Paso 3. Elegir qué quieres ensayar

Con una persona sintética ya creada, podrás pedir una de estas tareas:

- **Entrevista:** “Simula una entrevista de 30 minutos con realismo medio usando esta guía”.
- **Encuesta:** “Genera respuestas variadas para probar la lógica de este cuestionario”.
- **Workshop:** “Ensaya esta actividad con tres personas sintéticas y conserva los desacuerdos”.

### Paso 4. Revisar el resultado

La función `governance-check` comprobará que el material esté bien marcado, separado y trazado.

Petición de ejemplo:

> Revisa la gobernanza de los artefactos sintéticos antes de compartirlos.

El flujo completo será:

```text
Preparar el proyecto
        ↓
Crear personas sintéticas
        ↓
Simular entrevista, encuesta o workshop
        ↓
Revisar la gobernanza
```

## Reglas de uso responsable

Estas reglas son obligatorias:

1. Todo material sintético debe mostrar el texto “SINTÉTICO — No es evidencia de usuarios reales”.
2. Todo material sintético tiene el estado “Hipótesis”. Nunca se etiqueta como hallazgo o patrón.
3. Los identificadores sintéticos empiezan por `SYN-`. Por ejemplo, `SYN-P03`.
4. Los datos reales y los sintéticos se guardan en carpetas diferentes.
5. El complemento nunca escribe dentro de `research/real/`.
6. En modo anclado, cada rasgo indica si procede de una fuente, de un supuesto o de una inferencia.
7. Si una fuente no cubre un tema, el resultado debe indicar esa laguna. No debe inventar una respuesta.
8. Las contradicciones se conservan. No se crea un supuesto “usuario medio”.
9. No se copian nombres, lugares identificables ni citas largas de participantes reales.
10. Simular colectivos vulnerables o personas con discapacidad tiene un riesgo alto de estereotipo. Se requieren fuentes reales y el resultado nunca sustituye su participación.

## Cómo reconocer un resultado sintético

Un resultado correcto tendrá varias señales de texto. No dependerá solo de un color o un icono.

- Un aviso visible al inicio.
- Un identificador con el prefijo `SYN-`.
- El estado `Hipótesis`.
- Una sección de procedencia llamada `provenance`.
- Una lista de límites de uso.

Si falta alguna de estas señales, no compartas el archivo como resultado válido.

## Cómo están organizados los archivos

- `README.md`: esta guía.
- `PLAN.md`: plan detallado de construcción y fases pendientes.
- `AGENTS.md`: reglas obligatorias que debe seguir Codex.
- `plugins/synthetic-research-lab/`: archivos propios del complemento.
- `.agents/plugins/marketplace.json`: ficha que permite mostrar el complemento en el catálogo local.
- `schemas/`: reglas que definirán el formato válido de los resultados.
- `tests/`: comprobaciones automáticas del funcionamiento.
- `evals/`: casos de prueba escritos como peticiones reales.
- `scripts/`: pequeñas herramientas de apoyo.

Varias carpetas están vacías porque pertenecen a fases que aún no se han desarrollado.

## Cómo funciona por dentro

Cuando esté terminado, Codex seguirá este proceso:

1. Interpretará tu petición y elegirá la función adecuada.
2. Comprobará si tiene la información necesaria.
3. Leerá solo las fuentes que hayas indicado.
4. Creará el resultado dentro de `research/synthetic/`.
5. Añadirá la procedencia, los límites y la marca de contenido sintético.
6. Ejecutará una revisión automática.
7. Registrará la generación en un historial llamado `_log.jsonl`.

Habrá tres capas de protección: las instrucciones de cada función, un validador y controles automáticos llamados hooks. Estas capas reducen errores, pero no sustituyen la revisión humana.

## Solución de problemas

### El complemento no aparece

Comprueba lo siguiente:

1. Abriste este repositorio en Codex.
2. Pegaste el comando completo sin cambiar la ruta.
3. Reiniciaste la aplicación después de añadir el catálogo.
4. Elegiste la fuente **Personal** en el directorio de plugins.

Si cambias de ordenador o mueves la carpeta, sustituye la ruta del comando por la nueva ubicación.

### El complemento aparece, pero no genera nada

Es el comportamiento esperado en la Fase A. Las funciones de generación todavía no existen.

### Codex pide revisar los hooks

Los hooks son controles automáticos del complemento. Cuando estén implementados, abre `/hooks` en la interfaz de línea de comandos de Codex, revisa su origen y apruébalos. Codex no confía automáticamente en los hooks incluidos en un plugin.

### Tengo dudas sobre un resultado

No lo uses como evidencia. Comprueba el aviso, el identificador `SYN-`, el estado “Hipótesis”, la procedencia y los límites de uso.

## Portabilidad a Claude Code

El proyecto se diseña para reutilizar su estructura principal en Claude Code. Las instrucciones, las skills y los formatos de datos podrán mantenerse en gran parte.

Será necesario adaptar la configuración específica del complemento y comprobar el formato de los hooks. Esta compatibilidad es futura y no forma parte del producto actual.

## Documentación de referencia

- [Cómo empaquetar y probar plugins de forma local](https://developers.openai.com/plugins/build/plugins)
- [Cómo revisar y aprobar hooks en Codex](https://learn.chatgpt.com/docs/hooks)
- [Plan completo del proyecto](PLAN.md)

## Revisión de accesibilidad (WCAG 2.1 AA)

**Criterios cumplidos**

- [WCAG 2.1 — 3.1.5 Nivel de lectura — AAA] — Se usa lenguaje directo y se explican los términos técnicos.
- [WCAG 2.1 — 1.3.1 Información y relaciones — A] — Los encabezados siguen un orden y las listas agrupan pasos relacionados.
- [WCAG 2.1 — 1.4.1 Uso del color — A] — Los estados y avisos se comunican con texto, no solo con color o iconos.
- [WCAG 2.1 — 2.4.6 Encabezados y etiquetas — AA] — Los títulos describen el contenido de cada sección.

**Consideraciones adicionales**

- Los nombres de carpetas y funciones se mantienen en inglés porque forman parte del sistema.
- El paso de instalación usa una terminal. Se incluye un único comando listo para copiar y una explicación de su efecto.
