---
name: css-ui
description: 'Use when working with CSS, Tailwind, design systems, responsive design, animaciones: estilos, UI components, theming, CSS security y buenas prácticas de maquetación'
license: MIT
compatibility: opencode
metadata:
  area: frontend
  prioridad: alta
---

## Qué hago

Guías y patrones para CSS, frameworks de estilos y sistemas de diseño:

### Tailwind CSS
- Utility-first: composición con clases atómicas
- Configuración: tailwind.config.{ts,js}, theme extensión
- Dark mode: class-based con strategy: 'class'
- Responsive: breakpoints sm/md/lg/xl/2xl
- Optimización: purging en producción, JIT mode
- Componentes: patrones con @apply (uso moderado)

### CSS moderno
- CSS Grid y Flexbox para layouts
- Container Queries (@container)
- CSS Custom Properties (variables) para theming
- CSS Modules para scoping en componentes
- Cascade Layers (@layer) para organización
- has(), :where(), :is() selectors

### Design Systems
- Tokens: colores, tipografía, spacing, shadows
- Componentes base: Button, Input, Modal, Select
- Estados: hover, focus, active, disabled, error
- Variantes: size, intent (primary/secondary/ghost)
- Política de colores: contraste WCAG AA mínimo

### Responsive y accesibilidad visual
- Mobile-first: min-width queries
- Unidades relativas: rem, em, vw, vh, clamp()
- Reduced motion: prefers-reduced-motion
- Focus visible: :focus-visible outline
- High contrast mode: prefers-contrast

### Animaciones
- CSS transitions y animations
- Framer Motion / motion para React
- Performance: animaciones solo en transform y opacity
- prefers-reduced-motion: respetar siempre

### Seguridad en CSS/UI
- CSS injection: validar input de usuarios en estilos inline
- Sanitizar clases generadas dinámicamente
- No usar url() con input de usuario sin validar
- CSP: evitar unsafe-inline para estilos si es posible
- No exponer datos sensibles en pseudo-elementos (content)
