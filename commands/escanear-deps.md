---
description: Escanea dependencias del proyecto en busca de vulnerabilidades
model: opencode-go/deepseek-v4-flash
---

Ejecuta un escaneo completo de vulnerabilidades en las dependencias del proyecto.

1. Si es Node.js, ejecuta `npm audit --audit-level=high`
2. Si hay `package-lock.json`, revisa dependencias transitivas
3. Identifica dependencias desactualizadas
4. Para cada vulnerabilidad encontrada:
   - Explica el riesgo
   - Sugiere la versión parcheada
   - Indica si hay breaking changes

Usa `skill("analisis-dependencias")` para guías detalladas.
