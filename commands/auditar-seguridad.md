---
description: Ejecuta una auditoría completa de seguridad del proyecto
agent: auditor-seguridad
model: opencode-go/kimi-k2.7-code
---

Ejecuta una auditoría completa de seguridad para este proyecto. Sigue este flujo:

1. Examina `package.json` y las dependencias
2. Revisa la configuración de seguridad (CORS, headers, CSP, autenticación)
3. Escanea dependencias con `npm audit`
4. Revisa el manejo de errores y logging
5. Genera un informe completo con hallazgos priorizados por severidad

Usa los skills `seguridad-owasp`, `analisis-dependencias`, y `hardening-servidores` según sea necesario.
