# OpenCode — Configuración Completa

> Última actualización: 16 septiembre 2026
> Sistema: macOS (M3 MacBook Air) · Usuario: `giolaguna`
> Instalación: Homebrew · opencode v1.18.31
> Usuario GitHub: `srgiolaguna`

---

## 1. Resumen

OpenCode está configurado con **13 proveedores** de modelos AI gratuitos, **34 modelos** disponibles, **5 agentes nativos** y **10 agentes oh-my-opencode**, con fallback automático y Ultrawork mode. Todo es **100% gratuito**. No se paga por nada.

**Seguridad**: Ningún API key está en los repositorios de GitHub. Todas las claves están en `~/.zshenv` (no rastreado por git). El historial de git fue limpiado con `git filter-repo`.

**Guía visual**: `GUIA_INTERACTIVA.html` — 534 líneas con sistema de popups explicativos (44), launcher interactivo (14 modelos con prompt + modo normal/ulw/plan), y 28 comandos copiables. Hacer click en cualquier elemento muestra detalles del proveedor, modelo o agente.

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
| `~/.omo/omo.jsonc` | Config oh-my-opencode | ✅ Sí |
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

## 4. Modelos gratuitos

### 34 modelos FREE en 12 proveedores

### Modelos OpenCode (propios, siempre FREE)

| Modelo | Uso | Velocidad |
|--------|-----|-----------|
| `opencode/nemotron-3-ultra-free` | **Default**, validación final, tareas pesadas | 🧠 Potente 70B+ |
| `opencode/ling-3.0-flash-fin-free` | Validación rápida, exploración | ⚡ Ultra-rápido |
| `opencode/mimo-v2.5-free` | Análisis, optimización, auditor | 🚀 Rápido |
| `opencode/nemotron-3.5-lightning-free` | Ejecución de cambios, implementer, agentes | ⚡⚡ Lightning |
| `opencode/deepseek-v4-pro` | Coding avanzado | 💻 Pro |
| `opencode/muse-spark-1.3-contributor-free` | Contributor tier | ⚡ Contributor |

### Modelos de Proveedores Externos (FREE tier)

| Proveedor | Modelo | Caso de uso |
|-----------|--------|-------------|
| `google/gemini-3.6-flash` | Razonamiento, planificación, visual | Multimodal |
| `google/gemini-3-flash` | Razonamiento rápido | General |
| `groq/llama-3.3-70b-versatile` | Inferencia rápida, contexto grande | 70B |
| `groq/llama-3.1-8b-instant` | Ultra-instant | Rápido |
| `groq/openai/gpt-oss-120b` | Coding avanzado | 120B |
| `cerebras/llama-3.3-70b` | Generación rápida | Contexto |
| `mistral/mistral-small` | Multilingüe | General |
| `sambanova/llama-3.1` | Inferencia rápida | General |
| `cloudflare/llama-3.1` | Workers AI | Escalable |
| `ai-gateway/mixtral` | MoE | Diversidad |
| `openrouter/nvidia/nemotron-3-ultra-550b-a55b` | Ultra-potente 55B | Pesado |
| `huggingface/Qwen/Qwen2.5-Coder-32B-Instruct` | Coding avanzado | 32B |
| `huggingface/google/gemma-4-31b-it` | Multimodal | 31B visual |
| `openrouter/perplexity/sonar-pro-search` | Investigación web | Search |
| `vercel/fish-audio/s1-free` | Transcripción de voz | Audio |

---

## 5. Agentes

### Agentes Nativos (opencode.jsonc) — 5 agentes

| Agente | Modelo | Permiso | Descripción |
|--------|--------|---------|-------------|
| `auditor` | `mimo-v2.5-free` | edit: deny | Analiza sin modificar |
| `implementer` | `nemotron-3.5-lightning-free` | — | Ejecuta cambios |
| `optimizer` | `mimo-v2.5-free` | edit: deny | Propone sin aplicar |
| `reviewer` | `nemotron-3-ultra-free` | edit: deny | Valida cambios |
| `validator` | `nemotron-3-ultra-free` | edit: deny | Lint, syntax-check |

### Agentes oh-my-opencode (~/.omo/omo.jsonc) — 10 agentes

| Agente | Modelo principal | Categoría |
|--------|-----------------|-----------|
| `hephaestus` | `nemotron-3.5-lightning-free` | construcción |
| `oracle` | `gemini-3.6-flash` | investigación |
| `librarian` | `gemini-3.6-flash` | documentación |
| `explore` | `ling-3.0-flash-fin-free` | exploración |
| `multimodal-looker` | `gemini-3.6-flash` | visual |
| `prometheus` | `gemini-3.6-flash` | sistemas |
| `metis` | `mimo-v2.5-free` | optimización |
| `momus` | `mimo-v2.5-free` | general |
| `atlas` | `nemotron-3.5-lightning-free` | arquitectura |
| `sisyphus-junior` | `nemotron-3-ultra-free` | persistencia |

---

## 6. oh-my-opencode v4.19.4

### Instalación

```bash
brew install bun
bun add -g oh-my-opencode
bunx oh-my-opencode install --no-tui \
  --platform opencode \
  --claude no --openai no --gemini no \
  --copilot no --opencode-zen no \
  --zai-coding-plan no --kimi-for-coding no \
  --opencode-go no
```

### Funciones

- **Enrutamiento automático** de modelos según el tipo de tarea
- **Fallback automático** sin perder contexto
- **Cadenas de fallback por agente**
- **8 categorías**: quick, deep, visual-engineering, ultrabrain, writing, artistry, exploration, construction
- **Ultrawork (ulw)** — modo autónomo. Escribe `ulw` en el prompt y el agente trabaja sin confirmar cada paso. **GRATIS.**
- **Modo Plan** — `--plan` o `/plan`. Primero planifica, luego ejecuta. **GRATIS.**
- **MCP Servers** — websearch (Exa), context7 (docs), grep_app (GitHub), LSP, 25+ hooks configurables. **GRATIS.**
- **Model routing inteligente** — usa modelos diferentes para cada tipo de tarea para ahorrar tokens. **GRATIS.**
- **AST-Grep**: `brew install ast-grep` (v0.45.3) — búsqueda por estructura AST

### Cadena de Fallback Global

```
nemotron-3-ultra-free → gemini-3.6-flash → nemotron-3.5-lightning-free → mimo-v2.5-free → ling-3.0-flash-fin-free
```

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
| `/model opencode/nemotron-3-ultra-free` | Cambiar modelo en sesión |
| `/plan` | Modo planificación |
| `/cost` | Mostrar tokens y coste |
| `/diff` | Mostrar cambios pendientes |
| `/compact` | Comprimir historial |
| `/undo` | Deshacer último cambio |
| `/review` | Revisar cambios |

### oh-my-opencode

| Comando | Descripción |
|---------|-------------|
| `omo` | Iniciar oh-my-opencode |
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

### ✅ Completo (13/13 claves configuradas, 0 pendientes)

Todas las claves API están configuradas en `~/.zshenv`:

| Variable | Estado |
|-----------|--------|
| `OPENCODE_SECRET_KEY` | ✅ |
| `GROQ_API_KEY` | ✅ |
| `CEREBRAS_API_KEY` | ✅ |
| `MISTRAL_API_KEY` | ✅ |
| `SAMBANOVA_API_KEY` | ✅ |
| `AI_GATEWAY_API_KEY` | ✅ |
| `GEMINI_API_KEY` | ✅ |
| `GOOGLE_API_KEY` | ✅ |
| `CLOUDFLARE_API_TOKEN` | ✅ |
| `OPENROUTER_API_KEY` | ✅ |
| `GITHUB_TOKEN` | ✅ |
| `NVIDIA_NIM_API_KEY` | ✅ |
| `HUGGINGFACE_TOKEN` | ✅ |

**0 claves pendientes.**

---

## 10. Guía Visual Interactiva

`GUIA_INTERACTIVA.html` — 534 líneas, paleta turquesa/klein blue/oro/plata/óxido sobre negro mate con fuente Urbanist.

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
├── opencode.jsonc          ← Config principal (13 proveedores, default: nemotron-3-ultra-free)
├── agent/                  ← Agentes nativos (5)
│   ├── auditor.md
│   ├── implementer.md
│   ├── optimizer.md
│   ├── reviewer.md
│   └── validator.md
├── node_modules/           ← oh-my-openagent plugin v4.19.4
├── DOCUMENTACION.md        ← Esta documentación
└── GUIA_INTERACTIVA.html   ← Guía visual (534 líneas, 45 popups, launcher)

~/.omo/
└── omo.jsonc              ← Config oh-my-opencode (10 agentes, 8 categorías)

~/.zshenv                  ← Claves API (no en git, en .gitignore)
~/.alias_gio               ← Comandos incluye 'cerrar'
~/dotfiles/                ← Repo git (config sistema)
~/Desktop/OpenCode_GUIA.html          ← Symlink → GUIA_INTERACTIVA.html
```

---

## 12. Historial de cambios

| Fecha | Evento |
|-------|--------|
| Sep 2026 | Reinstalación de opencode, pérdida de configuración |
| Sep 2026 | Recuperación de claves desde Safari/Chrome/Notes |
| Sep 2026 | Creación de opencode.jsonc con 13 proveedores |
| Sep 2026 | Migración de agentes desde Documents/.opencode/agents/ |
| Sep 2026 | Creación de .ssh/config para control-servidor-casero |
| Sep 2026 | Instalación de oh-my-opencode con oh-my-openagent plugin |
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
| Sep 2026 | Guía HTML actualizada: popups explicativos, paleta turquesa/klein/gold/plata/óxido |
| Sep 2026 | `cerrar` arreglado: source ~/.zshenv, sin claves hardcodeadas |
| Sep 2026 | opencode.jsonc corregido: default a nemotron-3-ultra-free, huggingface añadido |
| Sep 2026 | DOCUMENTACION.md actualizada a 311 líneas |
| Sep 2026 | Guía HTML 534 líneas: Launcher, /plan /cost /diff /compact slash commands |
| Sep 2026 | Documentación fiel a la realidad y actualizada ✅ |

---

## 13. Verificación rápida

Para verificar que todo funciona:

```bash
# Versiones
opencode --version          # 1.18.31
omo --version               # 4.19.4
sg --version                # ast-grep 0.45.3

# Doctor
bunx oh-my-opencode doctor  # Solo compatibility fallback (no-crítico)

# Claves
source ~/.zshenv            # Carga todas las variables
echo $GITHUB_TOKEN          # Debe mostrar ghp_...
echo $NVIDIA_NIM_API_KEY    # Debe mostrar nvapi_...

# Config
opencode models             # Listar 34 modelos
cat ~/.config/opencode/opencode.jsonc  # Verificar 13 proveedores
cat ~/.omo/omo.jsonc        # Verificar 10 agentes

# cerrar
cerrar                      # Commit + push de todos los repos
```

---

*Generado el 16 septiembre 2026 · opencode v1.18.31 · oh-my-opencode v4.19.4*
*Documentación fiel a la realidad y actualizada ✅*
*13 proveedores · 34 modelos · 15 agentes · 13 claves API · 0 pendientes*
