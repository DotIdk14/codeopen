# codeopen

Respaldo portable de la configuración global de **OpenCode v2**.

Fuente de verdad en vivo: `~/.config/opencode/`. Este repositorio es el respaldo y
la fuente de la que se restaura.

## Sintaxis: V2

OpenCode v2 usa `permissions`, `shell` y `subagent`.
**No** usar los campos legacy `permission`, `bash`, `task`, `temperature`,
`tools`, `maxSteps` ni `provider`/`plugin` en singular.

## Sistema multiagente por tiers

Objetivo: **maximizar horas de vibecoding por dólar**.

```
USER
  ↓
web-developer  .............  ORQUESTADOR (Tier 1, barato)
  │
  ├── explorador-repo  .....  Tier 1 · contexto compacto, read-only
  ├── planificador-web  ....  Tier 1 · diseño/plan, read-only
  ├── mecanico  ...........  Tier 1 · lint, types, tests, boilerplate
  ├── frontend-css/a11y/perf  Tier 1
  ├── seguridad-*  .........  Tier 1 · solo cuando hay superficie real
  │
  ├── frontend-react/pwa/testing  Tier 2 · implementación
  ├── backend-api  ........  Tier 2
  ├── arquitecto-web  ......  Tier 2 · arquitectura
  ├── revisor-codigo  .....  Tier 2 · review del diff
  │
  └── architect-premium  ..  Tier 3 · Codex. Solo escalamiento estricto.
```

### Modelos

| Tier | Provider | Modelo | Uso |
|---|---|---|---|
| 1 | `opencode-go` | `qwen3.8-flash` | explorar, leer, formatear, auditar, mecánico |
| 2 | `opencode-go` | `kimi-k2.7-code` | implementar, integrar, revisar |
| 3 | `openai` | `gpt-6.1-sol` | causa raíz, concurrencia, datos, migraciones |

El `model` raíz de `opencode.jsonc` es el **más barato a propósito**: un agente sin
`model:` propio hereda ese, así que un agente futuro que olvide declararlo cuesta
barato y no caro.

### Política premium

`architect-premium` (Codex) solo entra cuando:

1. Dos intentos razonables de Tier 1/2 fallaron, **o**
2. Hay riesgo en seguridad, integridad de datos, DB, concurrencia, transacciones,
   idempotencia o migraciones, **o**
3. La causa raíz sigue desconocida, **o**
4. Se requiere una decisión arquitectónica compleja, **o**
5. El usuario lo pide explícitamente.

Nunca para CSS, lint, imports, documentación, tests simples o boilerplate.

## Providers requeridos

| Pool | Comando | Estado |
|---|---|---|
| OpenCode Go | `/connect` → OpenCode Go, o `opencode auth login opencode-go` | **pendiente de conectar** |
| ChatGPT / OpenAI | `/connect` → OpenAI | conectado (OAuth) |
| GitHub Copilot | `/connect` → GitHub Copilot | no conectado |

> Sin OpenCode Go conectado, `opencode-go/*` devuelve `Missing API key.` y los
> agentes de Tier 1 y 2 no pueden ejecutarse.

## Contenido

- `opencode.jsonc` — configuración global (permisos, modelo raíz, agentes JSON)
- `AGENTS.md` — reglas globales: guardrails Windows corporativo + routing + seguridad
- `agents/` — 24 agentes
- `commands/` — comandos slash
- `skills/` — skills propias de OpenCode
- `claude-skills/` — skills compatibles con Claude
- `agents-skills/` — skills compatibles con Agent Skills
- `cli.json` — preferencias del TUI (sin secretos)

## Exclusiones de seguridad

`.gitignore` excluye `auth.json`, `service.json`, `opencode.db`, `*.env`,
`*.key`, `*.secret`. **Ninguna credencial debe entrar a este repositorio.**

`service.json` contiene el password del servicio en background de OpenCode: vive
solo en `~/.config/opencode/` y nunca se versiona.

## Restauración

```bash
cp opencode.jsonc AGENTS.md cli.json ~/.config/opencode/
cp -r agents commands skills ~/.config/opencode/
```

Después, dentro de OpenCode: `/reload`.

## Verificación

```bash
opencode --version                # debe ser v2.x
opencode auth list                # providers conectados
opencode reload
opencode debug config             # 3 fuentes de config, JSON válido
opencode debug agents             # 31 agentes (24 propios + 7 builtins)
```

## Fase 2

Endurecer el routing y contener el coste de contexto.

### `experimental.policies`

Prohibiciones absolutas (`deny`) que **vencen** a `permissions` y a "Allow always".
Son distintas de `permissions` (que admite `allow` / `ask` / `deny`).
Precedencia: **global > proyecto**.

### Clasificador fast-path

Toda tarea se clasifica antes de delegar: `TRIVIAL` → Tier 1 (`mecanico`),
`NORMAL` → Tier 2, `HIGH_RISK` → `architect-premium` + agentes de seguridad.

Superficies `HIGH_RISK`:

| Superficie | Motivo |
|---|---|
| Autenticación, autorización, sesiones, JWT | acceso y elevación de privilegios |
| Esquema de base de datos, migraciones | integridad de datos irreversible |
| Contratos de API (REST/GraphQL) | breaking changes aguas abajo |
| Pagos, dinero, facturación | pérdida económica |
| Criptografía y gestión de secretos | exposición irreversible |
| Concurrencia, transacciones, idempotencia | condiciones de carrera, datos corruptos |
| `permissions`, `experimental.policies`, permisos de agente | auto-modificación del control de acceso |
| Configuración del host: red, drivers, servicios, registry | fuera del alcance del proyecto |
| `Dockerfile`, CI/CD, deploy, TLS | superficie de ataque y producción |

### Serena (code intelligence)

Capa de code intelligence **semántica** y **read-only**, con **allowlist explícita
por agente** (no se habilita en bloque).

Nota: Serena requiere instalación oficial previa
(`uv tool install -p 3.13 serena-agent`). Como es una instalación a nivel de sistema
y requiere aprobación del usuario, el server MCP está en `disabled: true` en
`opencode.jsonc`.

### Política de context-budget

Mínimo contexto suficiente, sin re-lecturas. Un archivo se lee una vez; los agentes
siguientes consumen el bloque `DISCOVERY`, no los archivos completos otra vez.

### Security gates condicionales

Se aplican **solo cuando la tarea toca la superficie correspondiente**, no como
ritual. CSS/lint/imports/documentación no disparan checklist de seguridad.
El triage de escalamiento a agentes de seguridad sigue la tabla de
`AGENTS.md` ("Cuándo escalar a los agentes de seguridad").

### Quarantine de supply chain para skills

Skills de terceros se instalan en cuarentena (`agents-skills/`, `claude-skills/`)
y se revisan antes de promoverlas a `skills/`.

### Contrato de respuesta final

Todo handoff termina con:

```
IMPLEMENTED / VALIDATED / SECURITY / AGENTS USED / PREMIUM / RISKS
```

Más el `ROUTING SUMMARY` del orquestador: tier usado, agentes invocados y coste
aproximado de contexto.

### Nota de bug corregido

El Tier 1 estaba en `opencode-go/deepseek-v4.1-flash` y falló con:

```
requires Global regions
```

Se migró a `opencode-go/qwen3.8-flash`, verificado funcional.
La tabla "Modelos" de arriba conserva el valor histórico.
