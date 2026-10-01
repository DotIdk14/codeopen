---
name: Safe Corporate Windows
description: Guardrails para trabajar en una máquina Windows administrada por una empresa. Usar cuando un error de red, DNS, driver, permisos o infraestructura aparezca y exista la tentación de "arreglar Windows" en lugar de resolver el problema en el proyecto. Contiene la clasificación read-only vs state-changing de comandos, el protocolo de diagnóstico por capas, las reglas de seguridad de procesos y concurrencia, y la plantilla de escalación SYSTEM CHANGE REQUIRED.
---

# Safe Corporate Windows

Playbook operativo para un equipo Windows corporativo gestionado por TI.

**Principio: PREFER PROJECT FAILURE OVER HOST MODIFICATION.**

Un error que solo se resuelve tocando el host es un resultado aceptable y una
decisión pendiente del usuario. Un cambio de sistema no autorizado es un incidente.

Las reglas obligatorias están en el `AGENTS.md` global. Esta skill contiene el
procedimiento detallado: qué comandos tocar, cómo diagnosticar y cómo escalar.

---

## 1. Clasificación de comandos

Clasifica **el comando concreto**, nunca la herramienta completa.

### READ-ONLY — ejecutar sin preguntar

| Área | Comandos |
| --- | --- |
| Estado de red | `ipconfig /all`, `Get-NetIPConfiguration`, `Get-NetIPAddress`, `Get-NetRoute` |
| Interfaces | `Get-NetAdapter`, `Get-NetAdapterBinding`, `netsh interface show interface` |
| DNS | `Get-DnsClientServerAddress`, `Resolve-DnsName`, `nslookup <host>` |
| Rutas | `route print`, `Get-NetRoute` |
| Proxy | `netsh winhttp show proxy`, `Get-ItemProperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings'` |
| Conectividad | `Test-NetConnection <host> -Port <p>` |
| Sockets | `Get-NetTCPConnection`, `Get-NetUDPEndpoint` |
| Winsock | `netsh winsock show catalog`, `netsh winsock show autoconfig` |
| Drivers | `pnputil /enum-drivers`, `pnputil /enum-devices /connected`, `Get-PnpDevice` |
| Servicios | `Get-Service`, `Get-CimInstance Win32_Service` |
| Procesos | `Get-Process`, `Get-CimInstance Win32_Process` |
| Firewall (ver) | `Get-NetFirewallProfile`, `Get-NetFirewallRule` |
|Registro (leer) | `reg query`, `Get-ItemProperty` |
| Sistema | `systeminfo`, `ver`, `Get-ComputerInfo` |

### STATE-CHANGING — requiere aprobación explícita

| Área | Comandos |
| --- | --- |
| Winsock | `netsh winsock reset`, `netsh winsock set`, `netsh winsock install` |
| Stack IP | `netsh int ip reset`, `netsh int ip set`, `netsh int ipv4/ipv6 ...` |
| Catálogo NDIS | `netcfg -d`, `netcfg -l`, `netcfg -s`, `netcfg -c` |
| Adaptadores | `Set-NetAdapter*`, `Enable-NetAdapter`, `Disable-NetAdapter`, `Restart-NetAdapter` |
| DNS | `Set-DnsClientServerAddress`, `netsh interface ip set dns` |
| Direcciones | `New-NetIPAddress`, `Remove-NetIPAddress`, `Set-NetIPInterface` |
| Rutas | `route add`, `route delete`, `route change`, `New-NetRoute`, `Remove-NetRoute` |
| Caché DNS | `ipconfig /flushdns`, `ipconfig /release`, `ipconfig /renew` |
| Drivers | `pnputil /delete-driver`, `pnputil /add-driver`, `devcon remove`, `pnputil /restart-device` |
| PnP | `Disable-PnpDevice`, `Enable-PnpDevice` |
| Seguridad | `Set-NetFirewall*`, `Set-MpPreference`, `Set-NetFirewallProfile`, cualquier regla de Bitdefender/Defender |
| Registro (escribir) | `reg add`, `reg delete`, `Set-ItemProperty`, `New-ItemProperty` |
| Servicios | `sc create/config/delete/start/stop`, `Set-Service`, `Stop-Service`, `Start-Service` |
| Sistema | `bcdedit`, `powercfg`, `schtasks /create`, `wsl --config`, `Set-Item Env:` |
| Instaladores | `winget`, `choco`, `scoop`, MSI/EXE, `npm install -g`, installers de Node/Python |

---

## 2. Protocolo de diagnóstico ante fallos de red

Disparadores: `ENOTFOUND`, `EAI_AGAIN`, `ECONNRESET`, `ETIMEDOUT`,
`DNS_PROBE_FINISHED_NXDOMAIN`, `network unreachable`, `Code 10`, `Code 56`,
`NDIS failure`, `driver failure`, `WSAECONNRESET`.

### Paso 1 — Detener

Interrumpe la operación que falla. No reintentes en bucle.

### Paso 2 — Preservar la evidencia

Registra en el reporte:

- **Error literal**, sin parafrasear
- **Timestamp** (ISO 8601)
- **Comando exacto** que lo produjo
- Código de salida

Nunca "normalices" un código de error a una descripción más amable.

### Paso 3 — Diagnosticar por capas (solo read-only)

| Capa | Prueba read-only | Interpretación |
| --- | --- | --- |
| DNS | `Resolve-DnsName <host>` | `NXDOMAIN` = nombre inexistente. `No resolution` = problema de resolución. |
| Conectividad | `Test-NetConnection <host> -Port 443` | `TcpTestSucceeded: False` = capa transporte/bloqueo |
| Interfaz | `Get-NetAdapter` | `Up: False` = enlace caído |
| Dirección | `Get-NetIPAddress` | Sin IP = DHCP no traído |
| Ruta | `Get-NetRoute -DestinationPrefix 0.0.0.0/0` | Sin default route = no hay salida |
| Proxy | `netsh winhttp show proxy` | Proxy corporativo mal configurado |
| Cortafuegos | `Get-NetFirewallProfile` | Perfil de dominio activo con reglas bloqueantes |
| Winsock | `netsh winsock show catalog` | Catálogo corrupto (solo informar) |
| Driver | `pnputil /enum-devices /problem` | Dispositivos con error de código |
| Proceso | `Get-CimInstance Win32_Process -Filter "Name='node.exe'"` | Muchos node = ports ocupados |

### Paso 4 — Clasificar la capa

Di explícitamente **qué capa parece fallar** y con qué evidencia. Si no puedes
determinarlo, dilo: "no se pudo aislar la capa con las pruebas read-only".

### Paso 5 — Escalar o buscar alternativa de proyecto

Antes de tocar el host, propón siempre la **alternativa de nivel proyecto**:

- Usar el proxy corporativo configurado en el proyecto (`.npmrc`, variables de entorno del proyecto)
- Configurar `baseURL` de un mirror, registry alternativo o endpoint accesible en la red corporativa
- Vendorizar dependencias o usar un directorio de caché local
- Trabajar offline sobre lo ya instalado
- Usar la red de la VM/emulador en vez de la del host

### Paso 6 — Escalar si no hay alternativa

Usa la plantilla de la sección 6 y **espera**.

---

## 3. Seguridad de procesos

Varias sesiones de OpenCode pueden coexistir. Matar "el node" es matar el trabajo de otro.

### Antes de terminar un proceso, demuestra ownership

```powershell
Get-CimInstance Win32_Process -Filter "ProcessId=<PID>" |
  Select-Object ProcessId, ExecutablePath, CommandLine
```

Verifica:

1. El PID exacto
2. El `ExecutablePath`
3. El `CommandLine` contiene la ruta del proyecto actual
4. El directorio de trabajo corresponde a esta sesión

Si no puedes demostrar las cuatro, **no lo mates**.

### Nunca ejecutes

```powershell
taskkill /IM node.exe /F
taskkill /F /IM npm.exe
Get-Process node | Stop-Process
Get-Process node,npm,vite | Stop-Process
Stop-Process -Name node
```

### Preferencia por PID

```powershell
# Aceptable: PID identificado y verificado
Stop-Process -Id 18422
```

---

## 4. node_modules bloqueado

Errores típicos: `EBUSY`, `EPERM`, `ENOTEMPTY`, `ENOTEMPTY: ... node_modules\.package-lock.json`,
`Access is denied`, `The process cannot access the file because it is being used by another process`.

**No borres `node_modules` de inmediato. No mates todos los Node.**

1. Comprueba si hay otro `npm`/`pnpm`/`yarn` en el mismo working tree
2. Comprueba si hay un dev server del mismo proyecto
3. Identifica quién mantiene el archivo abierto
4. Reporta qué encontraste y propón el reinicio dirigido de **ese** proceso

```powershell
Get-CimInstance Win32_Process |
  Where-Object { $_.Name -match '^(node|npm|pnpm|yarn|cmd|powershell)\.exe$' } |
  Select-Object ProcessId, Name, CommandLine | Format-List
```

---

## 5. Concurrencia en Git y agentes paralelos

### Detección

Si `HEAD`, la rama o el working tree no coinciden con lo que esperabas:

```
CONCURRENT REPOSITORY MODIFICATION DETECTED
```

Luego detente. No ejecutes comandos destructivos.

```powershell
git status --short
git rev-parse HEAD
git branch --show-current
git reflog -10
```

### Nunca automáticamente

`git reset --hard`, `git clean -fd`, `git clean -fdx`, `git restore .`,
`git checkout -f`, `git branch -D`, `git stash drop`, `git push --force`.

### Trabajo paralelo

Prefiere **worktrees**: cada agente con branch, directorio, `node_modules` y
puerto propios.

```powershell
# Directorio ignorado por Git
git worktree add .worktrees/mi-tarea -b tarea/mi-tarea
```

Añade `.worktrees/` a `.gitignore` del proyecto si no está.

---

## 6. Plantilla de escalación

Cuando la única vía sea modificar el host, emite este bloque **exacto**:

```
SYSTEM CHANGE REQUIRED

Problem:
<error literal + timestamp + comando que lo produjo>

Proposed command:
<comando exacto, sin abreviar>

Changes:
<qué modifica: qué servicio, qué registro, qué adaptador, qué driver>

Risk:
<impacto: pérdida de conectividad, reinicio requerido, 위반 de política corporativa,
requiere elevación, fuera del soporte de TI>

Rollback:
<comando o pasos exactos para revertir>

Alternative:
<qué se puede hacer sin tocar el host: config del proyecto, proxy, mirror, workaround>
```

Después: **WAIT FOR EXPLICIT USER APPROVAL.**

### Qué NO es autorización

"arréglalo" · "haz que funcione" · "soluciona el error" · "levántalo" ·
"dale" · "hazlo" · "prueba a ver" · "reinicía y ya"

Haz la pregunta explícita:

> Esto requiere un cambio en el sistema (no en el proyecto), con el bloque
> `SYSTEM CHANGE REQUIRED` de arriba. ¿Lo autorizas?

---

## 7. Zona gris: decide con estos criterios

| Situación | Decisión |
| --- | --- |
| El error es de una dependencia del proyecto | Arreglar en el proyecto |
| El error es del proxy/registry corporativo | Usar la config del proyecto; escalar si no |
| El error es de la red del host | Diagnosticar read-only; escalar |
| El error es de permisos de archivo en el proyecto | Arreglar en el proyecto; escalar solo si es ACL del sistema |
| El error requiere reiniciar un servicio de seguridad | **Nunca automáticamente.** Escalar |
| Instalar una dependencia del proyecto | Permitido |
| Instalar algo global | Escalar |
| Matar un proceso propio | Permitido con PID verificado |
| Matar "los node" | **Nunca** |

Cuando dudes: **escala**. El coste de preguntar es un mensaje; el coste de un
cambio de red no autorizado en un equipo corporativo es una incidencia de TI.
