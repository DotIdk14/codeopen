---
description: Revisión de código del último commit o cambios sin stage
agent: revisor-codigo
model: opencode-go/kimi-k2.7-code
---

Realiza una revisión de código de los cambios recientes en el proyecto.

1. Obtén el diff de los últimos cambios (`git diff` o `git diff --cached`)
2. Revisa cada archivo modificado en busca de:
   - Vulnerabilidades de seguridad
   - Problemas de rendimiento
   - Bugs y edge cases
   - Calidad del código y buenas prácticas
3. Proporciona retroalimentación estructurada por severidad
