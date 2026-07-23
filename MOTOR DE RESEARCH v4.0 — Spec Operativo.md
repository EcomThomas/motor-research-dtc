# MOTOR DE RESEARCH v4.0 — Spec Operativo

> **v4.0 = fusión del motor v3.3 con el sistema VoC corpus-first "Master Skill Research"** (skill de referencia externa, piloteada en Chile — original intacto en `SISTEMAS Y SOPS INTERNOS\METODOS\SKILL-VOC-RESEARCH-AMIGO\`).
> Decisión de Thomas (2026-07-17): adoptar TODAS las mejoras del sistema externo, **sacar oferta/pricing del research** (ahora vive en SOP-MKT-04) y aceptar mayor gasto por research (corpus grande > research barato).
>
> **El motor ahora es EJECUTABLE como skill**: `/voc-research` (instalada a nivel usuario en `~/.claude/skills/voc-research/` — disponible en cualquier sesión de Claude Code). Este documento es la AUTORIDAD del mapa de docs y las decisiones de diseño; la skill es el manual de ejecución. Si contradicen algo, manda este doc y se corrige la skill.

---

## 1. Qué cambió de v3.3 → v4.0

| Área | v3.3 | v4.0 |
|---|---|---|
| Adquisición | El intake RECIBÍA dumps de Apify ya scrapeados | El motor DIRIGE la adquisición: discovery (Brave/WebSearch) → scrape Apify shardeado con gates de presupuesto por ronda (A/B/C, autorización explícita en USD) |
| Canales | Los que trajera el dump | **Canal 0 propio (Zigpoll/Judge.me/SAC — gratis; SOLO productos que la red YA vende, producto nuevo: N/A)** + 11 canales oficiales con specs Apify probadas, costeo por fila y yields reales medidos |
| Ingest | Scripts deterministas + clasificador con gold-set | Fan-out de agentes (Workflow), **cobertura 100% sin muestreo**, schema psicológico por fila, cuadre con identidades mecánicas, spot-audits, critic por ronda con contexto fresco |
| Anti-fabricación | REGLA MADRE ([CITA] vs [INFERENCIA]) por disciplina | La regla madre sigue + **script anti-fabricación**: toda quote de todo doc se verifica mecánicamente contra el corpus (texto+URL+engagement); 4 clases de fallo; fase no cierra sin persistir el resultado |
| Membresía de mercado | PI doble por idioma (venta/inteligencia) | PI doble + campo `membresia_mercado` (**domestico / proxy / inteligencia**) heredado del target — corta la contaminación pan-ES (foros de España, creadores pan-LATAM) que el filtro por idioma no ve |
| Micro-señales | Filas <4 palabras se descartaban | Emojis, tags y "yo 🥲" = **micro-señales**: capa de resonancia propia (suman al PI ×0.25, nunca a frecuencia) + capa de ecos |
| Avatares | Doc 15 fusionado (Evolve+DTS+Playbook) | Retrato (15) separado de **Avatares (21): FASE HERMÉTICA** — solo "The Evolve Avatar Training", QC independiente que RE-EJECUTA conteos, avatares por etapa de revenue, **modo incremental** para marcas con mapa existente (SuperCalm) |
| PI | log1p + winsorizing + penalizaciones (0.60/0.40) | **Fórmula de la skill: PI = 0.55·freq_norm + 0.45·reso_norm** (norm = x/max×100), pesos por plataforma para corregir escalas de like, micro-señales, y diagnósticos que reemplazan al winsorizing: % resonancia positiva, % fila única (≥40% = eco de un contenido), concentración por canal/URL. El PI v3 queda retirado |
| Oferta | Spine incluía decisiones de oferta | **FUERA del research.** El Spine v4 no tiene sección de oferta; pricing/bundles/garantía = SOP-MKT-04 (después del Spine) |
| Doble vía | Fase núcleo obligatoria | **SE CONSERVA ÍNTEGRA** (es la joya del v3.3): Vía A deep research + Vía B corpus + contraste; donde B contradice a A, manda B. Ahora la Vía B es el corpus COMPLETO verificable, no una muestra |

**Se conserva también de v3/v3.3 sin cambios:** UMP/UMS/USP con gate de sustanciación · conciencia y sofisticación como ejes independientes + matriz y palanca · gates anti-alucinación (§5) · TAM/demografía como inputs externos · saturación honesta (lanzar con cobertura parcial ETIQUETADA, nunca cerrar en falso — ahora instrumentada con critics + asks por canal bajo piso).

## 2. Mapa canónico de documentos (IDs CONGELADOS — v4)

| ID | Documento | Capa | Se construye en |
|----|-----------|------|-----------------|
| **00** | Master Spine / INDEX estratégico + nota de acceso honesta | Spine | Borrador en F4 · final en F6 (reconciliado, versionado vN, SIN oferta) |
| **B0** | Marca, Producto y Rendimientos (ficha real del producto) | Sustrato | F4b (input del brief) |
| **B1** | Espionaje de Competidores + Mapa de Vacíos (con teardown 1-3★) | Sustrato | F4b |
| **14** | Banco de Confesiones (anclas + capas de eco) | Evidencia | F4 |
| **15** | Retrato del Cliente (dimensiones con verbatims) | Interpretación | F4 |
| **16** | Banco de Lenguaje y Hooks (+ metáforas, jerga por país, palabras a evitar) | Interpretación | F4 |
| **17** | Ranking de Temas (PI) + Sofisticación + **Histograma de Conciencia** + Matriz y Palanca | Interpretación | F4 |
| **18** | Deseos de Masa (único scoring de deseo: test 3D alimentado por PI) | Interpretación | F4b |
| **19** | UMP / UMS / USP (con estatus de sustanciación) | Interpretación | F4b |
| **20** | Mapa de Dolores + Buyer Psychology (+ Identidades Reclamadas §5) | Interpretación | F4 |
| **21** | **Core Avatars, Sub Avatars y Ángulos (método Evolve, HERMÉTICO)** | Interpretación | F5 |
| **10** | Los 5 Ángulos + Briefs de Producción (puente) | Producción | F6 |

⚠️ Notas del mapa: el **21** es NUEVO en v4 (avatares Evolve) — no confundir con los archivos históricos "DOC 21(b) — Banco de Hooks" de la era v2/v3.2 (los hooks canónicos viven en el **16**; lo histórico no se renombra). Los docs "b" de contraste (15b/18b/19b/20b/COMPETITOR_MAP b) siguen existiendo como entregables de la doble vía en F4b. Ningún documento cita un ID sin resolverlo contra esta tabla.

## 3. Pipeline v4 (resumen — la ejecución detallada vive en la skill)

```
F0   Hipótesis + taxonomía + semillas (por país)          [skill FASE 0]
C0   Canal 0 (SOLO producto ya vendido en la red)          [sources.md §0 — producto nuevo: N/A]
GATE Presupuesto A/B/C con estimado USD                    [autorización explícita SIEMPRE]
F1   Discovery de targets (Brave/WebSearch, ~$0)           [skill FASE 1 — pisos por canal]
F2   Scrape masivo Apify (shards, registry, caps)          [skill FASE 2]
F3   Ingest 100% (fan-out, schema, cuadre, critic/ronda)   [skill FASE 3 — 2 rondas default]
F4   Dedupe → PI → docs 14/15/16/17/20 → anti-fab → 00 borrador   [skill FASE 4]
F4b  Doble vía por elemento → contrastes → B0/B1/18/19     [skill FASE 4b — B manda sobre A]
F5   Avatares Evolve (hermética) → QC → anti-fab → 21      [skill FASE 5 · incremental si ya hay mapa]
F6   Doc 10 → Master Spine (CONSOLIDADO) vN → Drive        [skill FASE 6 — SIN oferta]
```

Metas por defecto ("gastaremos más" — decisión 2026-07-17): corpus ≥30.000 filas únicas leídas al 100% (escalada por canales aplicables al mercado), ≥8.000 confesiones codificables, 2 rondas. Referencia de costos: Apify ~$40-150 por research según mercado y escala elegida (costo real ~$0.01-0.03 por confesión útil) + cómputo ruteado ~$60-85 (ingest en modelo barato, juicio en modelo caro — tabla de ruteo en orquestacion.md §3b). El canal 0 es gratis y va primero — pero SOLO existe para productos que la red ya vende (relanzamiento/ángulos nuevos); en producto nuevo se declara `N/A` en el INDEX y el research arranca directo en F1.

## 4. Metodología de los docs propios (F4b/F6 — heredada v3, resumida)

- **Doble vía (v3.3, soberana)**: por elemento → Vía A (deep research web, hipótesis con URLs, tipo INFERENCE) ‖ Vía B (corpus completo, quotes con url/engagement) → contraste `Hipótesis | Realidad VoC | Veredicto (CONFIRMA/REFUTA/MATIZA/AÑADE)`. **Donde B contradice a A, manda B.** QA del contraste: tablas completas, ≥8 quotes reales y ≥8 veredictos, HECHO vs HIPÓTESIS explícito.
- **18 — Deseos**: deseo dominante del corpus (instinto + frecuencia + urgencia en lenguaje real) → test 3D Scope×Urgencia×Permanencia → compara contra `_hipotesis.md` de F0 (hipótesis-vs-veredicto citado en el 00). Elevación por el ranking de instintos solo si es creíble.
- **19 — UMP/UMS/USP (10 pasos)**: precondición 14+16+17+20+B0+B1 saturados → aislar problema visible + deseo dominante → minar soluciones fallidas y autoculpa → 3-5 hipótesis de causa oculta → contrastar vs reseñas 1-3★ del competidor (ahí vive el UMP no resuelto) → nombrar UMP (reencuadra la culpa, explica por qué todo lo demás falló, contra-intuitivo con sentido en <5s) → UMS = atributo REAL del producto que neutraliza el UMP (candado causal, señalable en una línea del listing) → USP = resultado + UMS + UMP + prueba VoC (test "sólo nosotros"). **GATE DE SUSTANCIACIÓN**: `hecho respaldado | hipótesis persuasiva` — el VoC prueba la QUEJA, no la CAUSA; causa sin respaldo externo = hipótesis persuasiva, jamás hecho en pauta.
- **10 — Ángulos + Briefs**: cada ángulo re-narra el MISMO UMP canónico y llama a un Core/Sub del 21 con sus patrones de call-out; brief por batch con nivel de conciencia (nomenclatura: `[PRODUCTO] · BATCH #n · [CONSCIENCIA]`).

## 5. Gates anti-alucinación (v3 §6 — TODOS vigentes en v4)

1. UMP inventado: la relación causal se marca `[INFERENCIA]`, nunca `[CITA]`.
2. Demografía solo con evidencia (`[SIN EVIDENCIA DEMOGRÁFICA]` si no); TAM/tendencias/claims de ads = INPUTS EXTERNOS (`[NO DISPONIBLE - no bloqueante]`).
3. Scores con conteo crudo al lado (N quotes + PI), en bandas — sin precisión falsa.
4. Voz de competidor (`is_competitor`) jamás ancla una promesa del producto propio.
5. Todo número viene de fuente citable o se marca ilustrativo.
6. Identidad reclamada sin data = `NO aplica` (no se fabrica el arco).
7. Verbos prohibidos por vertical (curar/tratar/revertir/eliminar) definidos en el intake según jurisdicción.
8. **+ v4**: script anti-fabricación sobre TODOS los docs (protocolo completo en SKILL.md FASE 4) · conteos de avatares re-ejecutados por el QC · huecos solo con evidencia del intento.

## 6. Setup y requisitos

| Qué | Estado | Detalle |
|---|---|---|
| Skill `/voc-research` | ✅ instalada | `~/.claude/skills/voc-research/` (nivel usuario — todas las sesiones) |
| Tokens | ✅ | `<tu-carpeta-personal>\research_secrets.env` (`APIFY_TOKEN`, `TRENDTRACK_TOKEN`, `FOREPLAY_TOKEN`, `GETHOOKD_TOKEN`) — fuera de OneDrive, jamás en docs |
| **MCP Apify** | ⚠️ PENDIENTE | Requerido para F2. Instalar: `claude mcp add apify --scope user --env APIFY_TOKEN=<token> -- npx -y @apify/actors-mcp-server` (o el MCP hosted de Apify). Sin esto la skill no puede lanzar actors |
| **Brave Search** (discovery) | ⚠️ opcional recomendado | Con `BRAVE_API_KEY`: `claude mcp add brave-search --scope user --env BRAVE_API_KEY=<key> -- npx -y @modelcontextprotocol/server-brave-search`. Fallback sin Brave: WebSearch/Chrome (declarándolo) |
| MCP Google Drive | ✅ conectado | Subida del Spine a la carpeta del producto |
| Espionaje ads | ✅ | MCP de ad intelligence conectado + TrendTrack/Foreplay/GetHookd + links Kalodata/GetHookd de la tarjeta del producto |
| Original del sistema externo | ✅ archivado | `SISTEMAS Y SOPS INTERNOS\METODOS\SKILL-VOC-RESEARCH-AMIGO\` (NO editar — es la referencia; las adaptaciones viven solo en la skill instalada) |

## 7. Relación con los SOPs y qué quedó FUERA

- **SOP-MKT-01/05 (Research de Mercado)** → v2.0: el motor v4 es el sistema oficial de la fase B. Los prompts LLM de la era v2 (EAM/DTS sueltos) quedan como material de referencia, NO como flujo.
- **La oferta salió del research** (decisión 2026-07-17): pricing, bundles, garantía, bonos y unit economics = **SOP-MKT-04 — Construcción de Oferta**, se ejecuta DESPUÉS del Spine consolidado, con la FICHA DE PRICING del producto. El research entrega mercado/avatar/mecanismo/ángulos; la oferta es otra disciplina con otro documento.
- Los archivos v3 (spec v3.1, adenda v3.3, scripts `v31_*.py`, caso hígado MX) quedan como historia y herramientas reutilizables (`md2docx.py`, `build_xlsx.py` siguen sirviendo para entregables). El caso hígado NO se re-ejecuta con v4 salvo pedido explícito.

---
*Motor v4.0 — 2026-07-17. Fusión: motor v3.3 (doble vía, UMP/UMS/USP, conciencia×sofisticación, gates) + Master Skill Research (adquisición autónoma con gates de presupuesto, cobertura 100%, anti-fabricación mecánica, membresía de mercado, micro-señales, avatares Evolve herméticos con QC, modo incremental) + canal 0 de dato propio (Zigpoll/Judge.me). Ofertas fuera del research. La skill `/voc-research` es la ejecución; este doc es la autoridad.*
