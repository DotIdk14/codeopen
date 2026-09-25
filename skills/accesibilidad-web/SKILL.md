---
name: accesibilidad-web
description: 'Use when auditing, building or reviewing web accessibility: WCAG 2.2, ARIA, screen readers, keyboard navigation, axe-core, cumplimiento legal (EU 301549, Section 508)'
license: MIT
compatibility: opencode
metadata:
  area: frontend
  prioridad: alta
---

## Qué hago

Patrones y guías para accesibilidad web inclusiva y cumplimiento normativo:

### WCAG 2.2 Niveles
- **A**: mínimo indispensable
- **AA**: estándar legal (EU 301549, Section 508)
- **AAA**: accesibilidad avanzada

### Principios POUR
1. **Perceptible**: alternativas textuales, subtítulos, adaptable
2. **Operable**: teclado, tiempo suficiente, navegación, input modalities
3. **Comprensible**: legible, predecible, asistencia en input
4. **Robusto**: compatible con AT, HTML semántico válido

### HTML semántico
- landmarks: header, nav, main, aside, footer
- headings: un solo h1, jerarquía sin saltos
- forms: label asociado, fieldset/legend, aria-describedby
- tablas: caption, th scope, headers
- listas: ul/ol para navegación y contenidos listados

### ARIA (Accessible Rich Internet Applications)
- Primera regla: no usar ARIA si HTML nativo sirve
- Roles: button, dialog, alert, tabpanel, progressbar
- Properties: aria-label, aria-labelledby, aria-describedby
- States: aria-expanded, aria-pressed, aria-current, aria-hidden
- Patrones: combobox, dialog modal, tabs, accordion, carousel
- ARIA Authoring Practices Guide (APG) como referencia

### Teclado
- Tab order lógico: tabindex="0" o "-1" solo
- Focus management: focus() en modales, skip links
- Focus trap en modales y drawers
- Teclas comunes: Enter/Space para activar, Escape cerrar, Arrow para tabs/listboxes

### Testing de accesibilidad
- **Automático**: axe-core (axe DevTools, @axe-core/react, pa11y)
- **Semi-automático**: WAVE, Lighthouse Accessibility
- **Manual**: NVDA/VoiceOver/JAWS, keyboard-only testing
- **Checklist**: WCAG-EM Report Tool

### Checklist rápida
- [ ] Alt texts descriptivos en imágenes decorativas (alt="")
- [ ] Skip link visible al inicio
- [ ] Contraste de color mínimo 4.5:1 (AA)
- [ ] Todos los inputs tienen label visible
- [ ] Navegación completa por teclado
- [ ] focus visible en todos los elementos interactivos
- [ ] Modales: focus trap + Escape para cerrar
- [ ] Mensajes de error asociados al input via aria-describedby
- [ ] No hay autoplay de video/audio sin control
- [ ] Animaciones respetan prefers-reduced-motion
