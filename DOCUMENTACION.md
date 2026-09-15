# OpenCode — Configuración Completa

> Última actualización: 15 septiembre 2026  
> Sistema: macOS (M3 MacBook Air) · Usuario: `giolaguna`  
> Instalación: Homebrew · opencode v1.18.31  
> Usuario GitHub: `srgiolaguna`  

---

## 1. Resumen

OpenCode está configurado con **12 proveedores** de modelos AI gratuitos, **5 agentes nativos**, y **oh-my-opencode** como sistema de orquestación con fallback automático entre modelos. Todo es **100% gratuito**. No se paga por nada.

**Seguridad**: Ningún API key está en los repositorios de GitHub. Todas las claves están en `~/.zshenv` (no rastreado por git). Los repositorios en GitHub contienen solo la configuración sin secrets.

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
| `~/Desktop/OpenCode_Config_Resumen.md` | Symlink → doc en config | ❌ No |

---

## 3. Proveedores de API

Todas las claves se leen desde `~/.zshenv` mediante `{env:VARIABLE}`. Ningún API key está en el código.

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

> Las 3 claves pendientes no se encontraron en ningún archivo del sistema. El historial de navegación confirma que se usaban, pero nunca se guardaron. Añádelas a `~/.zshenv` cuando las tengas.

---

## 4. Modelos gratuitos disponibles

### Modelos opencode (propios, siempre FREE)

| Modelo | Uso recomendado | Velocidad |
|--------|----------------|-----------|
| `opencode/ling-3.0-flash-fin-free` | Validación rápida, tareas ligeras, default | ⚡ Ultra-rápido |
| `opencode/mimo-v2.5-free` | Análisis, optimización, tareas medias | 🚀 Rápido |
| `opencode/nemotron-3.5-lightning-free` | Ejecución de cambios, tareas complejas | 🚀 Rápido |
| `opencode/nemotron-3-ultra-free` | Validación final, tareas pesadas | 🐢 Más lento pero potente |

### Modelos de proveedores externos (FREE tier)

| Proveedor | Modelo | Caso de uso |
|-----------|--------|-------------|
| `google/gemini-3.6-flash` | razonamiento, planificación, debugging |
| `groq/llama-3.1-70b` | inferencia rápida, gran contexto |
| `cerebras/llama-3.3-70b` | generación rápida, gran contexto |
| `mistral/mistral-small` | tareas multilingües |
| `sambanova/llama-3.1` | inferencia rápida |
| `cloudflare/llama-3.1` | Workers AI, tareas rápidas |
| `ai-gateway/mixtral` | mixture-of-experts, tareas complejas |
| `ollama_chat/qwen2.5-coder:3b` | coding local, sin conexión |
| `ollama_chat/llama3` | uso general local |

---

## 5. Agentes configurados

### Agentes nativos (en `opencode.jsonc`)

| Agente | Modelo | Permisos | Descripción |
|--------|--------|----------|-------------|
| `auditor` | `mimo-v2.5-free` | `edit: deny` | Analiza proyectos, detecta problemas. **No modifica archivos.** |
| `implementer` | `nemotron-3.5-lightning-free` | — | Ejecuta cambios aprobados. |
| `optimizer` | `mimo-v2.5-free` | `edit: deny` | Propone optimizaciones **sin aplicarlas.** |
| `reviewer` | `nemotron-3-ultra-free` | `edit: deny` | Valida cambios y detecta errores y regresiones. |
| `validator` | `ling-3.0-flash-fin-free` | `edit: deny` | Validación rápida: lint, syntax-check, compilación. Ultra-rápido. |

### Agentes oh-my-opencode (en `~/.omo/omo.jsonc`)

| Agente | Modelo principal | Fallback chain |
|--------|-----------------|----------------|
| `sisyphus-junior` | `nemotron-3-ultra-free` | `gemini-3.6-flash` → `nemotron-3.5-lightning-free` → `mimo-v2.5-free` |
| `oracle` | `gemini-3.6-flash` | `nemotron-3-ultra-free` → `mimo-v2.5-free` |
| `librarian` | `gemini-3.6-flash` | `nemotron-3-ultra-free` → `nemotron-3.5-lightning-free` |
| `explore` | `ling-3.0-flash-fin-free` | `mimo-v2.5-free` |
| `multimodal-looker` | `gemini-3.6-flash` | `nemotron-3.5-lightning-free` → `nemotron-3-ultra-free` |
| `prometheus` | `gemini-3.6-flash` | `nemotron-3-ultra-free` → `nemotron-3.5-lightning-free` |
| `metis` | `mimo-v2.5-free` | `ling-3.0-flash-fin-free` → `gemini-3.6-flash` |
| `momus` | `mimo-v2.5-free` | `ling-3.0-flash-fin-free` |
| `atlas` | `nemotron-3.5-lightning-free` | `nemotron-3-ultra-free` → `gemini-3.6-flash` |
| `hephaestus` | `nemotron-3.5-lightning-free` | `nemotron-3-ultra-free` → `gemini-3.6-flash` |

### Categorías oh-my-opencode

| Categoría | Modelo | Fallback |
|-----------|--------|----------|
| `quick` | `ling-3.0-flash-fin-free` | — |
| `deep` | `nemotron-3-ultra-free` | `gemini-3.6-flash` → `nemotron-3.5-lightning-free` |
| `visual-engineering` | `gemini-3.6-flash` | `nemotron-3-ultra-free` |
| `ultrabrain` | `nemotron-3-ultra-free` | `gemini-3.6-flash` |
| `writing` | `mimo-v2.5-free` | — |
| `artistry` | `nemotron-3.5-lightning-free` | — |
| `unspecified-low` | `ling-3.0-flash-fin-free` | — |
| `unspecified-high` | `nemotron-3-ultra-free` | `gemini-3.6-flash` → `nemotron-3.5-lightning-free` |

### Cadena de fallback global

```
nemotron-3-ultra-free → gemini-3.6-flash → nemotron-3.5-lightning-free → mimo-v2.5-free → ling-3.0-flash-fin-free
```

Si un modelo falla (rate limit, error 500, timeout), el sistema automáticamente prueba el siguiente de la cadena **sin perder contexto**.

---

## 6. oh-my-opencode

### Instalación (ya realizada)

```bash
brew install bun                                          # Runtime JavaScript (MIT, gratis)
bun add -g oh-my-opencode                                # Instalación global
bunx oh-my-opencode install --no-tui \
  --platform opencode \
  --claude no --openai no --gemini no \
  --copilot no --opencode-zen no \
  --zai-coding-plan no --kimi-for-coding no \
  --opencode-go no                                       # Todo = gratuito
```

### Cómo funciona

oh-my-opencode es un **plugin de orquestación** que añade:

1. **Enrutamiento automático**: decide qué modelo usar según el tipo de tarea
2. **Fallback automático**: si un modelo falla, prueba el siguiente sin reiniciar
3. **Cadenas por agente**: cada agente tiene su propia lista de fallback
4. **Fallback global**: cadena de respaldo para todos los agentes

### Archivos clave

| Archivo | Propósito |
|---------|-----------|
| `~/.omo/omo.jsonc` | Configuración principal |
| `~/.config/opencode/node_modules/oh-my-openagent/` | Plugin instalado |
| `~/.bun/bin/omo` | Commando `omo` (wrapper via bun) |

---

## 7. Comandos útiles

### opencode

| Comando | Descripción |
|---------|-------------|
| `opencode` | Iniciar opencode |
| `opencode models` | Listar modelos disponibles |
| `opencode -m <model>` | Usar un modelo específico |
| `opencode doctor` | Verificar configuración |

### oh-my-opencode

| Comando | Descripción |
|---------|-------------|
| `omo` | Iniciar oh-my-opencode |
| `omo --version` | Ver versión (4.19.4) |

### Comando `cerrar` (en ~/.alias_gio)

**Uso**: escribir `cerrar` en la terminal antes de cerrar sesión.

**Qué hace**:
1. Commita cambios en `~/.config/opencode/` y `~/dotfiles/`
2. Verifica que los procesos de NEXº estén vivos (3 procesos)
3. Recuerda las 3 claves API pendientes
4. Hace `git push` a GitHub
5. Muestra resumen de estado

---

## 8. Repositorios GitHub

| Repo | URL | Contiene |
|------|-----|----------|
| `opencode-config` | `github.com/srgiolaguna/opencode-config` | opencode.jsonc, agentes, DOCUMENTACION.md |
| `dotfiles` | `github.com/srgiolaguna/dotfiles` | omo.jsonc, .gitignore, README.md |

> **Nota de seguridad**: `zshenv` está excluido de `.gitignore` en dotfiles. Ninguna clave API está en GitHub.

---

## 9. Problemas conocidos y pendientes

### ❌ 3 claves API pendientes

| Proveedor | Clave | Dónde buscar |
|-----------|-------|--------------|
| OpenRouter | `sk-or-v1-...` | Gmail, Google Docs, notas, navegador |
| GitHub | `ghp_...` | GitHub settings → Tokens |
| NVIDIA NIM | `nvapi-...` | NVIDIA API console |

### ⚠️ `omo` command wrapper

El comando `omo` funciona vía `~/.bun/bin/omo` que usa bun como runtime. No requiere `node` instalado.

---

## 10. Cómo restaurar la configuración tras reinstalar

```bash
# 1. Instalar Homebrew (si no existe)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 2. Instalar opencode
brew install opencode

# 3. Instalar bun
brew install bun

# 4. Clonar repos
git clone https://github.com/srgiolaguna/opencode-config.git ~/.config/opencode
git clone https://github.com/srgiolaguna/dotfiles.git ~/dotfiles

# 5. Restaurar ~/.zshenv (desde backup o recrear)
# Copiar las claves API a ~/.zshenv

# 6. Instalar oh-my-opencode
bun add -g oh-my-opencode
bunx oh-my-opencode install --no-tui \
  --platform opencode \
  --claude no --openai no --gemini no \
  --copilot no --opencode-zen no \
  --zai-coding-plan no --kimi-for-coding no \
  --opencode-go no

# 7. Verificar
opencode models
```

---

## 11. Historial de cambios

| Fecha | Evento |
|-------|--------|
| Sep 2026 | Reinstalación de opencode, pérdida de configuración |
| Sep 2026 | Recuperación de claves desde Safari History, Chrome, Notes |
| Sep 2026 | Creación de `opencode.jsonc` con 12 proveedores |
| Sep 2026 | Migración de agentes desde `Documents/.opencode/agents/` |
| Sep 2026 | Creación de `.ssh/config` para `control-servidor-casero` |
| Sep 2026 | Instalación de `oh-my-opencode` con `oh-my-openagent` plugin |
| Sep 2026 | Añadido agente `validator` |
| Sep 2026 | Corrección de schema JSONC (models debe ser objeto, no array) |
| Sep 2026 | **Seguridad**: todas las claves movidas de opencode.jsonc a `{env:VAR}` |
| Sep 2026 | Creación de repos GitHub (`srgiolaguna/opencode-config`, `srgiolaguna/dotfiles`) |
| Sep 2026 | `.zshenv` excluido de git mediante `.gitignore` |
| Sep 2026 | Alias `cerrar` añadido a `~/.alias_gio` |
| Sep 2026 | Documentación completa generada |

---

*Generado el 15 septiembre 2026 · opencode v1.18.31 · oh-my-opencode v4.19.4*
