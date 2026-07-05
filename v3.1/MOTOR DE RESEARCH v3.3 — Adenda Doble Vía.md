# MOTOR DE RESEARCH v3.3 — Adenda: Doble Vía Obligatoria

> Extiende el `MOTOR DE RESEARCH v3.1 — Spec Operativo`. **Cambio de v3.1 → v3.3:** la investigación por **doble vía + contraste** deja de ser opcional y pasa a ser **FASE NÚCLEO OBLIGATORIA** del motor, para todo elemento y todo producto.

---

## 1. Por qué existe v3.3

El v3.1 (auditable, con jerarquía de fuentes y PI transparente) resolvió la **calidad y trazabilidad de la data**, pero los documentos interpretativos se generaban en **una sola pasada sobre una muestra comprimida del scraping** → salían delgados y, peor, construidos sobre **hipótesis del equipo** en vez de sobre voz real contrastada. En el caso de referencia (hígado MX) esto hizo que la estrategia v3 se montara sobre un gancho (hinchazón estética) presente en solo **~4 citas de 13,548**.

**v3.3 lo corrige haciendo obligatorio cerrar el feedback loop:** cada elemento se investiga por dos vías independientes y se contrasta.

## 2. La regla (soberana)

Para CADA elemento del research (avatar, deseos, buyer psychology, mecanismo UMP/UMS, competidores, hooks):

- **Vía A — DEEP RESEARCH:** investigación externa (web/desk), verificada, con **fuentes/URLs**. Produce HIPÓTESIS fundamentadas. Tipo de fuente: `INFERENCE`/`INTEL_FOREIGN` — **nunca cuenta como prueba de mercado por sí sola.**
- **Vía B — SCRAPING VoC:** minería del **corpus completo** (no una muestra), extrayendo un banco por elemento con citas reales e **IDs `EVxxxx`**. Tipo de fuente: `RAW_CONFESSION`/`REVIEW`.
- **CONTRASTE:** documento final por elemento, tabla de 3 columnas → **Hipótesis (A + URL) | Realidad VoC (B + ID) | Veredicto (CONFIRMA / REFUTA / MATIZA / AÑADE)** + sección de hallazgos donde la VoC refuta/matiza a A + huecos sin evidencia.
- **Regla de arbitraje:** donde **B contradice a A, manda B.** Donde A afirma sin fuente clínica/directa → se marca **[HIPÓTESIS PERSUASIVA]**, no hecho.
- **Spine al final:** el Master Spine se **reconcilia con todos los contrastes** (changelog honesto de qué cambió), nunca antes.

## 3. Pipeline v3.3 (dónde encaja)

```
Fase 0-2  (v3.1)  Intake → scraping por pools → ingesta → PI transparente → DATA QUALITY
Fase 3    (v3.1)  Banco canónico DOC 14 (evidence_bank.csv con IDs)
Fase 4    NUEVA·OBLIGATORIA — extraer bancos VoC por elemento (voc_banks/)
Fase 5    NUEVA·OBLIGATORIA — por cada elemento: Vía A (deep research web) ‖ Vía B (VoC corpus)
Fase 6    NUEVA·OBLIGATORIA — Contraste por elemento (docs "b": 15b/18b/19b/20b/COMPETITOR_MAP b)
Fase 7    (v3.1) RECONCILIATION + Spine reconciliado al final
Fase 8    (v3.1) Validadores (VALIDATION REPORT)
Fase 9    Producción (hooks/advertoriales) alineada al Spine reconciliado
```

## 4. Artefactos que produce (por elemento)
- `voc_banks/<elemento>.md` — banco de citas reales con ID del corpus completo.
- `research/A1-<elemento>.md`, `research/A2-<elemento>.md` — deep research con fuentes.
- `research/B-<elemento>.md` — perfil VoC anclado en IDs.
- `<DOC> b — Deep Research × VoC Contrastado.md` — el documento de contraste (entregable).

## 5. Criterio de "hecho bien" (QA del contraste)
Un doc de contraste está bien solo si: es **completo** (tablas escritas, no un resumen), cita **≥8 IDs `EVxxxx` reales** y **≥8 veredictos**, marca explícitamente qué es HECHO vs HIPÓTESIS, y donde B contradijo A **mandó B**. Arranca directo con `#`, sin texto de proceso del LLM (lo caza el validador).

## 6. Caso de referencia ejecutado
Proyecto hígado MX: 5 docs de contraste (`DOC 15b/18b/19b/20b/COMPETITOR_MAP b`), `DOC 00 — Master Spine v3.2 (post-contraste)` con changelog de 14 cambios, y `DOC 21b` (hooks alineados). El contraste **invirtió el núcleo del avatar** (madre creyente aterrada por el diagnóstico, no la hinchada estética) — prueba de que la fase es load-bearing, no cosmética.

---
*Adenda v3.3 · la doble vía + contraste es ahora fase obligatoria del motor. Complementa el spec v3.1 y no lo reemplaza.*
