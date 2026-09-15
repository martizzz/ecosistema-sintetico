# Reglas del repositorio: Synthetic Research Lab

Estas reglas son obligatorias para Codex y para cualquier skill o script del plugin.

## Reglas no negociables

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

## Accesibilidad e inclusión

- Redacta guías, preguntas y materiales con lenguaje claro, preferentemente B2 o inferior, frases breves y voz activa.
- Evita preguntas sugestivas, dobles negaciones y supuestos sobre capacidades, dispositivo, canal o contexto.
- En encuestas, usa anclas textuales en las escalas e incluye `Prefiero no responder` cuando corresponda.
- En workshops, contempla pausas, formatos alternativos y necesidades de accesibilidad sin convertir la discapacidad en un estereotipo.
- Mantén jerarquías de encabezados sin saltos y tablas con encabezados explícitos. No uses color o iconos como única señal.

## Alcance y ejecución

- Sigue las fases y puntos de parada de `PLAN.md`; no avances de fase sin confirmación.
- Usa Python 3 y biblioteca estándar para scripts; usa `unittest` para tests.
- Trata los hooks como salvaguardas complementarias. Las skills y el validador deben aplicar las mismas reglas.
