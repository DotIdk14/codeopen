---
name: analisis-dependencias
description: Análisis de vulnerabilidades en dependencias npm, pip, maven, go modules; SBOM, actualizaciones
license: MIT
compatibility: opencode
metadata:
  area: seguridad
  prioridad: alta
---

## Qué hago

Análisis exhaustivo de dependencias en busca de vulnerabilidades conocidas. Generación de SBOM y guías de actualización.

## Cuándo usarme

- En CI/CD para escanear dependencias antes de deploy
- Para auditar un proyecto existente
- Para generar un SBOM (Software Bill of Materials)
- Para planificar actualizaciones de seguridad

## Herramientas por ecosistema

### Node.js / npm
```bash
npm audit              # Escaneo básico
npm audit --audit-level=high  # Solo altas/críticas
npm outdated           # Paquetes desactualizados
npx snyk test          # Snyk (más completo)
```

### Python (pip)
```bash
pip-audit
safety check
pip list --outdated
```

### Go
```bash
govulncheck ./...
go list -m -u all
```

### Docker
```bash
docker scout quick <image>
trivy image <image>
grype <image>
```

## Flujo recomendado

1. `npm audit` / `pip-audit` / `govulncheck` - escaneo rápido
2. Revisión manual de findings críticos/altos
3. `npm audit fix` con precaución (revisar breaking changes)
4. Pruebas post-actualización
5. Generar SBOM: `npm sbom` / `cyclonedx-bom`

## Priorización

- **Crítica**: Parche inmediato (< 24h)
- **Alta**: Parche planificado (< 7 días)
- **Media**: Revisar en próximo ciclo
- **Baja**: Monitorear

## SBOM (Software Bill of Materials)

Formato recomendado: SPDX o CycloneDX

```bash
# Generar SBOM con CycloneDX
npx @cyclonedx/bom . -o sbom.json
```

Usar SBOM para:
- Inventario de dependencias
- Cumplimiento normativo
- Respuesta rápida a CVEs nuevos
