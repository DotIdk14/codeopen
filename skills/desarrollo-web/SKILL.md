---
name: desarrollo-web
description: 'SKILL UNIFICADO de desarrollo web full-stack. Use for ANY frontend or backend web task: React, Next.js, Vue, Angular, CSS, Tailwind, accesibilidad, rendimiento, testing, PWA, Node.js, Python, APIs REST/GraphQL, bases de datos, arquitectura web, diseño de sistemas. Clasifica y deriva a skills especializados según el área.'
license: MIT
compatibility: opencode
metadata:
  area: desarrollo
  prioridad: critica
---

## Rol

Soy el skill de entrada para **todo** desarrollo web. Analizo el requerimiento, determino el área de especialidad y activo el skill especializado correspondiente para la respuesta detallada.

## Clasificador de áreas

Usa esta tabla para identificar el área exacta y cargar el skill adecuado:

| Si el request es sobre...                                           | Activa el skill                      |
| ------------------------------------------------------------------- | ------------------------------------ |
| React, Next.js, Server Components, hooks, estado global             | `react-next`                         |
| CSS, Tailwind, design systems, animaciones, responsive              | `css-ui`                             |
| WCAG, ARIA, screen readers, teclado, contraste, a11y                | `accesibilidad-web`                  |
| Core Web Vitals, LCP/INP/CLS, Lighthouse, bundle splitting, perf    | `rendimiento-frontend`               |
| Vitest, Testing Library, Playwright, E2E, mocking, coverage         | `testing-frontend`                   |
| Service Workers, offline-first, manifest, push, PWA                 | `pwa-mobile`                         |
| Node.js, Express, Fastify, APIs REST, GraphQL                       | `seguridad-apis`                     |
| Autenticación, autorización, OWASP, CORS, rate limiting             | `seguridad-apis` + `seguridad-owasp` |
| Auth, cookies, JWT, OAuth, RBAC                                     | `seguridad-apis`                     |
| Hardening de servidores, headers, TLS, CSP, HSTS                    | `hardening-servidores`               |
| Pentesting, vulnerabilidades, exploits, OWASP Top 10                | `pentesting-web` + `seguridad-owasp` |
| Auditoría de dependencias, SBOM, npm/pip audit                      | `analisis-dependencias`              |
| CI/CD seguro, pipelines, SAST, DAST, escaneo de imágenes            | `seguridad-cicd`                     |
| GDPR, OWASP ASVS, informes de cumplimiento, normativa               | `cumplimiento-normativo`             |

## Manejo de requests multi-área

Si el request abarca **más de un área** (ej: "crea un formulario accesible en React con tests"):
1. Identifica todas las áreas involucradas
2. Activa los skills correspondientes
3. Responde integrando las guías de cada uno, priorizando el orden lógico

Ej: formulario React + a11y + tests → `react-next` + `accesibilidad-web` + `testing-frontend`

## Cross-cutting concerns (aplican siempre)

Pase lo que pase, en cada tarea web verifica:

- **Validación de entrada** siempre del lado servidor (aunque haya validación cliente)
- **TypeScript** estricto para todo el código compartido
- **Manejo de errores** sin exponer stack traces en producción
- **Logging** sin datos sensibles
- **Headers de seguridad**: CSP, HSTS, X-Frame-Options
- **Accesibilidad**: contraste, teclado, labels, roles semánticos
- **Rendimiento**: lazy loading donde aplique, Core Web Vitals awareness
