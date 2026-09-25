---
name: rendimiento-frontend
description: 'Use when optimizing web performance: Core Web Vitals, Lighthouse, lazy loading, bundle splitting, CDN, caching, performance budgets, RUM, Web Vitals monitoring'
license: MIT
compatibility: opencode
metadata:
  area: frontend
  prioridad: alta
---

## Qué hago

Guías y estrategias para optimización de rendimiento frontend:

### Core Web Vitals
- **LCP** (≤2.5s): Largest Contentful Paint
  - Optimizar: imágenes next/image/imgix, preload LCP resource, evitar CLS
- **INP** (≤200ms): Interaction to Next Paint
  - Optimizar: code splitting, lazy handlers, evitar long tasks (>50ms)
- **CLS** (≤0.1): Cumulative Layout Shift
  - Optimizar: dimensiones explícitas en img/video, reserva espacio para ads/fonts, evitar inject dinámico arriba del fold

### Estrategias de carga
- **Lazy loading**: IntersectionObserver, loading="lazy", React.lazy + Suspense
- **Preloading**: `<link rel="preload">`, `<link rel="prefetch">`, `<link rel="preconnect">`
- **Priority hints**: fetchpriority="high/low/auto"
- **Fonts**: next/font, font-display: swap/optional, subfont
- **Images**: next/image, WebP/AVIF, responsive srcset/sizes, blur placeholder

### Bundle optimization
- Code splitting por rutas (Next.js App Router automático)
- Dynamic imports: `const Component = dynamic(() => import('./heavy'))`
- Tree shaking: imports específicos, sideEffects en package.json
- Análisis: `next/bundle-analyzer`, `webpack-bundle-analyzer`, `vite-bundle-visualizer`
- Eliminar código muerto: Knip, ts-prune

### Caching
- **HTTP**: Cache-Control, ETag, Service Workers (Workbox)
- **CDN**: static assets con hash en filename, CDN para images/fonts
- **Browser**: localStorage con TTL, SessionStorage, Cache API
- **SWR**: stale-while-revalidate, TanStack Query cache

### Monitoreo (RUM)
- web-vitals library para recolección
- analytics: Core Web Vitals a GA4/Datadog/Sentry
- Performance budgets en CI (Lighthouse CI)
- Real User Monitoring: INP, FID, LCP reales vs lab

### Técnicas avanzadas
- Streaming SSR (Next.js streaming)
- Partial Prerendering (PPR) en Next.js
- Islands architecture (Astro)
- Edge rendering para respuestas rápidas
- Optimistic UI para percepción de velocidad

## Checklist de rendimiento
- [ ] LCP < 2.5s en 75th percentile
- [ ] INP < 200ms
- [ ] CLS < 0.1
- [ ] Imágenes en WebP/AVIF con srcset
- [ ] Fonts con font-display: swap
- [ ] Code splitting implementado
- [ ] Lighthouse Performance score > 90
- [ ] Performance budget configurado en CI
- [ ] Core Web Vitals monitoreados en producción
- [ ] No hay render blocking resources críticos sin preload
