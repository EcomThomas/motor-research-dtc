# Motor de Research DTC v4.0

Máquina **repetible, auditable y auto-correctiva** de research pre-lanzamiento (VoC) para productos DTC (ads Meta/TikTok, metodología Schwartz). Es **product-agnostic**: el mismo motor sirve para cualquier producto/mercado. Los casos concretos son **ejecuciones de referencia**, no parte del motor.

## Estructura
```
motor-research-dtc/
├── MOTOR DE RESEARCH v4.0 — Spec Operativo.md   ← autoridad vigente del motor
├── motor/            ← el método reusable (spec v3.1 + adenda v3.3 + "cómo ejecutar" + spec v3)
├── scripts/          ← scripts ejecutables (ingesta, PI, validadores, md2docx)
├── material-fuente/  ← prompts fuente EAM/Evolve que el motor fusionó
├── casos/
│   └── higado-mx/    ← 1er producto corrido (EJEMPLO de ejecución, NO el motor)
└── RESEARCH DE PRODUCTOS EN TRENDTRACK/  ← product-hunting (research aparte)
```

## Empieza aquí
- **[INSTALL.md](INSTALL.md)** — 🔧 **cómo instalar el motor en tu Claude** (base, skills, tokens, MCPs, costos).
- **[MOTOR DE RESEARCH v4.0 — Spec Operativo](MOTOR%20DE%20RESEARCH%20v4.0%20—%20Spec%20Operativo.md)** — la **autoridad vigente**: mapa de docs (00, 14-21, B0/B1, 10), decisiones de diseño, pipeline F0-F6.
- **Skill ejecutable `/voc-research`** — el motor v4.0 corrible en cualquier sesión de Claude Code (a nivel usuario en `~/.claude/skills/voc-research/`, **fuera de este repo**).
- **[HANDOFF.md](HANDOFF.md)** — traspaso · **[motor/](motor/)** el método · **[casos/higado-mx/](casos/higado-mx/)** el caso de referencia.

## Qué es v4.0
Fusión del **motor v3.3** (doble vía + UMP/UMS/USP + conciencia×sofisticación + gates anti-alucinación) con el sistema **Master Skill Research** (adquisición autónoma con gates de presupuesto, cobertura 100% sin muestreo, anti-fabricación mecánica, membresía de mercado, micro-señales, avatares Evolve herméticos con QC, canal 0 de dato propio). La **oferta/pricing salió del research** (vive en SOP-MKT-04).

## La regla núcleo (se conserva de v3.3)
Todo research combina **Vía A (deep research web, con fuentes)** + **Vía B (corpus VoC completo, con IDs)** y cierra el loop con un **documento de contraste** (Hipótesis vs Realidad VoC vs Veredicto). Donde el corpus contradice al research, **manda el corpus**. El Master Spine (doc 00) se reconcilia al final.

## Salida → Creativos
El research entrega el **Master Spine (doc 00)**. La skill **`/master-spine`** lo consolida y emite un **`spine.json`** que consume el **Motor de Creativos** (repo `motor-creativos-dtc`, etapa 2). Pipeline completo: **Research (v4.0) → Creativos → Media Buying → Feedback**.

## El método (`motor/`)
El motor generalizado y repetible vive en `motor/`: `MOTOR DE RESEARCH v3.1 — Spec Operativo` + `v3.3 — Adenda Doble Vía` + `README — Cómo ejecutar este motor en otro producto` (+ el spec v3 original). El spec **v4.0** (raíz) los extiende; la **ejecución** vive en la skill `/voc-research`.

## Caso de referencia (`casos/higado-mx/` — NO es el motor)
El **suplemento hepático MX** fue el primer producto corrido con el motor (research v3.2 post-contraste): Master Spine v3.2, hooks v3.2, los 5 docs de contraste (Deep Research × VoC), briefs, corpus de 13,548 confesiones con IDs, y el `CONSOLIDADO`. Todo vive en `casos/higado-mx/` (con `v3-superado/` = los docs v3 previos al contraste). Es un **ejemplo de ejecución**, no parte del motor generalizado — cualquier producto nuevo arranca con `/voc-research`, no con el hígado.

## Seguridad
Los tokens de API (Apify, TrendTrack, Foreplay, GetHookd, GitHub) viven **fuera del repo** en `<tu-carpeta-personal>\research_secrets.env`. Nunca se commitean (ver `.gitignore`).

## Estado
Motor **v4.0** (ejecutable: skill `/voc-research`) · método/linaje en `motor/` · caso hígado v3.2 (histórico, en `casos/higado-mx/`).
