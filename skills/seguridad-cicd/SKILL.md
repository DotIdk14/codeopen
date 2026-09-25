---
name: seguridad-cicd
description: Seguridad en pipelines CI/CD: secretos, firmado, SAST, DAST, escaneo de imágenes, SLSF
license: MIT
compatibility: opencode
metadata:
  area: devsecops
  prioridad: alta
---

## Qué hago

Integración de seguridad en pipelines CI/CD. Automatización de escaneos, gestión segura de secretos, firmado de artifacts y cumplimiento SLSF.

## Cuándo usarme

- Al configurar un pipeline CI/CD desde cero
- Para auditar la seguridad de pipelines existentes
- Para implementar DevSecOps en el equipo
- Para cumplir con SLSF (Supply-chain Levels for Software Artifacts)

## Prácticas de CI/CD seguro

### 1. Gestión de Secretos
- NUNCA en código ni en variables del pipeline en texto plano
- Usar vaults: GitHub Secrets, GitLab CI Variables, AWS Secrets Manager
- Rotación automática de secretos
- Escaneo de secrets en commits (git-secrets, truffleHog)

### 2. SAST (Static Analysis)
```yaml
# Ejemplo GitHub Actions
- name: SAST Scan
  uses: github/codeql-action/analyze@v3
- name: ESLint Security
  run: npx eslint . --rulesdir eslint-plugin-security
```

### 3. DAST (Dynamic Analysis)
```yaml
- name: DAST Scan
  uses: zaproxy/action-full-scan@v0
  with:
    target: 'https://staging.example.com'
```

### 4. Escaneo de dependencias
```yaml
- name: Dependency Scan
  run: npm audit --audit-level=high
- name: Snyk
  uses: snyk/actions/node@master
```

### 5. Escaneo de imágenes Docker
```yaml
- name: Scan Image
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: 'myapp:${{ github.sha }}'
    severity: 'CRITICAL,HIGH'
```

### 6. Firmado
- Firmar commits con GPG o SSH
- Firmar imágenes Docker con Docker Content Trust (DCT)
- Firmar releases con cosign (sigstore)
- Verificar proveniencia con SLSF

### 7. Pipeline Hardening
- Principio de mínimo privilegio en tokens de CI
- Revisión humana en pasos críticos (deploy a prod)
- Aislamiento de jobs (runners efímeros)
- No cachear secretos entre jobs
- Audit logging de todas las acciones del pipeline
