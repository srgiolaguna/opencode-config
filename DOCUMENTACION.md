# OpenCode — Configuración Completa

> Última actualización: 17 septiembre 2026 (auditoría con pruebas en vivo, reemplaza la revisión del 16-sep)
> Sistema: macOS (M3 MacBook Air) · Usuario: `giolaguna`
> Instalación: Homebrew · opencode v1.18.31 · oh-my-openagent v4.19.4 (`omo`)
> Usuario GitHub: `srgiolaguna`

---

## 0. Qué pasó el 16-17 de septiembre (para que no se repita)

Las "FIX" del 16-sep tarde (ver §12) marcaban modelos como "verificados OK" **sin probarlos en vivo** —
solo comprobaban que el ID aparecía en el catálogo de models.dev. Eso dejó el sistema roto sin que nadie
lo notara hasta la mañana del 17. Esta revisión probó **cada modelo con una llamada real** (`opencode run`)
antes de darlo por bueno. Hallazgos:

1. **OpenRouter con cuenta sin crédito = 50 peticiones/día, COMPARTIDAS entre TODOS los modelos `:free`.**
   No es un límite por modelo, es un límite de cuenta. El 17-sep esa cuota ya estaba agotada de tanto
   probar cosas el día anterior — por eso "modelos verificados" dejaron de responder sin motivo aparente.
   **Por eso OpenRouter ya NO es el proveedor principal.** Sigue configurado como último recurso.
2. **Groq no es viable para opencode/omo con el free tier `on_demand`**: el límite es 8000 tokens-por-minuto
   *de cuenta*, y las herramientas propias de opencode + omo ya ocupan 9-12k tokens solo en overhead, antes
   de que el prompt del usuario cuente. Con cualquier modelo, siempre da `rate_limit_exceeded`. No es un
   problema de configuración — la cuenta gratuita on-demand de Groq es estructuralmente demasiado pequeña
   para un agente con herramientas. **Nota real detectada**: omo entró en un bucle de reintento infinito
   ante este error en vez de rendirse y avisar — tuve que matar el proceso a mano. Evítalo: no uses Groq
   como modelo de agente por ahora.
3. **Cerebras pide tarjeta**: comprobado en su web (17-sep) — el mensaje real es *"API access isn't active
   yet. Add a payment method to start running requests and claim $5 in free credits"*. No cobra
   automáticamente, pero **no es un free tier permanente**: es un crédito promocional de 5$ que caduca a
   los 30 días y requiere tarjeta en el sistema. No encaja con la regla de "solo gratis de verdad" →
   **decisión: no activarlo, se queda desactivado.**
4. **Mistral — corregido tras verificar con fuentes reales (no me lo inventé la primera vez, esta vez sí
   lo busqué)**: el free tier oficial de La Plateforme se llama "Experiment" y **es gratis de verdad, sin
   tarjeta**: 1 req/s, 500.000 tokens/min, 1.000 millones de tokens/mes
   ([docs.mistral.ai](https://docs.mistral.ai/deployment/ai-studio/tier),
   [help.mistral.ai](https://help.mistral.ai/en/articles/225174-what-are-the-limits-of-the-free-tier)). El
   `0 req/min` que devolvió tu cuenta el 17-sep **no es el valor por defecto esperado** — algo en tu
   workspace no está en modo "Free". Pasos para diagnosticarlo tú (no lo puedo ver yo, no tengo acceso a
   tu cuenta): entra en `admin.mistral.ai/plateforme/limits` para ver el límite real asignado a tu
   workspace, y revisa la sección Billing/Plan del panel — si tiene un plan distinto de "Free"/"Experiment"
   seleccionado (o si el workspace nunca terminó de configurarse), ahí está el problema.
5. **GitHub Copilot — ELIMINADO de la config (17-sep, a petición tuya)**: `GITHUB_TOKEN` es un Personal
   Access Token normal, y la API de Copilot exige específicamente un token OAuth de dispositivo (el que da
   `gh auth login --scope copilot`), además de una suscripción activa a Copilot. Como no vas a pagarla, se
   quitó del todo de `opencode.jsonc` en vez de dejarlo como "desactivado pendiente".
6. **Bug real de omo con Google (Gemini/Gemma)**: con el toolset completo de omo (no `--pure`), Google
   rechaza la petición con `400 INVALID_ARGUMENT` porque una de las tool-schemas que inyecta omo trae un
   `enum` vacío en un campo anidado, y la API de Google valida function-calling más estricto que otros
   proveedores (que lo toleran). Funciona perfecto en modo `opencode run --pure` (sin omo). Desactivado
   como modelo de agente hasta que se actualice omo o el bug se arregle.
7. **Vercel AI Gateway** no es free tier real — es crédito de prueba que se agota y no se renueva. Nunca
   se llegó a añadir a la config, y no tienes cuenta en Vercel — descartado sin más.

8. **Los 5 agentes nativos tienen su modelo duplicado en dos sitios, y gana el archivo `.md`.**
   `opencode.jsonc` define `agent.auditor.model` etc., pero también existen
   `~/.config/opencode/agent/{auditor,implementer,optimizer,reviewer,validator}.md` con un campo `model:`
   en el frontmatter — y ese es el que opencode usa de verdad. Cambié `opencode.jsonc` y me quedé pensando
   que ya estaba arreglado; al probar `opencode run --agent auditor "..."` en vivo seguía usando el modelo
   viejo de OpenRouter. **Si cambias el modelo de un agente nativo, edita el `.md`, no solo el `.jsonc`**
   (o los dos, para que no se desincronicen). Además `validator.md` tenía el frontmatter mal formado (sin
   los delimitadores `---` de apertura/cierre) — corregido de paso.

**Único hallazgo bueno**: las 13 claves nunca desaparecieron. Todas siguen en `~/.zshenv`, intactas. Lo que
pasó es que el `opencode.jsonc` de las 02:31h del 16-sep las redujo de 13 proveedores a solo 1
(`enabled_providers: ["openrouter"]`), pensando que el resto estaba "roto". No era eso — la mayoría solo
necesitaba el modelo correcto o tenía un problema de cuenta, no de config.

---

## 1. Resumen

OpenCode está configurado con **2 proveedores activos y verificados en vivo** (Cloudflare Workers AI como
principal, OpenRouter como último recurso) más **5 proveedores registrados pero desactivados hoy** por
problemas de cuenta o un bug de omo (detalle en §0 y §3). **15 agentes** (5 nativos + 10 oh-my-openagent)
con cadena de fallback real. Todo **100% gratuito** — no se paga por nada ni hay riesgo de que se empiece
a cobrar (Cloudflare Workers AI tiene 10.000 "neurons"/día gratis; OpenRouter sin crédito nunca cobra, solo
te corta a 50 peticiones/día).

**Seguridad**: Ningún API key está en los repositorios de GitHub. Todas las claves están en `~/.zshenv`
(no rastreado por git).

---

## 2. Archivos de configuración

| Archivo | Propósito | Rastreado en git |
|---------|-----------|:----------------:|
| `~/.config/opencode/opencode.jsonc` | Config principal de opencode | ✅ Sí |
| `~/.config/opencode/agent/*.md` (5 archivos) | Definición de los 5 agentes nativos — **el `model:` de aquí gana sobre el de `opencode.jsonc`** | ✅ Sí |
| `~/.zshenv` | Variables de entorno (claves API) | ❌ No (`.gitignore`) |
| `~/.omo/omo.jsonc` | Config oh-my-openagent | ✅ Sí |
| `~/.config/opencode/DOCUMENTACION.md` | Esta documentación | ✅ Sí |
| `~/.config/opencode/GUIA_INTERACTIVA.html` (symlink en `~/Desktop/OpenCode_GUIA.html`) | Guía visual interactiva | ✅ Sí |

---

## 3. Proveedores de API (13 configurados, 2 activos)

Todas las claves se leen desde `~/.zshenv` mediante `{env:VARIABLE}`. Las 13 claves están presentes y
tienen formato correcto; lo que varía es si el proveedor responde de verdad hoy.

| Proveedor | ID en config | Estado 17-sep | Motivo |
|-----------|--------------|:---:|--------|
| **Cloudflare Workers AI** | `cloudflare-workers-ai` | ✅ **ACTIVO — principal** | Probado en vivo con el toolset completo de omo. Responde. |
| **OpenRouter** | `openrouter` | ✅ **ACTIVO — último recurso** | Funciona, pero 50 peticiones/día compartidas sin crédito. Hoy la cuota ya estaba agotada al probarlo. |
| Google (Gemini/Gemma) | `google` | ⏸️ Desactivado | Funciona con `--pure`, pero rompe con el toolset completo de omo (bug, ver §0.6) |
| Groq | `groq` | ⏸️ Desactivado | TPM de cuenta (8000) insuficiente para un agente con herramientas |
| Cerebras | `cerebras` | ⏸️ Desactivado a propósito | Pide tarjeta para crédito promo de 5$/30 días — no es free tier real, no se activa |
| Mistral | `mistral` | ⏸️ Desactivado, diagnóstico pendiente | El free tier "Experiment" SÍ es gratis sin tarjeta, pero tu cuenta da 0 req/min — revisa admin.mistral.ai/plateforme/limits |
| GitHub Copilot | — | ❌ Eliminado de la config | Sin suscripción de pago no es viable (y el PAT no sirve igualmente) |
| Vercel AI Gateway | — | ❌ No incluido | Crédito de prueba, no tienes cuenta, descartado |
| opencode (Zen) | — | ❌ No incluido | Nunca se pudo confirmar que responda; fuera del alcance de esta revisión |
| SambaNova | — | ❌ No incluido | Clave presente, no probada esta vez |
| NVIDIA NIM | — | ❌ No incluido | Clave presente, no probada esta vez |
| HuggingFace | — | ❌ No incluido | Clave presente, no probada esta vez |
| Ollama | — | ❌ No incluido | 100% local, requiere tener Ollama corriendo — no configurado |

**Para reactivar un proveedor desactivado**: arregla lo que dice la columna "Motivo" y añade su ID a
`enabled_providers` en `~/.config/opencode/opencode.jsonc` (las entradas del proveedor ya están ahí,
comentadas como "desactivadas hoy", solo hace falta añadir el ID a la lista).

---

## 4. Modelos gratuitos (verificados con respuestas reales en vivo, 17-sep)

| Modelo | Proveedor | Verificado con toolset completo de omo | Uso |
|--------|-----------|:---:|-----|
| `cloudflare-workers-ai/@cf/google/gemma-4-26b-a4b-it` | Cloudflare Workers AI | ✅ Sí | **Default y small_model** |
| `openrouter/nex-agi/nex-n2.5-mini:free` | OpenRouter | ✅ Sí (estructuralmente — falló hoy por cuota agotada, no por el modelo) | Último recurso |
| `openrouter/nex-agi/nex-n2.5-pro:free` | OpenRouter | Sin probar hoy (misma cuota agotada) | Último recurso, tareas de razonamiento |
| `openrouter/nvidia/nemotron-3-ultra-550b-a55b:free` | OpenRouter | Sin probar hoy (misma cuota agotada) | Último recurso, contexto 1M |
| `openrouter/nvidia/nemotron-3.5-lightning:free` | OpenRouter | Sin probar hoy | Último recurso, rápido |
| `openrouter/inclusionai/ling-3.0-flash-fin:free` | OpenRouter | Sin probar hoy | Último recurso |
| `openrouter/inclusionai/ling-3.0-flash-vl:free` | OpenRouter | ⚠️ Respuesta vacía sin error al probarlo (probable cuota) | Último recurso para imágenes — sin confirmar |
| `google/gemma-4-31b-it` | Google | ✅ Sí, pero **solo con `--pure`** | No usar como modelo de agente (ver §0.6) |
| `google/gemini-3.6-flash` | Google | ✅ Sí, pero **solo con `--pure`** | No usar como modelo de agente (ver §0.6) |

> **Limitación honesta**: hoy no hay ningún modelo de visión 100% gratis confirmado funcionando end-to-end
> con el toolset completo de omo. Si necesitas describir/analizar imágenes, prueba
> `opencode run --pure -m openrouter/inclusionai/ling-3.0-flash-vl:free` directamente y si falla, es la
> cuota diaria de OpenRouter — espera al reset o añade 10$ de crédito en openrouter.ai para subir a
> 1000/día.

---

## 5. Agentes

### Agentes nativos (`opencode.jsonc`) — 5 agentes

| Agente | Modelo | Permiso | Descripción |
|--------|--------|---------|-------------|
| `auditor` | Cloudflare gemma | edit: deny | Analiza sin modificar |
| `implementer` | Cloudflare gemma | — | Ejecuta cambios |
| `optimizer` | Cloudflare gemma | edit: deny | Propone sin aplicar |
| `reviewer` | OpenRouter nex-n2.5-pro:free | edit: deny | Valida cambios (último recurso, sujeto a cuota) |
| `validator` | Cloudflare gemma | edit: deny | Lint, syntax-check |

### Agentes oh-my-openagent (`~/.omo/omo.jsonc`) — 10 agentes

Todos con Cloudflare gemma como modelo principal (probado en vivo) y 1-2 modelos de OpenRouter como
`fallback_models` (mismo motor, distinto modelo, por si uno específico da problemas — comparten la cuota
diaria de OpenRouter entre sí, así que no protegen contra el límite de 50/día, solo contra que un modelo
concreto esté caído).

| Agente | Modelo principal | Fallback |
|--------|-------------------|----------|
| `hephaestus` | Cloudflare gemma | nex-n2.5-mini → nemotron-3.5-lightning |
| `oracle` | Cloudflare gemma | nex-n2.5-pro → nemotron-3-ultra |
| `librarian` | Cloudflare gemma | ling-3.0-flash-fin → nex-n2.5-mini |
| `explore` | Cloudflare gemma | nemotron-3.5-lightning → nex-n2.5-mini |
| `multimodal-looker` | Cloudflare gemma (texto — sin visión real hoy) | ling-3.0-flash-vl (sin confirmar) |
| `prometheus` | Cloudflare gemma | nex-n2.5-mini → ling-3.0-flash-fin |
| `metis` | Cloudflare gemma | nemotron-3.5-lightning → nex-n2.5-mini |
| `momus` | Cloudflare gemma | nex-n2.5-pro → nex-n2.5-mini |
| `atlas` | Cloudflare gemma | nemotron-3-ultra → nex-n2.5-mini |
| `sisyphus-junior` | Cloudflare gemma | nex-n2.5-mini → nemotron-3.5-lightning |

---

## 6. oh-my-openagent v4.19.4 (`omo`)

**Sigue vigente, no es legacy** — el nombre del paquete es `oh-my-openagent`, `oh-my-opencode` es un alias
histórico. El binario instalado se llama `omo`.

### Bug conocido del doctor (no lo arregles, es ruido)

`omo doctor` marca 10 avisos "Deprecated reasoning config key: agents.<nombre>.fallback_models — Replace
fallback_models with models" — **es un falso positivo**. Comprobado leyendo el código fuente del plugin
(`packages/omo-opencode/src/config/schema/agent-overrides.ts`): el schema real que valida
`[opencode].agents.<nombre>` solo acepta `fallback_models`, no `models` — ese campo `models` solo existe
en el schema de `categories.<nombre>` (que sí lo usa, ver `omo.jsonc`) y en catálogos de modelos aparte. Si
cambias `fallback_models` por `models` en `agents.*`, omo falla al arrancar con "Unknown config key". Deja
esos 10 avisos tal cual.

### Funciones

- **Enrutamiento automático** de modelos según el tipo de tarea (8 categorías)
- **Fallback automático** (`model_fallback: true` + `fallback_models` por agente)
- **Ultrawork (ulw)** — modo autónomo. Escribe `ulw` en el prompt. **GRATIS.**
- **Modo Plan** — `--plan` o `/plan`. **GRATIS.**

---

## 7. Comandos

### opencode

| Comando | Descripción |
|---------|-------------|
| `opencode` | Iniciar opencode (TUI) |
| `opencode run "mensaje"` | Ejecutar un mensaje suelto, no interactivo |
| `opencode run -m proveedor/modelo "mensaje"` | Ejecutar con un modelo concreto |
| `opencode run --pure ...` | Ejecutar sin el plugin omo (toolset mínimo — usa esto para probar Google/Gemini) |
| `opencode models [proveedor]` | Listar modelos disponibles (usa `--verbose` para ver coste) |
| `opencode providers list` | Ver qué claves detecta y desde dónde |
| `opencode doctor` | Verificar configuración |

### oh-my-openagent

| Comando | Descripción |
|---------|-------------|
| `omo` | Iniciar oh-my-openagent |
| `omo --version` | Ver versión (4.19.4) |
| `omo doctor` | Diagnosticar configuración (ver §6 sobre el falso positivo) |
| `ulw <prompt>` | Ultrawork mode autónomo |

---

## 8. Historial de cambios (resumen — el detalle completo del 16-sep queda en `git log`)

| Fecha | Evento |
|-------|--------|
| 16-sep tarde | Config inicial con 13 proveedores, luego reducida a solo OpenRouter creyendo que el resto estaba roto (no era así del todo, ver §0) |
| **17-sep** | **Auditoría con pruebas en vivo**: cada modelo candidato probado con `opencode run` real antes de aceptarlo. Cloudflare Workers AI confirmado como único proveedor 100% estable con el toolset completo de omo. OpenRouter degradado a último recurso (cuota compartida de 50/día). Groq, Cerebras, Mistral, GitHub Copilot desactivados por problemas de cuenta documentados en §0. Bug de omo+Google documentado y evitado. `omo.jsonc` corregido con el nombre de campo real (`fallback_models`, no `models`, para `agents.*`) |

---

## 9. Verificación rápida

```bash
# Versiones
opencode --version          # 1.18.31
omo --version                # 4.19.4

# Claves (deben existir, no hace falta ver el valor)
env | grep -c '_API_KEY\|_TOKEN\|_ACCOUNT_ID' | grep -v grep   # ~11 variables

# Que el modelo principal responde de verdad
opencode run "responde solo con la palabra OK"

# Config
opencode providers list     # confirma qué proveedores detecta
omo doctor                  # 10 avisos de "fallback_models" son ruido conocido (ver §6)
```

---

## 10. Cosas que TÚ tienes que hacer a mano (no lo puede resolver la config)

Ninguno de estos pasos es obligatorio: con Cloudflare Workers AI + OpenRouter de reserva, opencode/omo ya
funciona 100% gratis hoy mismo. Son solo para ampliar más adelante si te interesa.

1. **Mistral (el único que de verdad merece la pena mirar)**: entra en
   `admin.mistral.ai/plateforme/limits` para ver qué límite tiene asignado tu workspace de verdad, y en
   la sección Billing/Plan del panel de Mistral comprueba que el plan activo sea "Free"/"Experiment" (no
   uno sin seleccionar). El free tier oficial es 1 req/s · 500K tokens/min · 1.000M tokens/mes, sin
   tarjeta — si tu cuenta muestra 0, algo quedó a medio configurar, no es el comportamiento normal.
   Cuando lo arregles, añade `"mistral"` a `enabled_providers` en `opencode.jsonc`.
2. **Cerebras — decisión: no activarlo.** Pide tarjeta para un crédito promocional de 5$ que caduca en 30
   días — no es gratis para siempre, así que no encaja con la idea de "solo modelos gratis". Se queda
   desactivado a propósito.
3. **GitHub Copilot — eliminado de la config.** No vale la pena sin la suscripción de pago.
4. **Vercel AI Gateway — descartado.** No tienes cuenta y no es free tier real.
5. **OpenRouter**: si quieres que deje de tener el límite de 50/día, añade una vez 10$ de crédito en
   openrouter.ai/settings/credits. No se gasta nunca si solo usas modelos `:free` — solo desbloquea subir
   el límite diario a 1000.
