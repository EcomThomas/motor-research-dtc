# INSTALL — Cómo instalar el motor en tu Claude

Guía de instalación del stack completo: **Motor de Research v4.0** (etapa 1) + **Motor de Creativos** (etapa 2) + las skills que los encadenan. Tiempo estimado: 20-30 min.

---

## 0. Base (obligatorio para todo)

| Requisito | Cómo verificar |
|---|---|
| **Claude Code** | Los motores son *skills* + *Workflow scripts* de Claude Code. Sin esto no corre nada. |
| **Python 3.10+** | `python --version` |
| **Git** + acceso a los repos (son **privados** — pídeselo al dueño) | `git --version` |
| **Node.js / npx** | Solo si vas a usar el MCP de Apify (research). `node --version` |

```bash
pip install -r requirements.txt      # openpyxl + python-docx
```

---

## 1. Motor de Research v4.0 (etapa 1 — produce el Spine)

### 1.1 Clonar
```bash
git clone https://github.com/EcomThomas/motor-research-dtc.git
cd motor-research-dtc
pip install -r requirements.txt
```

### 1.2 Instalar la skill `/voc-research`
Copia `skills/voc-research/` a tus skills de usuario, para que quede disponible en **todas** tus sesiones:

- **Windows:** `xcopy /E /I "skills\voc-research" "%USERPROFILE%\.claude\skills\voc-research"`
- **macOS/Linux:** `cp -r skills/voc-research ~/.claude/skills/`

Verifica escribiendo `/voc-research` en Claude Code.

> **Nota:** el método de avatares está en `evolve-avatares.md`. El PDF original del training de Evolve **no se distribuye** (material de terceros); no hace falta para correr la skill.

### 1.3 Tus tokens
Crea un archivo de secretos **fuera del repo** (p. ej. `C:\Users\<tu-usuario>\research_secrets.env`):
```
APIFY_TOKEN=...
TRENDTRACK_TOKEN=...      # o FOREPLAY_TOKEN / GETHOOKD_TOKEN (≥1 ad-spy)
```

### 1.4 MCP de Apify (requerido para la fase de scraping, F2)
```bash
claude mcp add apify --scope user --env APIFY_TOKEN=<tu-token> -- npx -y @apify/actors-mcp-server
```
*Opcional recomendado* — Brave Search para el discovery (sin él usa WebSearch):
```bash
claude mcp add brave-search --scope user --env BRAVE_API_KEY=<tu-key> -- npx -y @modelcontextprotocol/server-brave-search
```
*Opcional* — conector de **Google Drive** en tu Claude (Settings → Connectors) para subir el Spine.

### 1.5 Correr
Abre Claude Code en la carpeta del repo y escribe **`/voc-research`**. El pipeline (F0-F6) está en `MOTOR DE RESEARCH v4.0 — Spec Operativo.md`.

---

## 2. Skill `/master-spine` (el puente research → creativos)

Consolida los docs del research en el **Master Spine (doc 00)** y emite el **`spine.json`** que consume el Motor de Creativos. Vive en el repo de creativos (`skills/master-spine/`):

- **Windows:** `xcopy /E /I "skills\master-spine" "%USERPROFILE%\.claude\skills\master-spine"`
- **macOS/Linux:** `cp -r skills/master-spine ~/.claude/skills/`

Sin tokens ni configuración. Se invoca con `/master-spine`.

---

## 3. Motor de Creativos (etapa 2 — produce los baches y briefs)

```bash
git clone https://github.com/EcomThomas/motor-creativos-dtc.git
cd motor-creativos-dtc
pip install -r requirements.txt
cp .env.example .env                       # pon tu CLICKUP_TOKEN (obligatorio)
cp config.local.example.json config.local.json   # tu lista de ClickUp, Drive, producto
```

- **`CLICKUP_TOKEN`**: ClickUp → avatar → *Settings → Apps → API Token → Generate* (empieza con `pk_`).
- **`config.local.json`**: `clickup_list_id` (de la URL `.../v/li/<NUM>`), `clickup_status` (`idea`), `drive_parent_folder_id`, `producto`, `spine_path`.
- Detalle completo en `.claude/skills/subir-batch-dtc/SETUP.md` del repo.

**Verificar:**
```bash
python -c "from scripts.motor_config import store, secrets_path, get_token; print('secretos:', secrets_path()); print('clickup ok:', bool(get_token('CLICKUP_TOKEN')))"
```
Debe imprimir la ruta de TU `.env` y `clickup ok: True`.

El flujo operativo (intake → `wf_motor` → persist → ClickUp) está en `RUNBOOK.md`.

---

## 4. Cuentas y costo por research

| Servicio | Necesidad | Costo aprox. |
|---|---|---|
| **Apify** | obligatorio para el research | **$40-150** por research |
| Cómputo (agentes) | obligatorio | **$60-85** por research |
| Ad-spy (TrendTrack / Foreplay / GetHookd) | ≥1, para los guiones ganadores | suscripción |
| **ClickUp** | obligatorio para el entregable de creativos | workspace + lista propia |
| Google Drive | opcional | — |

---

## 4.1 Créditos: quién paga qué (LÉELO)

**Cada quien corre con SUS propias credenciales y su cuenta paga su consumo.** El motor está hecho para eso:

| Costo | Lo paga | Cómo se aísla |
|---|---|---|
| **Apify** (scraping del research) | Tu cuenta de Apify | Tu `APIFY_TOKEN`, en TU archivo de secretos |
| **Ad-spy** (TrendTrack/Foreplay/GetHookd) | Tu suscripción | Tu token |
| **Agentes de Claude** (el grueso del cómputo) | **Tu cuenta de Claude** | Corres el motor en TU Claude Code |
| **ClickUp** | Tu token personal (`pk_...`) + **tu propia lista** | `config.local.json` es tuyo y está gitignored |

**Reglas que evitan que le gastes créditos a otro:**
1. **Nunca compartas ni pidas tokens ajenos.** `.env` y `research_secrets.env` están gitignored: no viajan en el repo.
2. El motor **no tiene fallback a credenciales de nadie**: si no encuentra TU archivo de secretos, **falla con un mensaje claro** en vez de usar otro. Resolución: `MOTOR_SECRETS` → `<repo>/.env` → error.
3. Verifica de quién son los secretos que estás usando:
```bash
python -c "from scripts.motor_config import secrets_path; print(secrets_path())"
```
Debe imprimir **tu** ruta. Si imprime la de otra persona, estás gastando sus créditos: corrige tu `.env`.

> ⚠️ **La única excepción compartida:** si todos trabajan en el **mismo workspace de ClickUp**, los *asientos* los paga el dueño del workspace. Las tareas se crean con tu token en tu lista, pero la suscripción del workspace es del dueño. Coordínalo con él.

---

## 5. Camino mínimo (sin gastar en research)

Si **ya tienes un Spine** de tu producto, puedes correr **solo el Motor de Creativos**: te basta Claude Code + Python + el repo + `CLICKUP_TOKEN`. El motor de research (Apify + presupuesto) lo necesitas recién cuando quieras **generar** el Spine desde cero.

---

## 6. Lo que NUNCA se comparte

Tokens (`.env`, `research_secrets.env`), `config.local.json` y los casos reales (`casos/*/`) están **gitignored**: son tuyos y no tocan el repo. Cada persona usa sus propios tokens y su propia lista de ClickUp.
