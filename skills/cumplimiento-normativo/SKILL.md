---
name: cumplimiento-normativo
description: Cumplimiento normativo: GDPR, OWASP ASVS, checklist de seguridad, generación de informes
license: MIT
compatibility: opencode
metadata:
  area: cumplimiento
  prioridad: media
---

## Qué hago

Guía para cumplimiento normativo en aplicaciones web: GDPR, OWASP ASVS, e informes de auditoría.

## Cuándo usarme

- Para preparar una aplicación para auditoría de seguridad
- Para verificar cumplimiento con GDPR
- Para generar informes de seguridad formales
- Para implementar controles según OWASP ASVS

## OWASP ASVS (Application Security Verification Standard)

### Nivel 1 (Opportunistic) - Mínimo
- Todas las vulnerabilidades OWASP Top 10 mitigadas
- Validación de entrada básica
- Autenticación implementada
- Logging básico

### Nivel 2 (Standard) - Recomendado
- Controles de seguridad más estrictos
- Rate limiting
- MFA
- Headers de seguridad completos
- Pruebas de penetración
- SAST/DAST en CI/CD

### Nivel 3 (Advanced) - Máxima seguridad
- Arquitectura Zero Trust
- Hardening completo
- Monitoreo y respuesta en tiempo real
- Pruebas de penetración anuales
- Bug bounty program

## Checklist GDPR para aplicaciones web

### Consentimiento y transparencia
- [ ] Consentimiento explícito para cookies y tracking
- [ ] Política de privacidad clara y accesible
- [ ] Aviso de recolección de datos en puntos de captura
- [ ] Opción para retirar consentimiento fácilmente

### Derechos del usuario
- [ ] Derecho de acceso (exportar datos personales)
- [ ] Derecho de rectificación
- [ ] Derecho de supresión (right to be forgotten)
- [ ] Derecho de portabilidad
- [ ] Derecho de oposición

### Seguridad de datos
- [ ] Cifrado en reposo (AES-256)
- [ ] Cifrado en tránsito (TLS 1.3)
- [ ] Pseudonimización donde sea posible
- [ ] Política de retención de datos definida
- [ ] Notificación de breaches (72h)

### Documentación
- [ ] Registro de actividades de procesamiento (ROPA)
- [ ] DPIAs para procesamientos de alto riesgo
- [ ] Data Protection Officer designado (si aplica)
- [ ] Acuerdos DPA con procesadores de datos

## Informe de auditoría

Estructura recomendada:
1. Resumen ejecutivo
2. Alcance y metodología
3. Hallazgos (ordenados por severidad)
4. Riesgo de cada hallazgo
5. Recomendaciones y plan de remediación
6. Evidencia y capturas
7. Checklist de cumplimiento
