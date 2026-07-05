# Motor de Research DTC v3.3 + Proyecto Hígado MX

Máquina **repetible, auditable y auto-correctiva** de research pre-lanzamiento para productos DTC (ads Meta/TikTok, metodología Schwartz), más el primer producto ejecutado con ella: un suplemento hepático para México.

## Empieza aquí
- **[HANDOFF.md](HANDOFF.md)** — traspaso completo: qué es, qué se hizo, el veredicto vigente, cómo continuar.
- **[v3.1/DOC 00 — Master Spine v3.2 (post-contraste)](v3.1/)** — el **veredicto estratégico VIGENTE** (corregido por el contraste; el frame v3 fue refutado).
- **[v3.1/MOTOR DE RESEARCH v3.3 — Adenda Doble Vía](v3.1/)** — la regla núcleo: deep research + scraping + contraste, obligatorio.
- **[CONSOLIDADO — Research Hígado MX.xlsx](CONSOLIDADO%20—%20Research%20Hígado%20MX.xlsx)** — resumen ejecutivo con tier lists + hooks v3.2.

## La regla del motor (v3.3)
Todo research combina **Vía A (deep research web, con fuentes)** + **Vía B (scraping VoC del corpus, con IDs)** y cierra el loop con un **documento de contraste** (Hipótesis vs Realidad VoC vs Veredicto). Donde el scraping contradice al research, **manda el scraping**. El Spine se reconcilia al final.

## Qué contiene
| Capa | Dónde |
|---|---|
| Motor generalizado (repetible) | `v3.1/MOTOR ... v3.1 Spec` + `v3.3 Adenda` + `v3.1/README` |
| Evidencia canónica | `v3.1/evidence_bank.csv` (13,548 confesiones con IDs) |
| Scoring transparente | `v3.1/PI_SCORES.csv` + `CLUSTER_REPORT` |
| **Contraste (Deep Research × VoC)** | `v3.1/DOC 15b/18b/19b/20b/COMPETITOR_MAP b` |
| Estrategia vigente | `v3.1/DOC 00 — Master Spine v3.2` |
| Producción | `v3.1/DOC 21b — Banco de Hooks v3.2` (36 hooks) |
| Scripts ejecutables | `scripts/` |

## Seguridad
Los tokens de API (Apify, TrendTrack, GitHub) viven **fuera del repo** en `C:\Users\Thomas\research_secrets.env`. Nunca se commitean (ver `.gitignore`).

## Estado
Motor **v3.3** · Proyecto hígado research **v3.2** (post-contraste) · Validación **APROBADO**.
