# Motor de Research DTC v4.0

Máquina **repetible, auditable y auto-correctiva** de research pre-lanzamiento (VoC) para productos DTC (ads Meta/TikTok, metodología Schwartz). Es **product-agnostic**: el mismo motor sirve para cualquier producto/mercado. Los casos concretos son solo **ejecuciones de referencia**, no parte del motor.

## Empieza aquí
- **[MOTOR DE RESEARCH v4.0 — Spec Operativo](MOTOR%20DE%20RESEARCH%20v4.0%20—%20Spec%20Operativo.md)** — la **autoridad vigente**: mapa de docs (00, 14-21, B0/B1, 10), decisiones de diseño, pipeline F0-F6.
- **Skill ejecutable `/voc-research`** — el motor v4.0 corrible en cualquier sesión de Claude Code (instalada a nivel usuario en `~/.claude/skills/voc-research/`, **fuera de este repo**).
- **[HANDOFF.md](HANDOFF.md)** — traspaso: qué es, cómo continuar.

## Qué es v4.0
Fusión del **motor v3.3** (doble vía + UMP/UMS/USP + conciencia×sofisticación + gates anti-alucinación) con el sistema **Master Skill Research** (adquisición autónoma con gates de presupuesto, cobertura 100% sin muestreo, anti-fabricación mecánica, membresía de mercado, micro-señales, avatares Evolve herméticos con QC, canal 0 de dato propio). La **oferta/pricing salió del research** (vive en SOP-MKT-04).

## La regla núcleo (se conserva de v3.3)
Todo research combina **Vía A (deep research web, con fuentes)** + **Vía B (corpus VoC completo, con IDs)** y cierra el loop con un **documento de contraste** (Hipótesis vs Realidad VoC vs Veredicto). Donde el corpus contradice al research, **manda el corpus**. El Master Spine (doc 00) se reconcilia al final.

## Salida → Creativos
El research entrega el **Master Spine (doc 00)**. La skill **`/master-spine`** lo consolida y emite un **`spine.json`** que consume el **Motor de Creativos** (repo `motor-creativos-dtc`, etapa 2). Pipeline completo: **Research (v4.0) → Creativos → Media Buying → Feedback**.

## Linaje v3.x (auditable, en `v3.1/`)
El predecesor v3.1/v3.3 sigue aquí como base auditable: `MOTOR ... v3.1 Spec` + `v3.3 Adenda`, `evidence_bank.csv` (13,548 confesiones con IDs), `PI_SCORES.csv` + `CLUSTER_REPORT`, los docs de contraste (`15b/18b/19b/20b/COMPETITOR_MAP b`), `scripts/`. v4.0 lo **extiende**, no lo borra.

## Caso de referencia (histórico — NO es el motor)
El **suplemento hepático MX** fue el primer producto corrido con el motor (research v3.2 post-contraste): `v3.1/DOC 00 — Master Spine v3.2`, `DOC 21b — Banco de Hooks v3.2` (36 hooks), `CONSOLIDADO — Research Hígado MX.xlsx`, briefs. Vive en `v3.1/`. Es un **ejemplo de ejecución**, no parte del motor generalizado — cualquier producto nuevo arranca con la skill `/voc-research`, no con el hígado.

## Seguridad
Los tokens de API (Apify, TrendTrack, Foreplay, GetHookd, GitHub) viven **fuera del repo** en `C:\Users\Thomas\research_secrets.env`. Nunca se commitean (ver `.gitignore`).

## Estado
Motor **v4.0** (ejecutable: skill `/voc-research`) · linaje v3.3 auditable · caso hígado v3.2 (histórico, en `v3.1/`).
