# CÓMO sintetizar — fórmulas, docs MASTER, blueprint psicológico y avatares

## Índice
1. [Priority Index](#1-pi)
2. [Sophistication → estrategia Big 3](#2-sophistication)
3. [Los docs MASTER](#3-docs)
4. [Doc 20 — Buyer Psychology Blueprint](#4-doc20)
5. [Identidades Reclamadas](#5-identidades)
6. [Hooks — patrones y etiquetado](#6-hooks)
7. [Avatares y Ángulos (FASE 5) — remite a evolve-avatares.md](#7-avatares)

---

## 1. Priority Index (ranking de temas, doc 17) {#1-pi}

Para cada theme key, sumar a través de TODAS las plataformas:

- **Frecuencia** = nº total de menciones.
- **Resonancia** = engagement sumado, ponderado por plataforma para corregir escalas de "like". **Engagement negativo (downvotes de Reddit): se recorta a 0 para la resonancia** (`max(engagement, 0)` — un downvote no resta tema; la quote conserva su valor negativo como señal y es citable con él). Peso por valor del enum `platform` del schema (cada valor tiene exactamente un peso): `reddit` ×1.0 (upvotes) · `youtube` ×1.0 · `tiktok` ×1.6 · `instagram` ×2.2 · `facebook` ×3.0 · `fb_group` ×3.0 · `tiktok_shop` ×2.0 · `forum` ×2.5 (poco engagement explícito, mucha densidad) · `amazon` ×1.0 (sobre votos "helpful") · `zigpoll` ×2.5 y `judgeme` ×2.5 (canal 0 — dato propio de compradores reales, densidad máxima; su engagement son estrellas/0, no likes). Fuente opcional autorizada: hereda el peso del canal que sustituye (ej. `mercadolibre` sustituyendo TikTok Shop → ×2.0); si no sustituye a ninguno, ×1.0 — declarado en el INDEX.

**Normalización EXACTA** (determinista — dos agentes deben producir el mismo PI): `norm(x) = x / max(x) × 100`, donde `max(x)` se toma sobre TODAS las theme keys del corpus completo (todas las rondas juntas; conjunto = keys de `_taxonomia.json` + señales_nuevas promovidas a key). Nunca min-max. Si `max = 0`, el PI del tema es 0. El doc 17 registra el método y los denominadores usados. Combinar: **PI = 0.55 × frecuencia_norm + 0.45 × resonancia_norm**

Las **micro-señales** (extraction.md §8) suman a la RESONANCIA con fórmula fija: `resonancia_tema += conteo_micro_señales × 0.25 × peso_de_plataforma` (cada micro-señal vale 1 unidad de engagement; el engagement propio del comentario micro se ignora). No suman a frecuencia.

**PI doble cuando el mercado de lanzamiento no cubre todo el corpus** (ej. lanzamiento full-ES con rama EN de inteligencia — doctrina Cúrcuma/Alivra): emitir DOS rankings — PI global (inteligencia) y PI del corpus de lanzamiento. **Filtro canónico de membresía al corpus de lanzamiento**: entra toda confession con `lang` = idioma de lanzamiento Y `membresia_mercado = "domestico"` (campo heredado del target, extraction §8) — **el `country=unknown` NO basta**: una fila de foro proxy-España también es unknown, y sin el campo de membresía 52 confesiones proxy contaminaron el PI "de Chile" del piloto. Las filas `proxy` e `inteligencia` van SOLO al ranking global y su % se reporta SIEMPRE en el doc 00 (aunque se emita PI único). Micro-señales y ecos heredan la membresía de su target en `_scrape_targets.json`. **Umbral de exención (de-minimis)**: si el corpus de lanzamiento cubre ≥95% de las confessions globales, se emite UN solo ranking (el de lanzamiento) y el doc 17 anota "PI único: lanzamiento = N% del global; excluidas X proxy + Y inteligencia" — por debajo de 95%, PI doble obligatorio. **Cada ranking se normaliza con max(x) sobre SU PROPIO corpus** (global→todo; lanzamiento→solo las filas del filtro), y el doc 17 registra el filtro exacto y ambos pares de denominadores. El cold-open, los veredictos de uso y la recomendación Big-3 se deciden sobre el ranking de LANZAMIENTO — un solo ranking global dejaría que quotes EN dicten el ad ES (el error que Alivra corrigió a mano).

**Diagnósticos por tema (obligatorios en la tabla del doc 17 — el PI rankea CONVERSACIÓN, no dolor; estos dos números evitan leerlo mal):**
- **% resonancia positiva** del tema (`(pos + 0.5·mixta) / total`, sobre resonancia ponderada). Si ≥50%, el tema NO puede presentarse como "dolor" — es conversación/prueba-social/deseo cumplido (en el piloto, el "dolor #1" era 54% positivo y manchas 91% positivo: su ancla top era una quote de ÉXITO). El doc 20 lo reclasifica.
- **% fila única**: participación de la fila de mayor resonancia en el total del tema. Si ≥40%, marcar "resonancia de fila única — eco de UN contenido, no del mercado" y degradar el veredicto de cold-open a escalación/VSL (en el piloto: una pregunta gremial viral era el 53% de su tema; una quote positiva, el 79% del suyo).

**Métrica de concentración (obligatoria en doc 17 y doc 00)**: % de las confessions de lanzamiento que aporta el canal #1 y % que aportan las 4 URLs top. Regla de lectura: si un canal aporta >60% O las top-4 URLs >40%, el critic lo marca y el doc 17 advierte que el PI refleja la conversación de ESE canal/creadores — los veredictos de cold-open se leen con ese sesgo declarado (piloto: TikTok 72-77%).

Reglas de lectura (validadas en proyectos previos):
- Resonancia altísima + pocas menciones = **mecanismo/escalación de VSL**, no hook de masa.
- Tema que la estrategia ama pero el corpus ignora (cae del top al escalar la muestra) = vive dentro del VSL, JAMÁS lidera un ad.
- El tema #1 por PI define el cold-open de menor fricción. Empate de PI → gana el de mayor resonancia.
- Veredicto de uso por reglas: mecanismo/escalación de VSL si reso_norm ≥70 y freq_norm ≤40 · retargeting si la key es de familia objeción · descartar si PI <15 en ambos rankings · el resto: cold hook por orden de PI de lanzamiento.
- Añadir al doc 17 el **balance de valencia por tema** (neg/pos/mixta del tally; las neutras quedan fuera). Dominancia determinista: `mixta` se reparte 0.5/0.5 entre neg y pos; dominante = el lado mayor sobre el total codificado NO-neutro del tema (neg+pos+mixta); empate → decide la intensidad dominante (aguda/crónica→dolor; aspiracional→deseo). La recomendación de apertura se emite en §4.3.

## 2. Sophistication → estrategia Big 3 (cierre del doc 17) {#2-sophistication}

Determinar la etapa (1-5) CON EVIDENCIA del corpus (no intuición): volumen de "probé X y no funcionó" (etapa 3+), burla/escepticismo ante claims ("sí claro, otro milagrito" = etapa 4-5), vocabulario de mecanismos ya conocido por el público.

En ecommerce casi todo mercado está en 3-5. La respuesta estratégica se elige entre las **Big 3**:
- **Nuevo Mecanismo** — nueva forma de entregar el mismo resultado ("no es otro X, es el primero que…"). Requiere poder demostrarlo. El corpus dice qué mecanismos ya quemó el mercado.
- **Nueva Información** — revelar el "porqué" oculto del problema / por qué fallaron los mecanismos anteriores. El corpus dice qué acaba de aprender el mercado y qué aún no sabe.
- **Nueva Identidad** — quién se vuelve el cliente al usarlo; tribu, cultura, anti-establishment. El corpus da los marcadores de identidad y el villano común.

Registrar el **gap avatar-sophistication**: el avatar puede estar perfecto y los ads morir igual si el mensaje no rompe el nivel de escepticismo. La recomendación Big-3 va escrita en el doc 17 con 3-5 quotes que la justifican.

**Conciencia — eje independiente (herencia motor v3, cierra el doc 17 junto a la sofisticación):** agregar el **histograma de conciencia** desde el campo `conciencia` de las confessions de LANZAMIENTO (% por etapa Schwartz, ponderado por resonancia; N etiquetado junto al histograma — si <30% de filas traen el campo, marcar "⚠️ baja cobertura" igual que buying_emotion). Dominante = mayor %. **Regla anti-sobreestimación**: si dos niveles quedan a ≤10 puntos, se elige el MENOR (educar de más es más barato que asumir de más). La **matriz Conciencia × Sofisticación** emite la entrada del mensaje: la conciencia fija DÓNDE arranca (qué sabe ya el prospecto); la sofisticación fija QUÉ claim es creíble — **palanca**: sof 1-2 → promesa directa (+ Nueva Información si conciencia baja) · sof 3-4 → Nuevo Mecanismo (el UMP/UMS del doc 19 es su ejecución) · sof 5 → Nueva Identidad + mecanismo de soporte. Los batches de creativos heredan el nivel elegido en su nombre (nomenclatura BBUILDERS: UNAWARE/PROBLEM/SOLUTION/PRODUCT/MOST). El mercado hispano suele ir 1-2 niveles de sofisticación por debajo del inglés → arbitraje: mecanismo quemado en EN puede estar fresco en ES (verificarlo en el corpus ES, no asumirlo). **Elevación** (del framework de deseos masivos): al posicionar, mover el producto HACIA ARRIBA del ranking de instintos (Comfort→Health: "los veterinarios ruegan que cambies") multiplica escala; hacia abajo lo comoditiza — y debe ser creíble.

## 3. Los docs MASTER {#3-docs}

(Este archivo cubre los docs del corpus: 14/15/16/17/20 y el 00. Los docs B0/B1/18/19 —doble vía— se construyen en FASE 4b y el 10 en FASE 6, con specs en SKILL.md y el MOTOR v4.0. Mismo formato canónico de cita y mismo script anti-fabricación para todos.)

### Formato canónico de cita (TODOS los docs — lo que el script anti-fabricación parsea)
Toda cita de VoC en los docs MASTER usa UN solo formato-bloque: `*"<quote verbatim>"* — <plataforma>, eng <N> — \`<URL>\`` (o sus campos en tabla, siempre juntos). La URL es obligatoria y jamás placeholder ("misma URL" solo si la línea anterior del MISMO bloque la trae). Las frases del autor (síntesis, etiquetas, hooks) NUNCA van entre comillas dobles estilo cita — usan cursiva sin comillas o la etiqueta explícita `[CONSTRUIDO]`/`(síntesis)`. Esto es lo que permite verificación automática sin falsos positivos.

### 14 — Banco de Confesiones
15-20 **anclas** — selección canónica: top-N por engagement ponderado DENTRO del corpus de lanzamiento, cubriendo los 5 temas top-PI, con ≥1 ancla con capa de eco; separadas EN vs ES igual que los docs 15-16. Formato: quote verbatim + plataforma + engagement exacto + URL + idioma/país (URL real siempre — un placeholder es fallo de anti-fabricación). Debajo de cada ancla, su **capa de eco**: las confessions con señal `eco` Y las filas mínimas del array `ecos` (extraction.md §8), emparejadas por `reply_to` → `comment_id` del ancla; si las anclas no traen `comment_id` o el canal no dio linkage, emparejar por misma URL del post declarándolo aproximado. Cerrar con clusters temáticos y tabla ancla→tema→uso sugerido (hook / lead / testimonio / prueba social).

### 15 — Retrato del Cliente
Estructura = dimensiones de extraction.md §6, TODAS con quotes de respaldo:
qué PIENSA / cómo HABLA / cómo VIVE (rutinas, momentos-trigger del día) / qué TEME (miedos 3AM + frases hirientes de su entorno) / qué YA PROBÓ (soluciones fallidas + soundbites) / qué NO está dispuesto a hacer / la TRANSFORMACIÓN que imagina (genio mágico + soundbites post-transformación) / de qué CUELGA su éxito / qué tendría que RENUNCIAR / a quién CULPA.
Separado EN vs ES con notas por país. Incluir "personalidad por plataforma" (dónde confiesa, dónde pregunta, dónde sueña). Si comprador ≠ usuario: retrato doble.

### 16 — Banco de Lenguaje y Hooks
- Frases de alto voltaje por categoría (dolor, solución fallida, entorno/relaciones, identidad, sistema/culpables, transformación) EN/ES por país — cada ítem con el mismo formato del doc 14: quote verbatim + plataforma + engagement + país + **URL (obligatoria — sin URL el ítem no es anclable por el script anti-fabricación y se considera fallo)**.
- Metáforas del corpus (sección propia — es el material más valioso), también con quote + plataforma + engagement.
- Jerga por país + vocabulario novato vs experto.
- **Palabras que resuenan** (top 10-15 por frecuencia en quotes de alta valencia) y **palabras a EVITAR** (las que activan escepticismo o minimizan el dolor), ambas con evidencia.
- 20-25 hooks etiquetados según §6 (máx ~25% con etiqueta "construido").

### 17 — Ranking de Temas y Sophistication
Tabla: tema · menciones · engagement ponderado · PI · balance de valencia · veredicto de uso (cold hook / escalación / mecanismo VSL / retargeting / descartar) + top anchors por tema. Cierra con §2 (etapa + estrategia Big 3 + evidencia).

### 00 — INDEX y Spine Estratégico
Mapa de archivos (la lista de docs MASTER vive en SKILL.md FASE 4) + spine (deseo masivo elegido y su test 3D, estrategia Big 3; el **Core Avatar elegido se añade al cerrar FASE 5**, no antes — en FASE 4 aún no existen avatares) + **nota de acceso honesta** (esta es la spec canónica): filas por canal, costo, runIds, cuadre leídas-vs-scrapeadas + spot-audits, tabla piso-vs-logrado por canal y ronda (canal bajo piso sin AskUserQuestion registrada = research incompleto), resultado de la verificación anti-fabricación, % de corpus proxy, y lo NO minado con razón y evidencia del intento.

## 4. Doc 20 — Mapa de Dolores y Buyer Psychology Blueprint {#4-doc20}

Se construye con los campos psicológicos del schema agregados. **Con PI doble, TODO el doc 20 (y las Identidades de §5) se computa sobre el corpus de LANZAMIENTO** (mismo filtro declarado del doc 17); el corpus de inteligencia solo aparece en subsecciones marcadas "inteligencia" y nunca decide emociones primarias, apertura ni umbrales:

1. **Mapa de dolores**: rankeado por **PI-dolor** = PI recomputado SOLO sobre confessions con valencia neg + 0.5·mixta (mismo método, corpus filtrado por valencia; el PI total se muestra al lado como referencia). Un tema con % resonancia positiva ≥50% (doc 17) NO entra al mapa de dolores — va a una sección aparte "conversación/prueba social". Cada dolor con su DOBLE vista: valencia por CONTEO (n neg/pos/mixta) y por RESONANCIA (%) — pueden contradecirse (piloto: desconfianza era neg por conteo y 54% pos por resonancia; ambas verdades van juntas). + intensidad + quotes. Momentos-trigger del día rankeados.
2. **6 emociones de compra puntuadas 0-10** (Relief / Nurturance / Belonging / Identity / Security / Status). Fórmula: `score = round(proporción sobre el total de quotes con buying_emotion no-null × 10)`; emoción con <3 quotes = "sin evidencia suficiente" (no score); registrar el N total etiquetado junto a los scores. **Umbral de cobertura**: si <30% de las confesiones de lanzamiento tienen `buying_emotion` no-null, TODOS los scores se marcan "⚠️ baja cobertura (n=X de Y)" y NO deciden por sí solos la emoción primaria del copy — la decisión se apoya además en valencia+señales y se declara el criterio. Por emoción: por qué ese score, cómo activarla, 1 línea de ejemplo sobre quotes. Marcar PRIMARIA (abre el copy), SECUNDARIA (build-up) y la que se EVITA.
3. **Dolor vs Deseo**: equivalencia canónica — valencia neg = huye DE (dolor); valencia pos = corre HACIA (deseo; las positivas de éxito post-compra cuentan como deseo cumplido → lado deseo); mixtas 0.5/0.5; neutras fuera. Scores = `round(proporción × 10)` + modo dominante + recomendación de apertura (dolor-dominante → abrir por dolor y pivotar a alivio; deseo-dominante → abrir por visión).
4. **Intensidad dominante** (aguda/crónica/aspiracional) → tono del copy (urgente-empático / reactivar urgencia / visionario). Si es crónica: qué la reactiva (dolor futuro, lo que se está perdiendo).
5. **Objeciones**: 10-15 rankeadas por nº de quotes con señal `objecion` (desempate: engagement ponderado), cada una con su emoción de hesitación (miedo/culpa/vergüenza/escepticismo), quote representativa y disolvente sugerido. Top 2 hesitaciones a atacar + la ignorable.
6. **Viaje emocional** (Interrumpir → Enganchar → Agitar → Creer → Decidir): qué siente el prospecto en cada etapa y qué emoción activar, con línea de ejemplo por etapa anclada en quotes.
7. **Identidades Reclamadas** — sección obligatoria de este doc (estructura en §5).
8. **Blueprint final de una frase** + fórmula de copy: ABRE CON / CONSTRUYE CON / MANEJA [hesitación] MEDIANTE / CIERRA CON — cada pieza con su porqué.

## 5. Identidades Reclamadas (SIEMPRE como sección del doc 20 — no crear doc propio) {#5-identidades}

De las quotes `identity_lost`: las **5-10 identidades reclamadas** más potentes. Por identidad:
- **Quién era** (identidad original, tangible) · **Qué perdió** (momento/proceso de traición, con LA quote) · **Qué quiere reclamar** (volver a ser, no ser otro).
- 3+ hooks derivados (nostalgia + pérdida + rabia, para cold traffic / primeros 5s).
- Ranking por dolor × universalidad: dolor = proporción de valencia neg en las quotes de la identidad; universalidad = nº de quotes independientes que la respaldan.
Tabla resumen + 3-5 combos (identidades fusionadas). Regla: si no hay ≥3 quotes reales que respalden una identidad, no entra.

## 6. Hooks — patrones y etiquetado {#6-hooks}

Cada hook del doc 16 se etiqueta: **patrón + tema ancla (PI) + emoción que activa + idioma/país + quote de origen** (verbatim + engagement + plataforma; si un hook no deriva de ninguna quote identificable, marcarlo explícitamente como "construido" — **máximo ~25% de los hooks pueden ser construidos**, y aun esos citan su tema ancla y las palabras del corpus que usan). Patrones:

callout directo ("A la que ya probó todo…") · identidad/identidad-reclamada · mecanismo-curiosidad · us-vs-them (el culpable del corpus) · tamaño del claim · velocidad del claim · autoridad ("veterinarios ruegan…") · antes/después · comparación con rival · quitar limitación ("aunque ya hayas probado X") · pregunta · nueva información ("por qué X en realidad…") · novedad · exclusividad · romper creencia ("pensé que era imposible hasta…") · momento-trigger ("¿también son las 4pm y ya no das más?").

Regla: el hook usa las PALABRAS del corpus (novato, no experto) y respeta compliance. Los mejores hooks suelen ser una quote real casi intacta.

## 7. Avatares y Ángulos (FASE 5) — NO usar este archivo {#7-avatares}

⛔ La construcción de Core Avatars, Sub Avatars y Ángulos NO se rige por este archivo. Es una fase HERMÉTICA que sigue única y exclusivamente [evolve-avatares.md](evolve-avatares.md) (método fiel de "The Evolve Avatar Training"). Nada de lo anterior en este archivo (PI, identidades reclamadas, buyer psychology, sophistication, hooks) se aplica a esa fase — esos frameworks alimentan los docs MASTER, no los avatares.
