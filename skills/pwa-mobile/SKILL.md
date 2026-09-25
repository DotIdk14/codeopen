---
name: pwa-mobile
description: 'Use when building Progressive Web Apps, mobile web, responsive design, offline-first apps: Service Workers, Cache API, IndexedDB, Web App Manifest, push notifications, responsive mobile UX'
license: MIT
compatibility: opencode
metadata:
  area: frontend
  prioridad: media
---

## Qué hago

Patrones y guías para Progressive Web Apps y experiencia móvil web:

### Service Workers
- Ciclo de vida: install, activate, fetch, push, sync
- Estrategias de cache:
  - **Cache First**: assets estáticos (versionados)
  - **Network First**: API calls con fallback a cache
  - **Stale While Revalidate**: contenido que cambia poco
  - **Network Only**: datos sensibles o mutaciones
- Workbox: high-level API sobre Service Workers
- Precache: precache de shells de app en install
- Runtime cache: estrategias por patrón de ruta
- Update flow: skipWaiting + clientsClaim para actualización inmediata

### Offline-first
- App Shell: HTML mínimo + JS + CSS en cache
- IndexedDB: almacenamiento estructurado offline (Dexie, idb)
- Background Sync: sincronizar mutations cuando hay conexión
- Estrategia: construir para offline, mejorar para online
- Manejo de conectividad: navigator.onLine + eventos online/offline

### Web App Manifest
- name, short_name, description, start_url, display (standalone)
- icons: sizes 192, 512, maskable
- theme_color, background_color
- orientation: any, portrait, landscape
- related_applications para Play Store / App Store

### Push Notifications
- VAPID keys para autenticación
- Notification API: title, body, icon, badge, data, actions
- Manejo de permiso: Notification.requestPermission()
- Mejores prácticas: no pedir permiso inmediato, contexto primero
- Click handling: clients.openWindow(), focus existing window

### Responsive mobile UX
- Touch targets: mínimo 48x48px (WCAG)
- Viewport: `<meta name="viewport" content="width=device-width, initial-scale=1">`
- Input: autocomplete, inputmode, enterkeyhint
- Safe areas: env(safe-area-inset-*) para notch/isla
- Gestures: no interferir con swipe nativo del browser
- Performance mobile: reducir JS, lazy load, critical CSS inline

### Seguridad en PWA
- **HTTPS obligatorio** para Service Workers
- Content Security Policy estricta
- No cachear datos sensibles en Cache API
- Sanitizar datos de IndexedDB al renderizar
- Push: validar payload en servidor, no confiar en data de push
- Manifest: no incluir URLs absolutas internas sensibles

## Checklist de PWA
- [ ] HTTPS habilitado (requisito para SW)
- [ ] Manifest.json completo (name, icons, start_url, display)
- [ ] Service Worker registrado con estrategia de cache definida
- [ ] App funciona offline (app shell + datos cacheados)
- [ ] Tiempo de carga inicial < 3s en 3G
- [ ] Touch targets mínimo 48x48px
- [ ] Lighthouse PWA score > 90
- [ ] Push notifications implementadas con permiso contextual
- [ ] Background sync para operaciones offline
- [ ] Safe areas manejadas para dispositivos con notch
