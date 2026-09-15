# OpenCode — Configuración Completa

> Última actualización: 15 septiembre 2026  
> Sistema: macOS (M3 MacBook Air) · Usuario: `giolaguna`  
> Instalación: Homebrew · opencode v1.18.31  

---

## Tabla de contenidos

1. [Resumen](#resumen)
2. [Archivos de configuración](#archivos-de-configuración)
3. [Proveedores de API](#proveedores-de-api)
4. [Modelos gratuitos disponibles](#modelos-gratuitos-disponibles)
5. [Agentes configurados](#agentes-configurados)
6. [oh-my-opencode](#oh-my-opencode)
7. [Comandos útiles](#comandos-útiles)
8. [Problemas conocidos y pendientes](#problemas-conocidos-y-pendientes)
9. [Cómo restaurar la configuración tras reinstalar](#cómo-restaurar-la-configuración-tras-reinstalar)

---

## 1. Resumen

OpenCode está configurado con **12 proveedores** de modelos AI gratuitos, **5 agentes nativos** y **oh-my-opencode** como sistema de orquestación con fallback automático entre modelos.

Todo es **100% gratuito**. No se paga por nada.

---

## 2. Archivos de configuración

| Archivo | Propósito | Contenido |
|---------|-----------|-----------|
| `~/.config/opencode/opencode.jsonc` | Config principal de opencode | 12 proveedores, 5 agentes, plugin oh-my-openagent |
| `~/.config/opencode/agent/auditor.md` | Agente auditor | Analiza sin modificar |
| `~/.config/opencode/agent/implementer.md` | Agente implementer | Ejecuta cambios |
| `~/.config/opencode/agent/optimizer.md` | Agente optimizer | Propone mejoras |
| `~/.config/opencode/agent/reviewer.md` | Agente reviewer | Valida cambios |
| `~/.config/opencode/agent/validator.md` | Agente validator | Lint/syntax-check rápido |
| `~/.zshenv` | Variables de entorno | Todas las claves API |
| `~/.omo/omo.jsonc` | Config oh-my-opencode | Enrutamiento automático + fallback |
| `~/.config/opencode/package.json` | Dependencias opencode | `@opencode-ai/plugin` v1.18.31 |
| `~/.config/opencode/node_modules/oh-my-openagent/` | Plugin oh-my-openagent | Instalado vía bun |

---

## 3. Proveedores de API

### ✅ Funcionando (con clave válida)

| Proveedor | Variable de entorno | Modelos disponibles | Tier |
|-----------|---------------------|---------------------|------|
| **opencode** | `OPENCODE_SECRET_KEY` | `ling-3.0-flash-fin-free`, `mimo-v2.5-free`, `nemotron-3.5-lightning-free`, `nemotron-3-ultra-free` | **FREE** |
| **google/gemini** | `GEMINI_API_KEY`, `GOOGLE_API_KEY` | `gemini-3.6-flash` | **FREE** |
| **groq** | `GROQ_API_KEY` | `llama-3.1-70b` | **FREE** |
| **cerebras** | `CEREBRAS_API_KEY` | `llama-3.3-70b` | **FREE** |
| **mistral** | `MISTRAL_API_KEY` | `mistral-small` | **FREE** |
| **sambanova** | `SAMBANOVA_API_KEY` | `llama-3.1` | **FREE** |
| **cloudflare** | `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID` | `llama-3.1` | **FREE** |
| **ai-gateway** | `AI_GATEWAY_API_KEY` | `mixtral` | **FREE** |
| **ollama** | Ninguna (local) | `qwen2.5-coder:3b`, `llama3` | **100% LOCAL** |

### ⏳ Pendientes (necesitan clave)

| Proveedor | Variable de entorno | Modelo | Tier | Estado |
|-----------|---------------------|--------|------|--------|
| **openrouter** | `OPENROUTER_API_KEY` | `openrouter/free` | **FREE** | ❌ Clave no encontrada |
| **github** | `GITHUB_TOKEN` | `github/gpt-4o` | **FREE** | ❌ Token no encontrado |
| **nvidia** | `NVIDIA_NIM_API_KEY` | `nvidia/nemotron-3-ultra` | **FREE** | ❌ Clave no encontrada |

> **Nota:** Las 3 claves pendientes no se encontraron en ningún archivo del sistema (Mac ni servidor SSH). El historial de navegación confirma que se usaban, pero nunca se guardaron como archivos locales.

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

| Agente | Modelo | Modo | Permisos | Descripción |
|--------|--------|------|----------|-------------|
| `auditor` | `mimo-v2.5-free` | all | `edit: deny` | Analiza proyectos, detecta problemas y riesgos. **No modifica archivos.** |
| `implementer` | `nemotron-3.5-lightning-free` | all | — | Ejecuta cambios aprobados respetando alcance y decisiones. |
| `optimizer` | `mimo-v2.5-free` | all | `edit: deny` | Propone optimizaciones y mejoras **sin aplicarlas.** |
| `reviewer` | `nemotron-3-ultra-free` | all | `edit: deny` | Valida cambios realizados y detecta errores y regresiones. |
| `validator` | `ling-3.0-flash-fin-free` | all | `edit: deny` | Validación rápida: lint, syntax-check, compilación. Ultra-rápido. |

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

---

## 6. oh-my-opencode

### Instalación

```bash
# Instalación completa (ya realizada)
brew install bun                              # Runtime JavaScript (MIT, gratis)
bun add -g oh-my-opencode                     # Instalación global
bunx oh-my-opencode install --no-tui \
  --platform opencode \
  --claude no --openai no --gemini no \
  --copilot no --opencode-zen no \
  --zai-coding-plan no --kimi-for-coding no \
  --opencode-go no                            # Todo = gratuito
```

### Cómo funciona

oh-my-opencode es un **plugin de orquestación** para OpenCode que añade:

1. **Enrutamiento automático de modelos**: decide qué modelo usar según el tipo de tarea
2. **Fallback automático**: si un modelo falla (rate limit, error 500, timeout), automáticamente prueba el siguiente de la cadena sin perder contexto
3. **Cadenas de fallback por agente**: cada agente tiene su propia lista de fallback
4. **Cadena de fallback global**: `nemotron-3-ultra-free` → `gemini-3.6-flash` → `nemotron-3.5-lightning-free` → `mimo-v2.5-free` → `ling-3.0-flash-fin-free`

### Archivos clave

| Archivo | Propósito |
|---------|-----------|
| `~/.omo/omo.jsonc` | Configuración principal (agentes, categorías, fallback) |
| `~/.config/opencode/node_modules/oh-my-openagent/` | Plugin instalado |

---

## 7. Comandos útiles

### opencode

| Comando | Descripción |
|---------|-------------|
| `opencode` | Iniciar opencode en el directorio actual |
| `opencode models` | Listar todos los modelos disponibles |
| `opencode -m <model>` | Usar un modelo específico (ej: `opencode -m gemini/gemini-3.6-flash`) |
| `opencode agent auditor` | Lanzar solo el agente auditor |
| `opencode agent validator` | Lanzar validación rápida |
| `opencode doctor` | Verificar configuración |

### oh-my-opencode

| Comando | Descripción |
|---------|-------------|
| `omo` | Iniciar oh-my-opencode |
| `omo --version` | Ver versión (4.19.4) |
| `omo run <mensaje>` | Ejecutar con modo todo-tareas |

### Alternancia de modelos durante sesión

Dentro de una sesión de opencode, puedes cambiar de modelo con:
```
/model gemini/gemini-3.6-flash
/model opencode/nemotron-3-ultra-free
```

---

## 8. Problemas conocidos y pendientes

### ❌ 3 claves API faltantes

| Proveedor | Clave | Dónde buscar |
|-----------|-------|--------------|
| OpenRouter | `sk-or-v1-...` | Gmail, Google Docs, notas, navegador |
| GitHub | `ghp_...` | GitHub settings → Tokens |
| NVIDIA NIM | `nvapi-...` | NVIDIA API console |

> Estas claves no se encontraron en ningún archivo del sistema. Si las recuerdas o las tienes en Gmail/Google Docs, agrégalas a `~/.zshenv`.

### ⚠️ `omo` command needs node

El comando `omo` requiere `node` que no está instalado (solo bun). Bun proporciona un runtime compatible. El comando funciona con `export PATH="$HOME/.bun/bin:$PATH"`.

### ⚠️ `.zshrc` no fuente `.zshenv` explícitamente

`.zshenv` se carga automáticamente en todas las invocaciones de zsh (interactivas o no). No necesita ser fuente explícita desde `.zshrc`. El PATH de bun se añade correctamente en `.zshenv`.

---

## 9. Cómo restaurar la configuración tras reinstalar

Si necesitas reinstalar todo desde cero:

```bash
# 1. Instalar homebrew (si no existe)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 2. Instalar opencode
brew install opencode

# 3. Instalar bun
brew install bun

# 4. Exportar claves API (desde ~/.zshenv)
source ~/.zshenv

# 5. Instalar oh-my-opencode
bun add -g oh-my-opencode
bunx oh-my-opencode install --no-tui \
  --platform opencode \
  --claude no --openai no --gemini no \
  --copilot no --opencode-zen no \
  --zai-coding-plan no --kimi-for-coding no \
  --opencode-go no

# 6. Añadir bun a PATH en ~/.zshenv
echo 'export PATH="$HOME/.bun/bin:$PATH"' >> ~/.zshenv

# 7. Crear agentes
mkdir -p ~/.config/opencode/agent
# (Copiar los .md de agentes)
```

---

## Estructura de la red de proveedores

```
                    ┌─────────────────────────────────────┐
                    │         opencode.jsonc               │
                    │  (config principal)                   │
                    └──────────────┬──────────────────────┘
                                   │
              ┌────────────────────┼────────────────────┐
              │                    │                    │
              ▼                    ▼                    ▼
     ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
     │  opencode    │   │  oh-my-open  │   │   agentes    │
     │  plugin      │   │  agent       │   │   nativos    │
     │  (fallback)  │   │  (orquest.)  │   │   (5)        │
     └──────┬───────┘   └──────┬───────┘   └──────┬───────┘
            │                  │                   │
            ▼                  ▼                   ▼
     ┌──────────────────────────────────────────────────────┐
     │              ~/.omo/omo.jsonc                         │
     │         (agentes + categorías + fallback)              │
     └──────────────────────────────────────────────────────┘
```

---

## Historial de cambios

| Fecha | Evento |
|-------|--------|
| Sep 2026 | Reinstalación de opencode, pérdida de configuración |
| Sep 2026 | Recuperación de claves desde Safari History, Chrome, Notes |
| Sep 2026 | Creación de `opencode.jsonc` con 12 proveedores |
| Sep 2026 | Migración de agentes desde `Documents/.opencode/agents/` |
| Sep 2026 | Creación de `.ssh/config` para `control-servidor-casero` |
| Sep 2026 | Instalación de `oh-my-opencode` con `oh-my-openagent` plugin |
| Sep 2026 | Añadido agente `validator` para validación rápida |
| Sep 2026 | Corrección de schema JSONC (models debe ser objeto, no array) |
| Sep 2026 | Instalación de `oh-my-openagent` en `node_modules` |
| Sep 2026 | Documentación completa generada |

---

*Generado el 15 septiembre 2026 · opencode v1.18.31 · oh-my-opencode v4.19.4*
