---
description: Verifica la configuración de seguridad de servidores e infraestructura
agent: hardenizador
model: opencode-go/deepseek-v4-flash
---

Realiza una verificación de hardening en la configuración del proyecto.

1. Busca archivos de configuración: `nginx.conf`, `Dockerfile`, `docker-compose.yml`, `.env.example`
2. Si existe `Dockerfile`, revisa:
   - Usuario no-root
   - Imagen base segura
   - Multi-stage builds
   - Exposición de puertos
3. Si existe configuración de nginx, revisa headers de seguridad
4. Si existe `docker-compose.yml`, revisa seguridad de contenedores
5. Genera un reporte con recomendaciones específicas

Usa `skill("hardening-servidores")` para guías detalladas.
