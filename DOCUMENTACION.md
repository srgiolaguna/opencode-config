# OpenCode — Configuración Completa

> Última actualización: 15 septiembre 2026  
> Sistema: macOS (M3 MacBook Air) · Usuario: `giolaguna`  
> Instalación: Homebrew · opencode v1.18.31  
> Usuario GitHub: `srgiolaguna`

---

## 1. Resumen

OpenCode está configurado con **12 proveedores** de modelos AI gratuitos, **5 agentes nativos**, y **oh-my-opencode** como sistema de orquestación con fallback automático entre modelos. Todo es **100% gratuito**. No se paga por nada.

**Seguridad**: Ningún API key está en los repositorios de GitHub. Todas las claves están en `~/.zshenv` (no rastreado por git).

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
| `~/.config/opencode/GUIA_INTERACTIVA.html` | Guía visual interactiva | ❌ No |

---

## 3. Proveedores de API

Todas las claves se leen desde `~/.zshenv` mediante `{env:VARIABLE}`.

### ✅ Funcionando (con clave válida en ~/.zshenv)

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
| **ollama** | Ninguna (local) | **100% LOCAL** |

### ⏳ Pendientes (necesitan clave)

| Proveedor | Variable de entorno | Tier | Estado |
|-----------|---------------------|------|--------|
| **openrouter** | `OPENROUTER_API_KEY` | **FREE** | ❌ Clave no encontrada |
| **github** | `GITHUB_TOKEN` | **FREE** | ❌ Token no encontrado |
| **nvidia** | `NVIDIA_NIM_API_KEY` | **FREE** | ❌ Clave no encontrada |
| **huggingface** | `HUGGINGFACE_TOKEN` | **FREE** | ❌ Clave no encontrada |

---

## 4. Modelos gratuitos

### Modelos OpenCode (propios, siempre FREE)

| Modelo | Uso | Velocidad |
|--------|-----|-----------|
| `opencode/ling-3.0-flash-fin-free` | Default, validación | ⚡ Ultra-rápido |
| `opencode/mimo-v2.5-free` | Análisis, optimización | 🚀 Rápido |
| `opencode/gemini-3.6-flash (opencode/nemotron-3.5-lightning-free → optimizado)` | Ejecución de cambios | 🚀 Rápido |
| `opencode/nemotron-3-ultra-free` | Validación final | 🐢 Potente |

### Modelos de Proveedores Externos (FREE tier)

| Proveedor | Modelo | Caso de uso |
|-----------|--------|-------------|
| `google/gemini-3.6-flash` | razonamiento, planificación, debugging |
| `groq/llama-3.1-70b` | inferencia rápida, gran contexto |
| `cerebras/llama-3.3-70b` | generación rápida, gran contexto |
| `mistral/mistral-small` | tareas multilingües |
| `sambanova/llama-3.1` | inferencia rápida |
| `cloudflare/llama-3.1` | Workers AI |
| `ai-gateway/mixtral` | mixture-of-experts |
| `ollama/qwen2.5-coder:3b` | coding local |
| `ollama/llama3` | uso general local |

---

## 5. Agentes

### Agentes Nativos (opencode.jsonc)

| Agente | Modelo | Permiso | Descripción |
|--------|--------|---------|-------------|
| `auditor` | `mimo-v2.5-free` | edit: deny | Analiza sin modificar |
| `implementer` | `nemotron-3.5-lightning-free` | — | Ejecuta cambios |
| `optimizer` | `mimo-v2.5-free` | edit: deny | Propone sin aplicar |
| `reviewer` | `nemotron-3-ultra-free` | edit: deny | Valida cambios |
| `validator` | `ling-3.0-flash-fin-free` | edit: deny | Lint, syntax-check |

### Agentes oh-my-opencode (~/.omo/omo.jsonc)

| Agente | Modelo principal | Fallback |
|--------|-----------------|----------|
| `sisyphus-junior` | `nemotron-3-ultra-free` | `gemini-3.6-flash` → `nemotron-3.5` → `mimo-v2.5` |
| `oracle` | `gemini-3.6-flash` | `nemotron-3-ultra-free` → `mimo-v2.5` |
| `librarian` | `gemini-3.6-flash` | `nemotron-3-ultra-free` → `nemotron-3.5` |
| `explore` | `ling-3.0-flash-fin-free` | `mimo-v2.5-free` |
| `prometheus` | `gemini-3.6-flash` | `nemotron-3-ultra-free` → `nemotron-3.5` |
| `metis` | `mimo-v2.5-free` | `ling-3.0-flash-fin-free` → `gemini-3.6-flash` |
| `momus` | `mimo-v2.5-free` | `ling-3.0-flash-fin-free` |
| `hephaestus` | `nemotron-3.5-lightning-free` | `nemotron-3-ultra-free` → `gemini-3.6-flash` |
| `atlas` | `nemotron-3.5-lightning-free` | `nemotron-3-ultra-free` → `gemini-3.6-flash` |
| `multimodal-looker` | `gemini-3.6-flash` | `nemotron-3.5` → `nemotron-3-ultra-free` |

---

## 6. oh-my-opencode

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
- **Cadena global**: `nemotron-3-ultra-free` → `gemini-3.6-flash` → `nemotron-3.5` → `mimo-v2.5` → `ling-3.0-flash-fin-free`

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

### oh-my-opencode
| Comando | Descripción |
|---------|-------------|
| `omo` | Iniciar oh-my-opencode |
| `omo --version` | Ver versión (4.19.4) |

### cerrar (alias en ~/.alias_gio)
| Qué hace | Detalle |
|----------|---------|
| **Auto-commit** | En todos los repos git del sistema |
| **Auto-push** | A GitHub donde hay remoto configurado |
| **Verificación** | Comprueba procesos NEXº (3 procesos) |
| **Recordatorio** | Muestra las 4 claves API pendientes |

**Repos que `cerrar` procesa** (5 repositorios):
| Repo | Remoto | URL |
|------|--------|-----|
| `~/.config/opencode/` | `srgiolaguna/opencode-config` | `github.com/srgiolaguna/opencode-config` |
| `~/dotfiles/` | `srgiolaguna/dotfiles` | `github.com/srgiolaguna/dotfiles` |
| `~/Documents/Cycle/` | `srgiolaguna/cycle` | `github.com/srgiolaguna/cycle` |
| `~/Documents/NEXº/` | `srgiolaguna/nexo` | `github.com/srgiolaguna/nexo` |
| `~/Documents/Workstation/` | `srgiolaguna/workstation` | `github.com/srgiolaguna/workstation` |

**Excluidos** de cerrar: `node_modules`, `.cache`, `.codex`, `.openclaw`, `~/.git` (worktree).

---

## 8. Repositorios GitHub

| Repo | URL | Contiene |
|------|-----|----------|
| `opencode-config` | `github.com/srgiolaguna/opencode-config` | opencode.jsonc, agentes, DOCUMENTACION.md, GUIA_INTERACTIVA.html |
| `dotfiles` | `github.com/srgiolaguna/dotfiles` | omo.jsonc, .gitignore, README.md |

> **Seguridad**: `~/.zshenv` está en `.gitignore`. Ninguna clave API en GitHub.

---

## 9. Pendientes

### ❌ 4 claves API faltantes
| Proveedor | Clave | Dónde buscar |
|-----------|-------|--------------|
| OpenRouter | `sk-or-v1-...` | Gmail, Google Docs |
| GitHub | `ghp_...` | GitHub Settings → Tokens |
| NVIDIA NIM | `nvapi-...` | NVIDIA API Console |

---

## 10. Estructura de archivos

```
~/.config/opencode/
├── opencode.jsonc          ← Config principal
├── agent/                  ← Agentes nativos (5)
│   ├── auditor.md
│   ├── implementer.md
│   ├── optimizer.md
│   ├── reviewer.md
│   └── validator.md
├── node_modules/           ← oh-my-openagent plugin
├── DOCUMENTACION.md        ← Esta documentación
└── GUIA_INTERACTIVA.html   ← Guía visual interactiva

~/.omo/
└── omo.jsonc              ← Config oh-my-opencode

~/.zshenv                  ← Claves API (no en git)
~/.alias_gio               ← Comandos incluye 'cerrar'
~/dotfiles/                ← Repo git (config sistema)
~/Desktop/OpenCode_Config_Resumen.md  ← Symlink → DOCUMENTACION.md
~/Desktop/OpenCode_GUIA.html          ← Symlink → GUIA_INTERACTIVA.html
```

---

## 11. Historial de cambios

| Fecha | Evento |
|-------|--------|
| Sep 2026 | Reinstalación de opencode, pérdida de configuración |
| Sep 2026 | Recuperación de claves desde Safari/Chrome/Notes |
| Sep 2026 | Creación de opencode.jsonc con 12 proveedores |
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
| Sep 2026 | Documentación actualizada y verificada |

---

*Generado el 15 septiembre 2026 · opencode v1.18.31 · oh-my-opencode v4.19.4*  
*Documentación fiel a la realidad y actualizada ✅*
