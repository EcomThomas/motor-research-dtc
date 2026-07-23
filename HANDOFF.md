# HANDOFF — Motor de Research DTC v4.0

> Traspaso del repo: qué es, dónde está cada cosa y cómo continuar.
> **Estado: Motor v4.0** (fusión v3.3 + Master Skill Research), ejecutable como skill `/voc-research`. El **hígado MX** es un **caso de referencia histórico**, no el motor.

---

## 1. Qué es este repositorio

El **Motor de Research DTC**: máquina **repetible, auditable, auto-correctiva y product-agnostic** de research VoC pre-lanzamiento (ads Meta/TikTok, metodología Schwartz). Va de "¿qué desea el mercado?" hasta el veredicto estratégico (Master Spine), con evidencia trazable por IDs. Es la **ETAPA 1** de un pipeline de 4: **Research → Creativos → Media Buying → Feedback**.

Origen: fusión de sistemas previos del negocio (Desire-To-Scale 2.0, Evolve/EAM) + Playbook VoC → v3.1 (auditable) → v3.3 (doble vía) → **v4.0** (fusión con el sistema Master Skill Research: adquisición autónoma, cobertura 100%, anti-fabricación mecánica, avatares Evolve, canal 0).

## 2. Autoridad y ejecución
- **Autoridad de diseño (vigente):** `MOTOR DE RESEARCH v4.0 — Spec Operativo.md` (raíz) — mapa de docs 00/14-21/B0/B1/10, pipeline F0-F6.
- **Ejecución:** skill **`/voc-research`** (nivel usuario, `~/.claude/skills/voc-research/`, **fuera del repo**).
- **Método / linaje:** `motor/` (spec v3.1 + adenda v3.3 + "cómo ejecutar en otro producto" + spec v3).

## 3. LA REGLA NÚCLEO (se conserva de v3.3)
Todo research combina **DOS VÍAS** y cierra el loop:
- **Vía A — Deep research** (web/desk, con fuentes/URLs) → hipótesis fundamentadas.
- **Vía B — Corpus VoC** completo (con IDs `EVxxxx`) → lo que el cliente realmente dice.
- **Contraste** por elemento: **Hipótesis (A) | Realidad VoC (B+ID) | Veredicto (CONFIRMA/REFUTA/MATIZA/AÑADE)**.
- **Regla soberana: donde B contradice a A, manda B.** El Master Spine (doc 00) se reconcilia AL FINAL.

## 4. Estructura del repositorio
```
motor-research-dtc/
├── README.md · HANDOFF.md                       ← empieza aquí
├── MOTOR DE RESEARCH v4.0 — Spec Operativo.md    ← autoridad vigente del motor
├── motor/            ← el método reusable (spec v3.1 + adenda v3.3 + "cómo ejecutar" + spec v3)
├── scripts/          ← ejecutables (v31_foundation, v31_docs, validate_v31, md2docx, build_xlsx)
├── material-fuente/  ← prompts fuente EAM/Evolve que el motor fusionó
├── casos/
│   └── higado-mx/    ← 1er producto corrido (EJEMPLO, no el motor)
│       ├── DOC 00 Spine v3.2 · DOC 14/15b/18b/19b/20b · DOC 21b hooks · COMPETITOR_MAP b
│       ├── evidence_bank.csv (13,548 IDs) · PI_SCORES · reports · briefs/ · research/ · voc_banks/
│       └── v3-superado/  ← docs v3 previos al contraste (FASE 0, DOC 00/15/16/19/20/21) + CONSOLIDADO
└── RESEARCH DE PRODUCTOS EN TRENDTRACK/  ← product-hunting (research aparte, no hígado)
```

## 5. Salida → Creativos (encadenamiento)
El research entrega el **Master Spine (doc 00)**. La skill **`/master-spine`** lo consolida y emite un **`spine.json`** que consume el **Motor de Creativos** (repo `motor-creativos-dtc`). Así el veredicto de research alimenta directo la generación de baches/creativos.

## 6. Caso de referencia — Hígado MX (`casos/higado-mx/`, histórico, NO es el motor)
Primer producto corrido (research v3.2 post-contraste). Veredicto vigente de ESE caso:
- **Avatar núcleo:** madre creyente mexicana 45-65 recién diagnosticada de hígado graso, aterrada de MORIR. *No* la "hinchada estética 40+" (~4 citas → cabeza de playa a validar).
- **Gancho:** terror del diagnóstico + "revertirlo natural, sin cirugía ni pastillas". **Emoción:** terror→esperanza (fe con respeto).
- **Objeción raíz:** seguridad ("¿me hará daño?/¿choca con mi estatina?"). **Mecanismo:** "limpiar el hígado" + bilis/alcachofa; **prohibido "detox"**; el puente hinchazón→hígado sin respaldo clínico → testimonio.
- **Prueba:** ultrasonido limpio + kilos. **Villano:** frituras/azúcar + el DIY casero.

Es un **ejemplo de ejecución** que probó el motor, no parte del motor generalizado.

## 7. Cómo continuar
- **Producto nuevo:** correr **`/voc-research`** (pipeline F0-F6 del spec v4.0) → produce docs 14-21 + B0/B1 + 10 → **`/master-spine`** consolida el doc 00 y emite el `spine.json` → entra al Motor de Creativos.
- **Setup pendiente del v4** (ver spec §6): conectar **MCP Apify** (requerido para F2). Brave Search opcional.
- **90/95%:** la oferta/pricing quedó **fuera** del research (SOP-MKT-04).

## 8. Seguridad / GitHub
- Tokens (`APIFY_TOKEN`, `TRENDTRACK_TOKEN`, `FOREPLAY_TOKEN`, `GETHOOKD_TOKEN`, `GITHUB_TOKEN`) en `<tu-carpeta-personal>\research_secrets.env` — **fuera del repo**, nunca commiteados (`.gitignore` protege `*token*`/`*.env`).
- Repo privado `github.com/EcomThomas/motor-research-dtc`.

---
*Handoff v4.0. La oferta queda fuera del research (SOP-MKT-04). El corpus crudo y los tokens NO están en el repo por diseño.*
