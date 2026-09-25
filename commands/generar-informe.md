---
description: Genera un informe de cumplimiento y seguridad del proyecto
agent: auditor-seguridad
model: opencode-go/kimi-k2.7-code
---

Genera un informe completo de seguridad y cumplimiento para este proyecto.

1. Carga `skill("cumplimiento-normativo")`, `skill("seguridad-owasp")`, y `skill("analisis-dependencias")`
2. Evalúa el proyecto contra:
   - OWASP Top 10
   - Checklist de seguridad básica (AGENTS.md)
   - Checklist de hardening (AGENTS.md)
   - Dependencias vulnerables
3. Genera un informe estructurado con:

```
# Informe de Seguridad: [Proyecto]
## Resumen ejecutivo
## Cumplimiento OWASP Top 10
## Análisis de dependencias
## Hallazgos y recomendaciones
## Plan de remediación
```

4. Incluye secciones de cumplimiento normativo si aplica (GDPR)
