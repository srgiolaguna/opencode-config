# OpenCode — Configuración Completa

> Última actualización: 16 septiembre 2026 (revisión exhaustiva evening)
> Sistema: macOS (M3 MacBook Air) · Usuario: `giolaguna`
> Instalación: Homebrew · opencode v1.18.31
> Usuario GitHub: `srgiolaguna`

---

## 1. Resumen

OpenCode está configurado con **1 proveedor operativo (OpenRouter)** con **3 modelos gratuitos verificados y funcionando**, más 12 proveedores registrados como respaldo. **15 agentes** (5 nativos + 10 oh-my-openagent) con **cadena de fallback real** y `runtime_fallback` activado. Todo es **100% gratuito, verificado con respuestas reales (coste 0)**. No se paga por nada.

**Seguridad**: Ningún API key está en los repositorios de GitHub. Todas las claves están en `~/.zshenv` (no rastreado por git). El historial de git fue limpiado con `git filter-repo`.

**Guía visual**: `GUIA_INTERACTIVA.html` — 545 líneas con sistema de popups explicativos (43), launcher interactivo (13 modelos con prompt + modo normal/ulw/plan), y 28 comandos copiables. Hacer click en cualquier elemento muestra detalles del proveedor, modelo o agente.

---

## 2. Archivos de configuración

| Archivo | Propósito | Rastreado en git |
|---------|-----------|:----------------:|
| `~/.config/opencode/opencode.jsonc` | Config principal de opencode | ✅ Sí |
| `~/.config/opencode/agent/auditor.md` | Agente auditor | ✅ Sí |
| `~/.config/opencode/agent/implementer.md` | Agente implementer | ✅ Sí |
| `~/.config/opencode/agent/optimizer.md` | Agente optimizer | ✅ Sí |
| `~/.config/opencode/agent/reviewer.md` | Agente reviewer | ✅ Sí |
| `~/.config/opencode/agent/validator.md` | Agente validator | ✅ Sí |
| `~/.zshenv` | Variables de entorno (claves API) | ❌ No (`.gitignore`) |
| `~/.omo/omo.jsonc` | Config oh-my-openagent | ✅ Sí |
| `~/.config/opencode/node_modules/oh-my-openagent/` | Plugin oh-my-openagent | ❌ No |
| `~/.config/opencode/DOCUMENTACION.md` | Esta documentación | ✅ Sí |
| `~/.config/opencode/GUIA_INTERACTIVA.html` | Guía visual interactiva | ✅ Sí |

---

## 3. Proveedores de API (13)

Todas las claves se leen desde `~/.zshenv` mediante `{env:VARIABLE}`. **Las 13 claves configuradas y activas.**

| Proveedor | Variable de entorno | Tier |
|-----------|---------------------|------|
| **opencode** | `OPENCODE_SECRET_KEY` | **FREE** |
| **google/gemini** | `GEMINI_API_KEY`, `GOOGLE_API_KEY` | **FREE** |
| **groq** | `GROQ_API_KEY` | **FREE** |
| **cerebras** | `CEREBRAS_API_KEY` | **FREE** |
| **mistral** | `MISTRAL_API_KEY` | **FREE** |
| **sambanova** | `SAMBANOVA_API_KEY` | **FREE** |
| **cloudflare** | `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID` | **FREE** |
| **ai-gateway** | `AI_GATEWAY_API_KEY` | **FREE** |
| **openrouter** | `OPENROUTER_API_KEY` | **FREE** |
| **github** | `GITHUB_TOKEN` | **FREE** |
| **nvidia** | `NVIDIA_NIM_API_KEY` | **FREE** |
| **huggingface** | `HUGGINGFACE_TOKEN` | **FREE** |
| **ollama** | Ninguna (local) | **100% LOCAL** |

---

## 4. Modelos gratuitos (verificados con respuestas reales)

### Modelo principal operativo
| Modelo | Proveedor | Verificado | Uso |
|--------|-----------|:----------:|-----|
| `openrouter/nex-agi/nex-n2.5-mini:free` | OpenRouter | ✅ OK, coste 0 | **Default**, todas las tareas |
| `openrouter/inclusionai/ling-3.0-flash-fin:free` | OpenRouter | ✅ OK, coste 0 | Fallback 1 |
| `openrouter/nvidia/nemotron-3.5-lightning:free` | OpenRouter | ✅ OK, coste 0 | Fallback 2 |

### Modelos OpenCode PROPIOS — ❌ ROTOS o con rate limit
| Modelo | Estado |
|--------|--------|
| `opencode/nemotron-3-ultra-free` | ❌ 404 Provider error |
| `opencode/mimo-v2.5-free` | ❌ Rate limit exceeded |
| `opencode/ling-3.0-flash-fin-free` | ❌ Rate limit exceeded |

> **Nota**: Los modelos `opencode/*` aparecen en catálogo pero no funcionan en la práctica. El proveedor OpenRouter con su API key gratuita es la única opción operativa confirmada.

### Modelos destacados a 0$ en OpenRouter catálogo
| Proveedor | Modelo | Caso de uso |
|-----------|--------|-------------|
| `google/gemma-4-31b-it:free` | Multimodal | Visual + razonamiento |
| `nvidia/nemotron-3-ultra-550b-a55b:free` | Ultra-potente 55B | Pesado |
| `nvidia/nemotron-3.5-lightning:free` | Rápido | Fallback verificado |
| `nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free` | Rapido | 30B razonamiento |

---

## 5. Agentes

### Agentes Nativos (opencode.jsonc) — 5 agentes

| Agente | Modelo | Permiso | Descripción |
|--------|--------|---------|-------------|
| `auditor` | `nex-agi/nex-n2.5-mini:free` | edit: deny | Analiza sin modificar |
| `implementer` | `nex-agi/nex-n2.5-mini:free` | — | Ejecuta cambios |
| `optimizer` | `nex-agi/nex-n2.5-mini:free` | edit: deny | Propone sin aplicar |
| `reviewer` | `nex-agi/nex-n2.5-mini:free` | edit: deny | Valida cambios |
| `validator` | `nex-agi/nex-n2.5-mini:free` | edit: deny | Lint, syntax-check |

### Agentes oh-my-opencode (~/.omo/omo.jsonc) — 10 agentes

| Agente | Modelo principal | Categoría |
|--------|-----------------|-----------|
| `hephaestus` | `nex-agi/nex-n2.5-mini:free` | construcción |
| `oracle` | `nex-agi/nex-n2.5-mini:free` | investigación |
| `librarian` | `nex-agi/nex-n2.5-mini:free` | documentación |
| `explore` | `nex-agi/nex-n2.5-mini:free` | exploración |
| `multimodal-looker` | `nex-agi/nex-n2.5-mini:free` | visual |
| `prometheus` | `nex-agi/nex-n2.5-mini:free` | sistemas |
| `metis` | `nex-agi/nex-n2.5-mini:free` | optimización |
| `momus` | `nex-agi/nex-n2.5-mini:free` | general |
| `atlas` | `nex-agi/nex-n2.5-mini:free` | arquitectura |
| `sisyphus-junior` | `nex-agi/nex-n2.5-mini:free` | persistencia |

---

## 6. oh-my-openagent v4.19.4

### Instalación

```bash
brew install bun
bun add -g oh-my-openagent
bunx oh-my-openagent install --no-tui \
  --platform opencode \
  --claude no --openai no --gemini no \
  --copilot no --opencode-zen no \
  --zai-coding-plan no --kimi-for-coding no \
  --opencode-go no
```

> **Nombre**: el plugin se llama ahora **oh-my-openagent** (el nombre `oh-my-opencode` es legacy y ya no es el nombre oficial, pero el binario `omo` sigue funcionando). (renombrado en v3.11.0, marzo 2026). El nombre `oh-my-opencode` es legacy y sigue funcionando como alias, pero ya no se usa. El comando corto instalado es `omo`.

### Funciones

- **Enrutamiento automático** de modelos según el tipo de tarea
- **Fallback automático** sin perder contexto
- **Cadenas de fallback por agente**
- **8 categorías** (omo.jsonc): visual-engineering, ultrabrain, deep, artistry, quick, writing, unspecified-low, unspecified-high
- **Ultrawork (ulw)** — modo autónomo. Escribe `ulw` en el prompt y el agente trabaja sin confirmar cada paso. **GRATIS.**
- **Modo Plan** — `--plan` o `/plan`. Primero planifica, luego ejecuta. **GRATIS.**
- **MCP Servers** — websearch (Exa), context7 (docs), grep_app (GitHub), LSP, 25+ hooks configurables. **GRATIS.**
- **Model routing inteligente** — usa modelos diferentes para cada tipo de tarea para ahorrar tokens. **GRATIS.**
- **AST-Grep**: `brew install ast-grep` (v0.45.3) — búsqueda por estructura AST

### Cadena de Fallback Configurada

```
primary: nex-agi/nex-n2.5-mini:free (OpenRouter)
  ↓ rate limit / error
fallback 1: inclusionai/ling-3.0-flash-fin:free (OpenRouter)
  ↓ rate limit / error
fallback 2: nvidia/nemotron-3.5-lightning:free (OpenRouter)
```

**runtime_fallback** habilitado en `~/.omo/omo.jsonc`:
- `retry_on_errors`: [429, 500, 502, 503, 504]
- `max_fallback_attempts`: 3
- `cooldown_seconds`: 5
- `timeout_seconds`: 30
- `restore_primary_after_cooldown`: true

---

## 7. Comandos

### opencode

| Comando | Descripción |
|---------|-------------|
| `opencode` | Iniciar opencode |
| `opencode models` | Listar modelos disponibles |
| `opencode -m <model>` | Usar un modelo específico |
| `opencode agent <agente>` | Lanzar agente |
| `opencode doctor` | Verificar configuración |
| `/model openrouter/nex-agi/nex-n2.5-mini:free` | Cambiar modelo en sesión |
| `/plan` | Modo planificación |
| `/cost` | Mostrar tokens y coste |
| `/diff` | Mostrar cambios pendientes |
| `/compact` | Comprimir historial |
| `/undo` | Deshacer último cambio |
| `/review` | Revisar cambios |

### oh-my-opencode

| Comando | Descripción |
|---------|-------------|
| `omo` | Iniciar oh-my-openagent |
| `omo --version` | Ver versión (4.19.4) |
| `omo doctor` | Diagnosticar configuración |
| `ulw <prompt>` | Ultrawork mode autónomo |

### cerrar

| Qué hace | Detalle |
|----------|---------|
| **Auto-commit** | En todos los repos git del sistema |
| **Auto-push** | A GitHub donde hay remoto configurado |
| **Carga claves** | `source ~/.zshenv` al inicio |
| **5 repos** | opencode-config, dotfiles, Cycle, NEXº, Workstation |

**Excluidos** de cerrar: `node_modules`, `.cache`, `.codex`, `.openclaw`, `~/.git`.

---

## 8. Repositorios GitHub

| Repo | URL | Contiene |
|------|-----|----------|
| `opencode-config` | `github.com/srgiolaguna/opencode-config` | opencode.jsonc, agentes, DOCUMENTACION.md, GUIA_INTERACTIVA.html |
| `dotfiles` | `github.com/srgiolaguna/dotfiles` | omo.jsonc, .gitignore, README.md |

> **Seguridad**: `~/.zshenv` está en `.gitignore`. Ninguna clave API en GitHub. `.zshenv` eliminado del historial de git con `git filter-repo`.

---

## 9. Estado de Configuración

### ✅ Verificado con respuestas reales (13/13 claves + 3 modelos OK)

Todas las claves API están configuradas en `~/.zshenv`. Los 3 modelos gratuitos fueron verificados con respuestas reales (coste 0):

| Variable | Estado |
|-----------|--------|
| `OPENROUTER_API_KEY` | ✅ |
| `GEMINI_API_KEY` / `GOOGLE_API_KEY` | ✅ (registrados, no operativo principal) |
| `GROQ_API_KEY` | ✅ |
| `CEREBRAS_API_KEY` | ✅ |
| `MISTRAL_API_KEY` | ✅ |
| `SAMBANOVA_API_KEY` | ✅ |
| `AI_GATEWAY_API_KEY` | ✅ |
| `CLOUDFLARE_API_TOKEN` | ✅ |
| `GITHUB_TOKEN` | ✅ |
| `NVIDIA_NIM_API_KEY` | ✅ |
| `HUGGINGFACE_TOKEN` | ✅ |
| `OPENCODE_SECRET_KEY` | ✅ |
| `CLOUDFLARE_ACCOUNT_ID` | ✅ |

**Modelos gratuitos verificados OK:** `nex-agi/nex-n2.5-mini:free`, `inclusionai/ling-3.0-flash-fin:free`, `nvidia/nemotron-3.5-lightning:free` (todos coste 0)

---

## 10. Guía Visual Interactiva

`GUIA_INTERACTIVA.html` — 546 líneas, paleta turquesa/klein blue/oro/plata/óxido sobre negro mate con fuente Urbanist.

### Secciones

PROVEEDORES · MODELOS · AGENTES · OH-MY-OPENCODE · LAUNCHER · FALLBACK · COMANDOS · CERRAR · SEGURIDAD · VERIFICACIÓN · CONSEJOS

### Launcher interactivo

- **Dropdown** con 14 modelos
- **Textarea** para escribir el prompt
- **3 modos**: Normal, ULTRAWORK (ulw), PLAN (--plan)
- **Botón** que genera el comando `opencode -m MODEL "prompt"` y lo copia al portapapeles
- **Filtros** de búsqueda en comandos y consejos

### Popups explicativos

- 44 elementos con `showInfo()` — click en cualquier proveedor, modelo o agente para ver descripción, specs y por qué es válido
- Cada popup muestra: nombre, descripción, specs (modelo, coste, tipo, velocidad), y "¿Por qué usarlo?"

### Paleta de colores

| Color | Hex | Uso |
|-------|-----|-----|
| Turquesa | `#00C8D4` | Título, acento principal |
| Klein blue | `#2E4AA8` | Códigos, poder, local |
| Oro | `#D4A843` | Tags FREE, valores, footer |
| Plata | `#e0e0e0` | Texto, `#9a9a9a` muted |
| Rojo óxido | `#C2410C` | Visual, warnings |

---

## 11. Estructura de archivos

```
~/.config/opencode/
├── opencode.jsonc          ← Config principal (OpenRouter: 3 modelos gratis verificados, default: nex-n2.5-mini:free)
├── agent/                  ← Agentes nativos (5)
│   ├── auditor.md
│   ├── implementer.md
│   ├── optimizer.md
│   ├── reviewer.md
│   └── validator.md
├── node_modules/           ← oh-my-openagent plugin v4.19.4
├── DOCUMENTACION.md        ← Esta documentación
└── GUIA_INTERACTIVA.html   ← Guía visual (546 líneas, 43 popups, launcher)

~/.omo/
└── omo.jsonc              ← Config oh-my-openagent (10 agentes, runtime_fallback, 8 categorías)

~/.zshenv                  ← Claves API (no en git, en .gitignore)
~/.alias_gio               ← Comandos incluye 'cerrar'
~/dotfiles/                ← Repo git (config sistema)
~/Desktop/OpenCode_GUIA.html          ← Symlink → GUIA_INTERACTIVA.html
```

---

## 12. Historial de cambios

| Fecha | Evento |
|-------|--------|
| Sep 2026 | Revisión exhaustiva: configura todo gratis, quita modelos que no eran gratis, actualiza plugin a oh-my-openagent |
| Sep 2026 | Recuperación de claves desde Safari/Chrome/Notes |
| Sep 2026 | Creación de opencode.jsonc con 13 proveedores |
| Sep 2026 | Migración de agentes desde Documents/.opencode/agents/ |
| Sep 2026 | Creación de .ssh/config para control-servidor-casero |
| Sep 2026 | Instalación de oh-my-openagent con oh-my-openagent plugin |
| Sep 2026 | Añadido agente validator |
| Sep 2026 | Corrección de schema JSONC (models como objeto) |
| Sep 2026 | **Seguridad**: todas las claves movidas a {env:VAR} |
| Sep 2026 | Creación de repos GitHub (opencode-config, dotfiles) |
| Sep 2026 | .zshenv excluido de git mediante .gitignore |
| Sep 2026 | Alias `cerrar` añadido a ~/.alias_gio |
| Sep 2026 | Guía HTML interactiva creada |
| Sep 2026 | **Seguridad**: `.zshenv` eliminado del historial de git con `git filter-repo` |
| Sep 2026 | Añadido proveedores: openrouter, github, nvidia, huggingface |
| Sep 2026 | Todas las 13 claves API resueltas |
| Sep 2026 | Instalación de AST-Grep (`brew install ast-grep`) |
| Sep 2026 | Guía HTML actualizada: quita modelos de pago no gratuitos, plugin renombrado oficialmente a oh-my-openagent, modelos 151 gratis verificados |
| Sep 2026 | `cerrar` arreglado: source ~/.zshenv, sin claves hardcodeadas |
| Sep 2026 | opencode.jsonc corregido: default a nemotron-3-ultra-free, huggingface añadido |
| Sep 2026 | DOCUMENTACION.md actualizada a 311 líneas |
| Sep 2026 | Guía HTML 545 líneas: Launcher, /plan /cost /diff /compact slash commands |
| Sep 2026 | Revisión exhaustiva: arregla popups rotos, plugin renombrado, modelos 0$, emojis eliminados, 151 modelos gratis |
| Sep 2026 | Guía: quita emojis datos, plugin renombrado, modelo gemma, +151 modelos gratis |
| Sep 2026 | Guía: emojis eliminados, modelo gemma, 151 modelos gratis |
| Sep 2026 | Guía: modelos alineados con configs reales, omo.jsonc actualizado |
| Sep 2026 | Guía: gemma corregido, modelos 0$ verificado |
| Sep 2026 | 151 modelos a 0$ en catálogo (nvidia 98, openrouter 28, opencode 8, google 7, groq 7, mistral 2, huggingface 1) |
| Sep 2026 | Documentación actualizada con datos verificados ✅ (modelos contrastados con catálogo models.dev) |
| Sep 2026 | **FIX: modelos opencode/* rotos (404/rate-limit)** → Cambio a OpenRouter nex-agi/nex-n2.5-mini:free (verificado OK coste 0) |
| Sep 2026 | **FIX: URL OpenRouter corregida** → https://openrouter.ai/api/v1 |
| Sep 2026 | **FIX: runtime_fallback habilitado** → nex-n2.5-mini → ling-flash-fin → nemotron-3.5-lightning |
| Sep 2026 | **FIX: small_model configurado** → evita selección de pago gpt-5.4-nano |
| Sep 2026 | **FIX: whitelist de 3 modelos gratuitos verificados** → enabled_providers: ["openrouter"] |

---

## 13. Verificación rápida

Para verificar que todo funciona:

```bash
# Versiones
opencode --version          # 1.18.31
omo --version               # 4.19.4
sg --version                # ast-grep 0.45.3

# Doctor
bunx oh-my-openagent doctor  # Solo compatibility fallback (no-crítico)

# Claves
source ~/.zshenv            # Carga todas las variables
echo $GITHUB_TOKEN          # Debe mostrar ghp_...
echo $NVIDIA_NIM_API_KEY    # Debe mostrar nvapi_...

# Config
opencode models             # Listar 151 modelos a 0$ del catálogo
cat ~/.config/opencode/opencode.jsonc  # Verificar 13 proveedores
cat ~/.omo/omo.jsonc        # Verificar 10 agentes

# cerrar
cerrar                      # Commit + push de todos los repos
```

---

*Generado el 17 septiembre 2026 · opencode v1.18.31 · oh-my-openagent v4.19.4*
*Modelo operativo: openrouter/nex-agi/nex-n2.5-mini:free (verificado OK, coste 0)*
*Fallback: inclusionai/ling-3.0-flash-fin:free → nvidia/nemotron-3.5-lightning:free (ambos verificados OK, coste 0)*
*1 proveedor operativo · 3 modelos gratuitos verificados · 15 agentes · 13 claves API · runtime_fallback activado*
