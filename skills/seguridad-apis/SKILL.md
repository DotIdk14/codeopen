---
name: seguridad-apis
description: Seguridad en APIs REST y GraphQL: autenticación, autorización, rate limiting, validación, CORS
license: MIT
compatibility: opencode
metadata:
  area: seguridad
  prioridad: alta
---

## Qué hago

Guía especializada en seguridad de APIs REST y GraphQL para backend.

## Cuándo usarme

- Al diseñar un sistema de autenticación/autorización
- Para revisar la seguridad de una API existente
- Para implementar rate limiting, CORS, y validación
- Para configurar JWT, OAuth2, refresh tokens

## Prácticas de seguridad para APIs

### Autenticación
- **JWT**: Access token corta duración (15min), refresh token larga duración (7 días)
- **JWT Storage**: httpOnly cookies > localStorage
- **OAuth2**: Prefiere Authorization Code Flow con PKCE
- **MFA**: Ofrece siempre como opción

### Autorización
- RBAC implementado en middleware, no en cada ruta
- Validación de permisos en cada endpoint
- Deny por defecto: solo lo explícitamente permitido
- Pruebas de autorización por rol

### Rate Limiting
- Por IP, por usuario, por endpoint
- Estrategias: Token Bucket, Sliding Window
- Headers: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `Retry-After`
- Código 429 cuando se excede

### Validación de entrada
- Schemas de validación en cada endpoint (Zod, Pydantic, Joi)
- Sanitización de entrada (NO confíes en el cliente)
- Content-Type validation
- Tamaño máximo de payload

### CORS
- Orígenes específicos, NO `*`
- Métodos específicos
- Credentials solo si es necesario
- Preflight con max-age apropiado

### Headers de seguridad
```
Content-Security-Policy: default-src 'self'
Strict-Transport-Security: max-age=63072000; includeSubDomains
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
X-XSS-Protection: 0
```

### GraphQL específico
- Depth limiting en queries
- Query cost analysis
- Rate limiting por query complexity
- Deshabilitar introspection en producción
- Persisted queries
