# OpenCode — Configuración Completa

> Última actualización: 16 septiembre 2026
> Sistema: macOS (M3 MacBook Air) · Usuario: `giolaguna`
> Instalación: Homebrew · opencode v1.18.31
> Usuario GitHub: `srgiolaguna`

---

## 1. Resumen

OpenCode está configurado con **13 proveedores** de modelos AI gratuitos, **5 agentes nativos**, y **oh-my-opencode** como sistema de orquestación con fallback automático entre modelos. Todo es **100% gratuito**. No se paga por nada.

**Seguridad**: Ningún API key está en los repositorios de GitHub. Todas las claves están en `~/.zshenv` (no rastreado por git). Todas las claves han sido resueltas.

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

Todas las claves se leen desde `~/.zshenv` mediante `{env:VARIABLE}`. **Todas las 13 claves configuradas y activas.**

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
| **openrouter** | `OPENROUTER_API_KEY` | **FREE** |
| **github** | `GITHUB_TOKEN` | **FREE** |
| **nvidia** | `NVIDIA_NIM_API_KEY` | **FREE** |
| **huggingface** | `HUGGINGFACE_TOKEN` | **FREE** |
| **ollama** | Ninguna (local) | **100% LOCAL** |

---

## 4. Modelos gratuitos

### Modelos OpenCode (propios, siempre FREE)

| Modelo | Uso | Velocidad |
|--------|-----|-----------|
| `opencode/gpt-5-nano` | Default, validación rápida | ⚡ Ultra-rápido |
| `opencode/mimo-v2.5-free` | Análisis, optimización | 🚀 Rápido |
| `opencode/nemotron-3.5-lightning-free` | Ejecución de cambios | 🚀 Rápido |
| `opencode/nemotron-3-ultra-free` | Validación final, tareas pesadas | 🐢 Potente |
| `opencode/ling-3.0-flash-fin-free` | Validación rápida | ⚡ Ultra-rápido |

| `opencode/ling-3.0-flash-fin-free` | Validación rápida, exploración | ⚡ Ultra-rápido | **FREE** |
| `opencode/mimo-v2.5-free` | Análisis, optimización, auditor, optimizer | 🚀 Rápido | **FREE** |
| `opencode/nemotron-3.5-lightning-free` | Ejecución de cambios, implementer, agentes omo | 🚀 Rápido | **FREE** |
| `opencode/nemotron-3-ultra-free` | Validación final, reviewer, tareas pesadas | 🐢 Potente | **FREE** |
| `opencode/muse-spark-1.2-contributor-free` | Contributor tier | ⚡ Gratuito | **FREE** |
| `opencode/muse-spark-1.3-contributor-free` | Contributor tier | ⚡ Gratuito | **FREE** |

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
| `implementer` | `gemini-3.6-flash` | — | Ejecuta cambios |
| `optimizer` | `mimo-v2.5-free` | edit: deny | Propone sin aplicar |
| `reviewer` | `nemotron-3-ultra-free` | edit: deny | Valida cambios |
| `validator` | `gpt-5-nano` | edit: deny | Lint, syntax-check |

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
- **Cadena global**: `nemotron-3-ultra-free` → `gemini-3.6-flash` → `nemotron-3.5` → `mimo-v2.5` → `ling-3.0-flash-fin-free` → `gpt-5-nano`

### AST-Grep

Herramienta de búsqueda de código por patrones AST. Habilita el skill `ast-grep` para buscar código por estructura.

Instalación: `brew install ast-grep`

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
| `omo doctor` | Diagnosticar configuración |

### cerrar (alias en ~/.alias_gio)

| Qué hace | Detalle |
|----------|---------|
| **Auto-commit** | En todos los repos git del sistema |
| **Auto-push** | A GitHub donde hay remoto configurado |
| **Verificación** | Comprueba procesos NEXº |
| **Recordatorio** | Muestra las claves API pendientes |

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

> **Seguridad**: `~/.zshenv` está en `.gitignore`. Ninguna clave API en GitHub. `.zshenv` eliminado del historial de git con `git filter-repo`.

---

## 9. Estado de Configuración

### ✅ Completo (13/13 claves configuradas)

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

## 10. Estructura de archivos

```
~/.config/opencode/
├── opencode.jsonc          ← Config principal (13 proveedores, 5 agentes)
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
└── omo.jsonc              ← Config oh-my-opencode (10 agentes, 8 categorías)

~/.zshenv                  ← Claves API (no en git, en .gitignore)
~/.alias_gio               ← Comandos incluye 'cerrar'
~/dotfiles/                ← Repo git (config sistema)
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
| Sep 2026 | **Seguridad**: `.zshenv` eliminado del historial de git con `git filter-repo` |
| Sep 2026 | Añadido proveedores: openrouter, github, nvidia, huggingface |
| Sep 2026 | Todas las 13 claves API resueltas |
| Sep 2026 | Instalación de AST-Grep (`brew install ast-grep`) |
| Sep 2026 | Documentación actualizada y verificada |

---

## 12. Verificación rápida

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

# cerrar
cerrar                      # Commit + push de todos los repos
```

---

*Generado el 16 septiembre 2026 · opencode v1.18.31 · oh-my-opencode v4.19.4*
*Documentación fiel a la realidad y actualizada ✅*
*13 proveedores · 13 claves API · 0 pendientes*
