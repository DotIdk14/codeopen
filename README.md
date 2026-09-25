# codeopen

Respaldo portable de la configuración personalizada de OpenCode.

## Contenido

- `opencode.jsonc`: configuración global.
- `AGENTS.md`: reglas e instrucciones globales.
- `agents/`: agentes personalizados.
- `commands/`: comandos personalizados.
- `skills/`: skills propias de OpenCode.
- `claude-skills/`: skills compatibles con Claude.
- `agents-skills/`: skills compatibles con Agent Skills.

## Exclusiones de seguridad

No se incluyen `auth.json`, API keys, tokens, bases de datos, logs, snapshots, cachés, `node_modules` ni archivos `.env`.

## Restauración

Copia el contenido de este repositorio a `~/.config/opencode/` y, opcionalmente, las carpetas de skills a `~/.claude/skills/` y `~/.agents/skills/`. Después reinicia OpenCode para que cargue la configuración.

## Repositorio

Repositorio privado de respaldo de OpenCode.
