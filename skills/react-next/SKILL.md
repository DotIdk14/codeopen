---
name: react-next
description: 'Use when building or reviewing React/Next.js: componentes, hooks, Server Components, SSR/SSG, estado global, patrones y seguridad en frontend con React 18+ y Next.js 14+'
license: MIT
compatibility: opencode
metadata:
  area: frontend
  prioridad: alta
---

## Qué hago

Patrones, buenas prácticas y consideraciones de seguridad para aplicaciones React y Next.js:

### React 18+
- Componentes Server vs Client en RSC
- Hooks: useState, useEffect, useReducer, useCallback, useMemo, useRef, useId
- Suspense, Transitions, useDeferredValue, useOptimistic
- Context API, Portales, Refs, Error Boundaries
- Composicion vs herencia, patrones render props, compound components

### Next.js 14+
- App Router: layout, page, loading, error, not-found, route groups
- Server Components por defecto, 'use client' solo cuando es necesario
- Data fetching: fetch con cache, Server Actions, Route Handlers
- ISR, SSR, SSG, static exports
- Middleware, rewrites, redirects, headers
- Turbopack, optimización de imágenes (next/image), fonts (next/font)

### Estado global y data fetching
- TanStack Query / SWR para fetching y cache
- Zustand, Jotai, Valtio para estado global mínimo
- Context solo para temas, locales, auth (no para estado que cambia frecuentemente)
- URL como estado: useSearchParams, next/navigation

### Seguridad en React/Next.js
- XSS: dangerouslySetInnerHTML validado, sanitización con DOMPurify
- CSRF: SameSite cookies, tokens en Server Actions
- Server Actions: validación de entrada SIEMPRE del lado servidor
- next/image para optimización segura de imágenes
- next.config.js: headers de seguridad (CSP, HSTS)
- No exponer secrets en Client Components (process.env.NEXT_PUBLIC_*)
- Sanitizar props en RSC: no pasar objetos sensibles al cliente

## Checklist de seguridad React/Next

- [ ] No hay dangerouslySetInnerHTML sin sanitizar
- [ ] Server Actions validan entrada con esquema (Zod)
- [ ] Las cookies de sesión usan httpOnly, secure, SameSite=Lax/Strict
- [ ] next.config.js tiene headers de seguridad (CSP, X-Frame-Options)
- [ ] No hay API keys/tokens en Client Components
- [ ] Middleware de autenticación protege rutas sensibles
- [ ] rate limiting en Route Handlers y Server Actions
- [ ] Input validation en ambos lados (cliente y servidor)
