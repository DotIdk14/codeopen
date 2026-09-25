---
name: seguridad-owasp
description: OWASP Top 10: detección, mitigación y prevención de las vulnerabilidades web más críticas
license: MIT
compatibility: opencode
metadata:
  area: seguridad
  prioridad: crítica
---

## Qué hago

Experto en OWASP Top 10. Proporciono guías detalladas para detectar, mitigar y prevenir cada vulnerabilidad del OWASP Top 10.

## Cuándo usarme

- Durante code review con enfoque de seguridad
- Al auditar una aplicación web existente
- Al diseñar la arquitectura de seguridad de un nuevo proyecto
- Para escribir guías de seguridad para el equipo

## OWASP Top 10: Resumen rápido

### A01: Broken Access Control
- **Detección**: Verificar que cada endpoint valide permisos del usuario
- **Mitigación**: RBAC, denying por defecto, validación lado servidor
- **Ejemplo**: `if (!usuario.tienePermiso('admin')) return 403`

### A02: Cryptographic Failures
- **Detección**: Buscar TLS, hashing de passwords, datos sensibles en texto plano
- **Mitigación**: bcrypt/argon2 para passwords, TLS 1.3, no almacenar datos sensibles innecesarios

### A03: Injection
- **Detección**: Concatenación de strings en queries SQL/NoSQL, eval(), shell exec
- **Mitigación**: Prepared statements, ORM parameterizado, validación de entrada
- **Ejemplo SQLi**: `db.query("SELECT * FROM users WHERE id = ?", [id])` en vez de interpolación

### A04: Insecure Design
- **Detección**: Falta de rate limiting, falta de validación, lógica de negocio vulnerable
- **Mitigación**: Threat modeling, Secure by Design, rate limiting, validación servidor

### A05: Security Misconfiguration
- **Detección**: Debug habilitado, CORS muy permisivo, headers faltantes
- **Mitigación**: Hardening, revisión de configs, escaneo automático
- **Headers esenciales**: CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy

### A06: Vulnerable and Outdated Components
- **Detección**: npm audit, Snyk, OWASP Dependency-Check
- **Mitigación**: Actualización continua, SBOM, monitoreo de CVEs

### A07: Identification and Authentication Failures
- **Detección**: Passwords débiles, falta de MFA, sesiones sin expiración
- **Mitigación**: MFA, políticas de password, httpOnly cookies, rotación de JWT

### A08: Software and Data Integrity Failures
- **Detección**: CI/CD sin firmar, dependencias sin checksum
- **Mitigación**: Firmado de artifacts, package-lock.json, SLSF, firmado de commits

### A09: Security Logging and Monitoring Failures
- **Detección**: Falta de logging, logs sin estructura, sin alertas
- **Mitigación**: Logging estructurado, monitoreo, alertas en tiempo real

### A10: Server-Side Request Forgery (SSRF)
- **Detección**: URLs de usuario usadas en requests del servidor
- **Mitigación**: Allowlist de URLs, validación de esquemas, network policies
