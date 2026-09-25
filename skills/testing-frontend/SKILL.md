---
name: testing-frontend
description: 'Use when writing or reviewing frontend tests: Vitest, Testing Library, Playwright, Cypress, mocking, coverage, TDD, component testing, E2E, integration tests'
license: MIT
compatibility: opencode
metadata:
  area: frontend
  prioridad: alta
---

## Qué hago

Estrategias y patrones para testing frontend moderno:

### Pirámide de testing frontend
1. **Unitarios** (60%+): funciones puras, hooks, utils — Vitest
2. **Integración** (25%): componentes con interacciones — Testing Library
3. **E2E** (10-15%): flujos completos de usuario — Playwright
4. **Visual** (5%): regresión visual — Chromatic, Percy

### Vitest
- Configuración rápida, compatible con Vite
- Matchers: toBe, toEqual, toContain, toThrow, toMatchSnapshot
- Mocking: vi.mock, vi.spyOn, vi.fn, vi.stubEnv
- Setup/teardown: beforeEach, afterEach, describe con contexto
- Coverage: c8/istanbul integrado
- Config: `vitest.config.ts` con setupFiles, environment: 'jsdom' o 'happy-dom'

### Testing Library (@testing-library/react)
- Enfoque: testear como usuario, no implementación
- Queries: getByRole > getByLabelText > getByPlaceholderText > getByText
- Eventos: userEvent (preferido sobre fireEvent)
- Async: waitFor, findBy*, findAllBy*
- Accesibilidad: getByRole con name, testing accesibilidad intrínseca
- NO testear: estado interno, métodos privados, detalles de implementación

### Playwright
- Multi-browser: chromium, firefox, webkit
- Locators: page.getByRole(), getByTestId(), getByText()
- Assertions: toBeVisible(), toHaveText(), toHaveValue()
- Intercept: page.route() para mock de API
- Component testing: storybook + Playwright
- Mobile: device emulation, geolocation, permissions
- Visual: expect(page).toHaveScreenshot()
- Parallel: sharding en CI, workers

### Mocking de API
- MSW (Mock Service Worker): handler, browser/server workers
- Playwright route interception: page.route()
- Vitest: vi.mock para módulos, msw/node para server

### CI y reporting
- GitHub Actions: matrix de browsers, sharding
- Reportes: HTML, JUnit, JSON
- Retries: Playwright retry config, flaky test detection
- Coverage thresholds en CI

### Buenas prácticas
- Test factory/ builders para datos de prueba (fábricas)
- AAA Pattern: Arrange, Act, Assert
- NO mockear lo que no es tuyo (fetch, router, etc.) — usa MSW
- Tests aislados: limpiar mocks en afterEach
- data-testid como último recurso (preferir roles semánticos)

## Checklist de testing
- [ ] Crítico: flujo de login/registro tiene test E2E
- [ ] Componentes renderizan correctamente estados: loading, empty, error, success
- [ ] Formularios validan errores y envío
- [ ] Mock de API respuestas exitosas y errores
- [ ] Accesibilidad: tests con getByRole verifican estructura semántica
- [ ] No hay tests con sleeps/tiempos fijos
- [ ] Coverage > 80% en código crítico
- [ ] Tests pasan en CI sin flakes conocidos
- [ ] Visual regression testing en componentes clave
- [ ] Mocks se limpian entre tests
