# HANDOFF — Motor de Research DTC v3.3 + Proyecto Hígado MX

> Documento de traspaso. Explica qué es este repo, qué se construyó, dónde está cada cosa y cómo continuar.
> **Estado: Motor v3.3 · Proyecto hígado con research v3.2 (post-contraste) completo.** Última actualización: 2026-07-05.

---

## 1. Qué es este repositorio

Dos cosas conviven aquí:

1. **El MOTOR DE RESEARCH v3.3** — una máquina **repetible, auditable y auto-correctiva** para investigación pre-lanzamiento de productos DTC (ads Meta/TikTok, metodología Eugene Schwartz). Va desde "¿qué desea el mercado?" hasta hooks listos para pauta, con evidencia trazable por IDs.
2. **El primer producto ejecutado con el motor** — un suplemento de salud hepática (base competidor **Purelia**) para lanzar en **México**, ampliado a salud intestinal. Es el **caso de referencia** que prueba el motor.

Origen: fusión de dos sistemas previos del negocio (**Desire-To-Scale 2.0** y **Evolve/EAM**) + un **Playbook VoC** con scraping real, refinado a v3.1 (auditable) y v3.3 (doble vía obligatoria).

---

## 2. LA REGLA NÚCLEO del motor (v3.3) — léela primero

**Todo research combina DOS VÍAS y cierra el loop. No es opcional.**
- **Vía A — Deep research** (web/desk, con fuentes/URLs) → hipótesis fundamentadas externas.
- **Vía B — Scraping VoC** del corpus real completo (con IDs `EVxxxx`) → lo que el cliente realmente dice.
- **Contraste** — documento por elemento: **Hipótesis (A) | Realidad VoC (B+ID) | Veredicto (CONFIRMA/REFUTA/MATIZA/AÑADE)**.
- **Regla soberana: donde B contradice a A, manda B.** El Spine se reconcilia AL FINAL.

Detalle en `v3.1/MOTOR DE RESEARCH v3.3 — Adenda Doble Vía.md`.

---

## 3. Estado actual (qué está hecho)

| Bloque | Estado |
|---|---|
| Motor v3.1 (auditable: fuentes, IDs, PI transparente, validadores) | ✅ |
| **Motor v3.3 (doble vía + contraste obligatorios)** | ✅ adenda |
| Corpus real escrapeado | ✅ **13,548 confesiones** (`v3.1/evidence_bank.csv`) |
| Docs de research v3 (avatar, psych, UMP, hooks, spine) | ✅ (superados por los "b") |
| **Docs de CONTRASTE v3.2** (15b, 18b, 19b, 20b, COMPETITOR_MAP b) | ✅ ~164 citas ID, ~173 veredictos |
| **Spine v3.2 (post-contraste)** con changelog de 14 cambios | ✅ |
| **Hooks v3.2** (DOC 21b, alineados al frame corregido) | ✅ 36 hooks |
| Consolidado Excel (tier lists + Hooks v3.2) | ✅ |
| Todo a Word | ✅ |
| Validación | ✅ **APROBADO** (0 residuales, 0 IDs colgantes) |

---

## 4. Estructura del repositorio

```
DOCUMENTOS DE INVESTIGACIÓN/
├── HANDOFF.md · README.md                       ← empieza aquí
├── MOTOR DE RESEARCH v3 — Spec Operativo.md      ← spec conceptual original
├── CONSOLIDADO — Research Hígado MX.xlsx         ← resumen ejecutivo (8 pestañas, incl. Hooks v3.2)
│
├── FASE 0 · DOC 00/15/16/19/20/21 (.md + .docx)  ← research v3 (SUPERADO por los "b")
├── scripts/                                      ← el motor ejecutable
│   ├── v31_foundation.py  (ingesta + jerarquía de fuentes + PI transparente)
│   ├── v31_docs.py        (DATA QUALITY / CLUSTER_REPORT / DOC 14)
│   ├── validate_v31.py    (validadores → VALIDATION REPORT)
│   ├── md2docx.py · build_xlsx.py
│
├── 1)..7) *.docx/.txt                            ← material fuente (prompts Evolve/EAM)
│
└── v3.1/  ← EL MOTOR AUDITABLE + LA CAPA DE CONTRASTE v3.2 (todo con .docx)
    ├── MOTOR DE RESEARCH v3.1 — Spec Operativo.md
    ├── MOTOR DE RESEARCH v3.3 — Adenda Doble Vía.md      ← la regla núcleo v3.3
    ├── README — Cómo ejecutar este motor en otro producto.md
    ├── DATA QUALITY REPORT.md · VALIDATION REPORT.md
    ├── DOC 14 — Banco de Evidencia Canónico.md + evidence_bank.csv (13,548 IDs)
    ├── PI_SCORES.csv · CLUSTER_REPORT.md
    ├── COMPETITOR_MAP.md · RECONCILIATION PASS.md
    ├── DOC 00 — Master Spine actualizado.md              (v3.1)
    ├── ─────────── CAPA DE CONTRASTE v3.2 ───────────
    ├── DOC 15b — Avatar · DOC 18b — Deseos · DOC 19b — Mecanismo
    ├── DOC 20b — Buyer Psychology · COMPETITOR_MAP b     (Deep Research × VoC)
    ├── DOC 00 — Master Spine v3.2 (post-contraste).md    ← EL VERDICTO VIGENTE
    ├── DOC 21b — Banco de Hooks v3.2.md                  ← hooks para producción
    ├── voc_banks/     (bancos VoC por elemento, 90 citas c/u)
    └── research/      (Vía A con fuentes + Vía B por elemento)
```

---

## 5. El veredicto estratégico VIGENTE (Spine v3.2 — corregido por el contraste)

⚠️ **El frame v3 (gancho de hinchazón estética) fue REFUTADO por la voz real.** El veredicto vigente es el v3.2:

- **Avatar núcleo:** **madre creyente mexicana 45-65 recién diagnosticada de hígado graso, aterrada de MORIR** (vio morir a alguien de esto). *No* la "mujer 40+ hinchada estética" (esa vive en ~4 citas → cabeza de playa para tráfico frío, a validar).
- **Gancho:** el **terror del diagnóstico** + **"revertirlo por vía natural, sin cirugía ni pastillas de por vida"**. NO "panza plana".
- **Emoción:** desesperación/terror → esperanza redentora (fe como capa de tono, con respeto).
- **Miedo dominante:** mortal. **Objeción raíz:** seguridad ("¿me hará daño? ¿choca con mi estatina?"), NO precio ni fraude.
- **Mecanismo:** "limpiar el hígado" (folk nativo) + bilis/alcachofa (explicación honesta). **Prohibido "detox".** El puente hinchazón→hígado **NO tiene respaldo clínico** → framear como testimonio, riesgo de compliance.
- **Prueba:** ultrasonido limpio + kilos + cifra (enzimas = blindaje US, no gancho MX).
- **Villano:** frituras/azúcar + el **DIY casero** que odian mantener → las gotas = el atajo.
- **Arbitraje:** el DR de hígado/intestino está dominado por marcas EN/EU; ES-MX casi vacío.

## 6. Integraciones (secretos FUERA del repo)
- **Apify** (scraping VoC, plan Scale) y **TrendTrack** (competidores/ads) por API.
- Tokens en `C:\Users\Thomas\research_secrets.env` — **FUERA del repo, nunca commiteados** (`APIFY_TOKEN`, `TRENDTRACK_TOKEN`, `GITHUB_TOKEN`).
- **GitHub:** repo privado `github.com/EcomThomas/motor-research-dtc`. Nota: el auto-mode bloquea el push automático (exfiltración) → el push lo corre el usuario.

## 7. Cómo continuar (próximos pasos)
1. **Validar el avatar-hinchazón** (~4 citas) con scraping dirigido antes de gastar presupuesto en la cabeza de playa.
2. **Compliance** del puente hinchazón→hígado antes de escalar pauta pagada.
3. **Advertoriales / copy largo** desde `DOC 21b` (alineados al Spine v3.2, no al v3).
4. **Deuda técnica:** migrar las citas de los docs v3 a EV-IDs (los IDs ya existen).
5. **Correr el motor v3.3 en un 2º producto** siguiendo `v3.1/README` (con la doble vía desde el inicio).

## 8. Cómo se ejecuta el motor (resumen técnico)
```
Fase 0-3   Intake → scraping por pools (Apify) → ingesta (v31_foundation.py) →
           PI transparente + DATA QUALITY + DOC 14 (v31_docs.py)
Fase 4-6   [v3.3] bancos VoC por elemento → doble vía (deep research ‖ VoC) → contraste
Fase 7-8   RECONCILIATION → Spine reconciliado al final → validadores (validate_v31.py)
Fase 9     Producción (hooks/advertoriales) alineada al Spine
```
Detalle generalizado en `v3.1/MOTOR DE RESEARCH v3.1 — Spec Operativo.md` + `v3.3 — Adenda` + `README`.

---
*Handoff v3.3. Compliance queda fuera de alcance a propósito. El corpus crudo (raw/) y los tokens NO están en el repo por diseño.*
