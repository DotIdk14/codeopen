---
description: Verifica que el último commit cumple con estándares de seguridad
agent: revisor-codigo
model: opencode-go/deepseek-v4-flash
---

Verifica que los cambios del último commit cumplen con los estándares de seguridad del proyecto.

1. Obtén el diff del último commit: `git diff HEAD~1 HEAD`
2. Revisa específicamente:
   - ¿Hay secretos o credenciales hardcodeadas?
   - ¿Se introducen nuevas dependencias? -> verifica si son seguras
   - ¿Hay cambios en configuraciones de seguridad?
   - ¿Se están manejando correctamente errores y excepciones?
3. Reporta cualquier hallazgo de seguridad inmediatamente
4. Si todo está limpio, confirma que el commit es seguro
