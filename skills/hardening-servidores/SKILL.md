---
name: hardening-servidores
description: Hardening de infraestructura web: servidores, contenedores, cloud, headers HTTP, TLS, CSP
license: MIT
compatibility: opencode
metadata:
  area: infraestructura
  prioridad: alta
---

## Qué hago

Hardening completo de infraestructura para aplicaciones web: servidores Linux, contenedores Docker, cloud (AWS/GCP/Azure), y configuración de seguridad perimetral.

## Cuándo usarme

- Antes de poner un servidor en producción
- Para auditar la configuración de seguridad de infraestructura existente
- Para configurar nginx, Docker, Kubernetes de forma segura
- Para implementar CSP, HSTS y demás headers de seguridad

## Hardening por capas

### 1. Sistema Operativo (Linux)
- Usuario no-root para la aplicación
- SSH: solo key-based, puerto no estándar, fail2ban
- Firewall: UFW/iptables, solo puertos necesarios
- Automatic security updates (unattended-upgrades)
- SELinux/AppArmor habilitado
- /tmp montado con noexec,nosuid

### 2. Servidor Web (nginx)
```
server {
    listen 443 ssl http2;
    ssl_protocols TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 1h;

    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains" always;
    add_header X-Frame-Options "DENY" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    add_header Content-Security-Policy "default-src 'self'" always;

    client_body_buffer_size 10K;
    client_header_buffer_size 1k;
    client_max_body_size 8m;
    large_client_header_buffers 2 1k;
}
```

### 3. Docker
- Usar imágenes oficiales y escaneadas
- USER no-root en Dockerfile
- READ_ONLY root filesystem cuando sea posible
- No privileged containers
- Resource limits (memory, CPU)
- Drop capabilities innecesarias: `--cap-drop=ALL --cap-add=NET_BIND_SERVICE`
- Usar redes bridge personalizadas
- Secrets montados como volúmenes, no env vars

### 4. TLS/SSL
- TLS 1.3 mínimo, TLS 1.2 como fallback
- Certificados de Let's Encrypt con renovación automática
- Perfect Forward Secrecy (DHE/ECDHE)
- OCSP Stapling
- HSTS con preload

### 5. Cloud (AWS/GCP/Azure)
- Security Groups / Firewall Rules restrictivos
- S3 Buckets: bloqueados, no públicos, cifrados
- IAM: mínimo privilegio, roles sobre usuarios
- WAF + CDN (CloudFront, Cloudflare)
- VPC privada con NAT Gateway
- Logging: CloudTrail, VPC Flow Logs
