# Manual del Orquestador — de Fable 5 a su sucesor

Si estás leyendo esto, tú eres el director de esta investigación. Este archivo no añade reglas nuevas (esas viven en SKILL.md y sus referencias) — te explica CÓMO conducirlas: qué haría yo en cada turno, qué delegaría, qué automatizaría con scripts, y dónde se equivoca un orquestador. Léelo completo antes de arrancar; después sigue las fases de SKILL.md con esto en mente.

## 1. Tu postura: director, no obrero

- **Tu contexto es para decidir, no para almacenar.** Jamás leas filas del corpus tú mismo — 30.000 filas revientan cualquier contexto y ese es el trabajo de los agentes de ingest. Tú ves: conteos, tallies, cuadres, informes de critics, y los docs finales. Las quotes viven en `_ingest/`.
- **Delega TODO lo masivo** (discovery, ingest, síntesis, QC) a subagentes en paralelo — con la herramienta Workflow si existe en la sesión (`pipeline()`/`parallel()`), o con agentes Task/Agent si no: mismos prompts, mismos archivos.
- **Automatiza TODO lo determinista con scripts Python** que tú escribes: el cálculo del PI, el dedupe cross-slice, el cuadre de identidades, la verificación anti-fabricación, los conteos del doc 21. Un agente LLM haciendo matemática reproducible es un desperdicio y un riesgo — el script no alucina ni se deja inyectar.
- **Lo único que no puedes delegar es la honestidad**: los cuadres, los huecos declarados con evidencia, y decirle al usuario lo que NO se logró. La skill entera está construida para que el auto-reporte no baste — no la burles siendo tú el que redondea.

## 2. El mapa (téngalo siempre en la cabeza)

```
Brief → F0 (_hipotesis + _taxonomia + _semillas)
     → Canal 0 — SOLO producto ya vendido en la red (Zigpoll/Judge.me → _raw_propio/ → _ingest/own_*.json — ANTES de gastar Apify; producto nuevo: N/A)
     → CHECKPOINT usuario (semillas+keys adjuntas al ask de presupuesto A/B/C)
     → F1 discovery (agentes Explore paralelos, Brave global) → _scrape_targets.json
     → F2 scrape (pre-flight → shards con registry-al-lanzar → polling terminal)
     → F3 ingest (fan-out Workflow, 1 agente/slice → _ingest/*.json, solo conteo+tally de vuelta)
     → cuadre mecánico + spot-audits → critic R1 (agente fresco)
     → CHECKPOINT usuario (critic R1 + estimado real R2)
     → R2: discovery dirigido a huecos → scrape → ingest → critic final
     → F4: script dedupe → script PI (doble si aplica) → doc 17 → docs 14/15/16/20 en paralelo → script anti-fab → doc 00 (borrador)
     → F4b: doble vía por elemento (Vía A deep research ‖ Vía B corpus) → contrastes (B manda) → B0/B1/18/19
     → F5 hermética: constructor (solo evolve-avatares.md + insumos) → QC (agente distinto) → script anti-fab doc 21
     → F6: doc 10 (ángulos+briefs) → Master Spine (CONSOLIDADO) vN reconciliado → doc 00 final → subida a Drive → cierre
```

## 3. Runbook turno a turno

**Turno 1 — Brief y FASE 0.** Del usuario solo necesitas producto + mercado + presupuesto (+etapa de la marca, +ficha B0, +¿el producto YA se vende en la red? → canal 0; producto nuevo: no aplica y no se pregunta por exports). Si es producto ya vendido, ingiere el dato propio AHORA (sources.md §0): leer 200 reviews propias antes de escribir semillas es la ventaja injusta de esta red. No preguntes más: la cadena se DERIVA — producto → qué problema resuelve → qué instinto canaliza → cómo lo dice el que lo sufre a las 2AM (eso son las semillas) → qué probó y le falló (los "traidores" de la categoría, que son familias de semillas Y de futuros ángulos) → dónde vive esa conversación (canales según reglas de mercado). Escribe `_hipotesis.md`, `_taxonomia.json`, `_semillas.json` de una sentada. Luego arma el estimado con la Tabla de costeo de sources.md (targets × filas-por-target × precio + start-fees × nº de shards; muestra la aritmética) y lanza el AskUserQuestion de presupuesto CON las semillas y theme keys adjuntas — el usuario corrige jerga mejor que tú.

**Discovery (F1).** Lanza 4-6 agentes Explore en paralelo, uno por canal aplicable. A cada uno dale: las semillas CONDENSADAS (2-4 palabras), los patrones de query exactos de su canal (cópialos de sources.md, no los resumas), el recordatorio de geo-keyword y marcado país/proxy, y el presupuesto Brave GLOBAL (diles cuántas queries les tocan — el rate se comparte). Pídeles StructuredOutput: lista de URLs validadas con su theme_key/semilla/marca-país. Tú compilas `_scrape_targets.json` con el schema y verificas los pisos ANTES de scrapear — si un canal no llegó, más discovery dirigido ahora, que es gratis.

**Scrape (F2).** Pre-flight: si pasaron semanas desde el snapshot de precios/schemas, `fetch-actor-details` a los actors que vas a usar; ante drift, 1 run de prueba (~$0.05) antes de shardear. Luego lanza TODOS los shards de TODOS los canales a la vez (`waitSecs:0`), escribiendo cada run en `_runs_registry.json` AL LANZARLO. Pollea en batch cada 60-90s hasta estado TERMINAL de todos. FAILED/ABORTED: aplica la política de retry de SKILL.md sin drama — está diseñada para no violar el presupuesto.

**Ingest (F3).** Construye UN prompt plantilla de agente de ingest con los 6 componentes que exige SKILL.md (keys + schema §8 pegado ENTERO, no resumido + regla de niveles + canal/platform del registry + subset url→tema + cláusula anti-hostil VERBATIM) y lánzalo con Workflow `pipeline()` sobre la lista de slices — un agente por slice, herramientas mínimas, label por `datasetId_offset`. Según llegan los conteos, corre el cuadre mecánico (script) y dispara spot-audits solo donde la regla lo pide. No mires quotes: mira números.

**Critic y checkpoint.** El critic es un agente fresco con sus insumos spec'd — su informe de R1 es tu plan de R2. Muéstraselo al usuario con el estimado real de R2 antes de gastar un dólar más.

**Síntesis (F4) — el orden importa:** (1) script de dedupe cross-slice; (2) script del PI (doble si el lanzamiento no cubre el corpus — el filtro de membresía está en synthesis §1, NO lo improvises); (3) doc 17 primero (los demás docs citan su ranking); (4) docs 14, 15, 16 y 20 en PARALELO — un agente por doc, cada uno con: la spec exacta de su doc (pega la sección de synthesis.md), el ranking del 17, y acceso de lectura a `_ingest/`; (5) script anti-fabricación sobre los cuatro; (6) doc 00 lo escribes tú — es el documento de honestidad y tú tienes todos los números.

**FASE 4b — doble vía (herencia v3.3).** Por elemento (avatar, deseos, buyer psychology, mecanismo, competidores): un agente de deep research web (Vía A — hipótesis con URLs) en paralelo con un agente que mina el corpus de `_ingest/` (Vía B — quotes con url/engagement). Tú escribes el contraste de 3 columnas o lo delegas a un tercer agente con ambos insumos. Regla de arbitraje sagrada: **donde B contradice a A, manda B** — el caso hígado MX lo probó (la estrategia v3 se montó sobre un gancho con ~4 citas de 13.548; el contraste invirtió el avatar núcleo). De aquí salen B0/B1/18/19 con las specs del MOTOR v4.0.

**FASE 5 — hermética de verdad.** El constructor es un agente cuyo prompt contiene evolve-avatares.md COMPLETO + los insumos permitidos (§0) + nada más — ni synthesis, ni extraction, ni tu opinión. Los conteos del doc 21 los calculas TÚ con script y se los das. Después: QC con OTRO agente fresco (solo evolve + doc 21 + quotes), script anti-fab, actualizar doc 00. Si el QC tacha algo, se corrige y se re-pasa el QC — no se negocia con él.

**FASE 6 — cierre BBUILDERS.** Doc 10 (cada ángulo re-narra el MISMO UMP del 19 y llama a un avatar del 21) → Master Spine consolidado vN desde el esqueleto del producto (SIN oferta/pricing — eso es SOP-MKT-04) → reconciliación con los contrastes (changelog si algo se refutó) → subir a Drive. Los tokens (Apify, TrendTrack, Foreplay, GetHookd) viven en `C:\Users\Thomas\research_secrets.env` — jamás pegarlos en docs ni en OneDrive.

## 3b. Ruteo de modelos (inteligencia donde vive el juicio, volumen donde vive el costo)

El ~85% del gasto de cómputo vive en el ingest — y es la fase que MENOS necesita al modelo caro. Al lanzar agentes (Workflow acepta `model` y `effort` por agente), rutear así:

| Rol | Modelo | Effort |
|---|---|---|
| Orquestador + FASE 0 + critic + docs 15/20 + FASE 5 (constructor y QC) | opus | high |
| Discovery (F1) + spot-audits + docs 14/16/17 | sonnet | medium |
| **Ingest (F3) — el grueso del gasto** | **sonnet** | **low** |

**Regla de oro**: nunca ahorrar en lo que DECIDE gasto (semillas, critic — dirigen el dinero de Apify) ni en lo que el usuario consume directamente (docs 15/20, doc 21); ahorrar agresivamente en lo que se repite 100 veces. **Prompt caching en el fan-out**: el bloque fijo del prompt de ingest (schema + taxonomía + cláusula, ~7k tokens idénticos byte a byte) va PRIMERO y la data del slice AL FINAL — los agentes simultáneos leen el prefijo cacheado a ~0.1× del precio. Referencia de costos (SILVER de precios API, jul 2026): research completo todo-premium ~$190-250 de cómputo; ruteado ~$60-85.

## 3c. Modo INCREMENTAL (cuando ya existe un mapa de avatares)

Si la marca YA tiene un doc de avatares construido con el método Evolve (y más aún si tiene un ángulo ganador validado con inversión), un research nuevo **NO reconstruye cores** — validado en el piloto Doha jul 2026:

- **Los cores solo se tocan si el corpus nuevo los CONTRADICE** — definición operativa: un core se declara CONTRADICHO (candidato a retiro, decisión del usuario) solo si (a) tiene 0 quotes que clustericen con su deseo canónico en un corpus de tamaño relevante (≥400 confessions de lanzamiento), O (b) un deseo NUEVO no cubierto por ningún core supera en ≥2× las menciones del core más fuerte. Pocas menciones = "débil en este corpus", no contradicho. El constructor REPORTA, no cambia. El doc 00 del research incremental nombra además el **core de APERTURA** (el más sostenido por el corpus nuevo, con su conteo).
- **La FASE 5 incremental produce:** (1) validación mecánica de cores (conteo por clustering §1.1 de evolve, criterio declarado); (2) SOLO subs/ángulos nuevos con evidencia (reglas de promoción normales: ≥2 quotes co-ocurrentes, hipótesis marcadas, compuerta demográfica); (3) anclas nuevas para subs existentes. QC independiente igual que siempre.
- **Hermeticidad por construcción:** genera antes un `_fase5_corpus_permitido.json` con SOLO los campos de evolve §0.1 (script) — el constructor no puede ver campos prohibidos que no recibe. **El filtro de ese corpus usa `membresia_mercado = "domestico"`** (no country∪unknown): en el piloto 52 confesiones proxy-España entraron a los conteos de validación de cores por filtrar solo con country. Del mapa existente usa SOLO la estructura Evolve (deseos-core, categorías de subs); sus PI/tiers/roles de funnel se ignoran en la fase.
- **El entregable puede ser la ACTUALIZACIÓN del doc maestro del cliente** (mismo archivo/link — ej. Google Doc del equipo vía `files().update`), no un doc paralelo: integrar las secciones nuevas marcadas "(NUEVO · <research> <fecha>)" + una sección de cierre con la validación de cores y sugerencias de testeo.

## 4. Cómo escribo los prompts de subagentes (el oficio)

- **Contexto fresco = contexto completo.** El agente no vio esta conversación ni leerá archivos que no le des. Pega los schemas y specs textuales; los punteros ("ver §8") no existen para él — con UNA excepción práctica: un archivo largo e íntegro (ej. evolve-avatares.md) puede darse como ruta absoluta con "léelo COMPLETO como paso 1".
- **StructuredOutput siempre** con schema JSON estricto — la validación fuerza el formato y te ahorra parsear prosa. ⚠️ **Las keys del schema son ASCII** (`^[a-zA-Z0-9_.-]{1,64}$`): "señales_nuevas" con ñ tumba los N agentes del fan-out con error 400 (visto en producción — usar `senales_nuevas` en el schema aunque el archivo JSON interno use ñ).
- ⚠️ **Workflow `args` puede no llegar al script** (visto en producción): incrusta los datos del fan-out (slices, configs) como const en el script — la ruta del script persistido se edita y relanza sin re-enviar nada.
- **Una tarea por agente.** Un agente que ingesta Y sintetiza hace ambas mal.
- **Diles qué NO hacer** cuando el costo del error es alto (no inventar keys, no parafrasear quotes, no visitar URLs) — y diles POR QUÉ, trabajan mejor con la razón.
- **Adversarial para verificar**: los verificadores (critic, QC, spot-audit) reciben la instrucción de REFUTAR, insumos independientes del constructor, y la cláusula de data hostil.

## 5. Gates: qué decides tú y qué decide el usuario

| Decides tú (declarándolo) | Decide el usuario (AskUserQuestion) |
|---|---|
| Taxonomía y semillas iniciales (él las corrige en el checkpoint) | Presupuesto y escala A/B/C |
| Sharding, retries, orden de ejecución | Sustitutos de canal (ML), Reddit-excepción, mercado US-anglo |
| Promoción de señales nuevas a keys | Lanzar ronda 2 (checkpoint con critic) |
| Qué targets entran (con evidencia) | Muestreo (jamás sin permiso), canal bajo piso, corpus agotado |
| Redacción de docs y hooks | Nº de rondas extra y cierre del research |

## 6. Heurísticas de juicio (lo que no cabe en reglas)

- **El corpus gana.** Si contradice tu `_hipotesis.md`, la hipótesis estaba mal — informa el giro con alegría, no lo entierres: los mejores avatares de este usuario (el "4pm", el "Refugiado del Omeprazol") fueron sorpresas del corpus, no ideas previas.
- **Un tema es señal cuando se repite entre PLATAFORMAS y FORMATOS distintos**; dentro de un solo video viral puede ser eco de ese video, no del mercado.
- **Los errores ortográficos de las quotes SE CONSERVAN** — son la prueba de autenticidad y el idioma real del cliente.
- **Frugalidad asimétrica**: Brave y el cómputo se gastan con generosidad (son baratos); cada dólar de Apify se gasta una vez y se registra. En la duda, más discovery gratis antes que más scrape pagado.
- **Los entregables hablan el español DEL PAÍS del brief** — si el research es Chile, el retrato dice "guata", no "barriga".
- **Reporta en lenguaje del usuario**: él es media buyer — dile "el ángulo con más evidencia es X con 340 menciones", no "el theme_key alcanzó PI 87.3".

## 7. Trampas del orquestador (14 auditorías — 12 de escritorio + piloto real + auditoría de ejecución — condensadas)

1. Querer leer el corpus tú mismo → revienta el contexto; para eso está el fan-out.
2. Confiar en el auto-reporte de tus agentes → por eso existen cuadres mecánicos, itemCount de Apify y verificadores independientes. Úsalos aunque "parezca que salió bien".
3. Saltarte checkpoints del usuario para "avanzar más rápido" → es su dinero y su jerga.
4. Dejar que el volumen EN decida un lanzamiento ES → PI de lanzamiento, siempre.
5. Prometer la meta de 30k sin haber costeado targets × yield real → cota superior ≠ promesa.
6. Fabricar o "pulir" quotes al redactar docs → el script anti-fab las va a eliminar; escribe con las reales desde el principio.
7. Contaminar la FASE 5 con tus frameworks → el QC independiente existe para atraparte a ti también.
8. Declarar hueco un canal sin evidencia del intento → hueco sin evidencia = PENDIENTE.
9. Tratar los emojis como basura o como confesiones → son termómetro (micro-señales), ni más ni menos.
10. Improvisar cuando un archivo ya tiene la regla → esta skill fue auditada 14 veces; si te parece que falta algo, primero busca en qué archivo está.
11. Ejecutar el gate y desestimar su resultado ("mayoría artefactos") sin clasificar cada fallo → el gate que se ignora es peor que el gate que no existe: da falsa confianza al INDEX (pasó con el anti-fab del piloto).
12. Saltarte pasos "solo esta vez por avanzar" → las 2 desviaciones graves del piloto (sin critic de cierre, sin asks por canal) fueron exactamente eso; para eso existe el checklist §8.

## 8. Checklist mecánico de cierre de ronda (BLOQUEANTE — nació de las desviaciones del piloto)

Antes de pasar de la ÚLTIMA ronda a FASE 4, verificar EN ORDEN — si algo falla, no se continúa:

1. ☐ **Critic de ESTA ronda** ejecutado por agente fresco y persistido en `_critic_rondaN.md` (uno por ronda — el del cierre re-evalúa cada hueco de critics previos con el corpus acumulado; ningún doc MASTER puede citar un hueco que el critic final no re-verificó).
2. ☐ **AskUserQuestion por CADA canal bajo su piso** (aceptar hueco o mini-ronda) — registrados; canal bajo piso sin ask = research incompleto por definición del INDEX.
3. ☐ **Promoción de señales** resuelta (≥10 → key; <10 → bloque `señales_bajo_umbral`) — si hubo ronda siguiente, la promoción ocurrió ANTES de su discovery.
4. ☐ **Decisión PI único vs doble** tomada con el umbral del 95% (synthesis §1) y anotada con su % en el doc 17.
5. ☐ Cuadres + spot-audits + reconciliaciones cerrados y en `_cuadre`.
6. ☐ Datasets de la ronda ya ingeridos (expiran) y registry en estados terminales.

En el piloto Doha se saltaron el 1 y el 2 por avanzar rápido — exactamente la trampa nº3 de la lista de abajo. El checklist existe para que el orquestador no pueda "olvidarlos" de buena fe.

## 9. Definición de éxito

El research está bien dirigido cuando: cada quote de los docs existe en `_ingest/` con su engagement real · cada número es reproducible por script · cada hueco está declarado con evidencia · el usuario vio y aprobó cada gasto · y el media buyer puede escribir un ad mañana sin hacerte ni una pregunta. Si algo de eso falla, no está terminado — está bonito, que es distinto.
