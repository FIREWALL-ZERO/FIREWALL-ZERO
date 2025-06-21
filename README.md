## Simulación de entorno vulnerable y fortificación básica
Simular la preparación y despliegue inicial de un servidor expuesto a Internet en una organización real. Cada equipo será responsable de configurar, asegurar y documentar su servidor, que servirá como base para las siguientes fases del ejercicio.
# Proyecto FIREWALL ZERO – Fase 1

---

### Equipo técnico  
- Freddy Alexis Vegas Gil  
- Natalia Kucherenko Sheverova  
- Carlos Lopez Villa  

**Fecha:** 22-06-2025  
**Empresa:** Socistar Caribe S.L.

---

## Descripción

Este proyecto consiste en la configuración inicial y fortificación básica de un servidor Ubuntu 24.04 con el objetivo de simular un entorno vulnerable controlado y asegurar los servicios más críticos, aplicando buenas prácticas de seguridad y monitoreo.

---

## Detalles del servidor

- **Tipo:** VPS 1 1 10  
- **Sistema operativo:** Ubuntu Server 24.04  
- **IP pública:** 217.160.4.23  

---

## Tecnologías y herramientas usadas

- Ubuntu Server 24.04  
- Firewall UFW y reglas iptables  
- Apache2  
- Fail2Ban para protección SSH  
- Lynis para auditoría de seguridad  

---

## Configuraciones principales

### Firewall (UFW)

- Política por defecto: denegar todo tráfico entrante y permitir saliente  
- Puertos abiertos:  
  - SSH en puerto 2222 (cambiado por seguridad)  
  - HTTP (80/tcp)  
  - HTTPS (443/tcp) (opcional)  

### SSH

- Puerto cambiado de 22 a 2222  
- Fail2Ban configurado para proteger contra intentos de fuerza bruta  

### Apache2

- Instalado y configurado para escuchar en puerto 80  

### Auditoría de seguridad

- Herramienta Lynis usada para auditoría básica y detección de vulnerabilidades  

---

## Comandos clave

```bash
# Firewall UFW
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2222/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable

# Cambiar puerto SSH
sudo nano /etc/ssh/sshd_config  # Cambiar puerto a 2222
sudo systemctl restart ssh

# Fail2Ban
sudo apt install fail2ban
sudo systemctl enable fail2ban
sudo systemctl start fail2ban

# Ver estado Fail2Ban
sudo fail2ban-client status sshd

# Desbloquear IP en Fail2Ban
sudo fail2ban-client set sshd unbanip <IP>

# Instalación Apache
sudo apt install apache2
sudo systemctl start apache2
sudo systemctl enable apache2

# Lynis auditoría
sudo apt install lynis
sudo lynis audit system


# Proyecto Aplicación Vulnerable con Django

## 2. Instalación de un paquete de aplicaciones vulnerables

### ¿Por qué Django es ideal para esto?

- Tiene un sistema de autenticación robusto (users, sesiones, permisos, etc.).
- Su ORM previene inyección SQL por defecto.
- Incluye protecciones contra CSRF, XSS y clickjacking por diseño.
- Muy modular y extensible, ideal para apps tipo SaaS.
- Puedes organizar la seguridad desde la configuración (`settings.py`) hasta las vistas, middlewares, etc.

---

### Seguridad basada en el OWASP Top 10

| OWASP Top 10                             | Cómo lo cubres en Django                                                                                   |
|----------------------------------------|------------------------------------------------------------------------------------------------------------|
| A01 – Broken Access Control             | `@login_required`, permisos de grupos, `has_perm`, lógica en views.                                        |
| A02 – Cryptographic Failures            | Usa `pbkdf2` (Django por defecto), HTTPS con `SECURE_SSL_REDIRECT`.                                        |
| A03 – Injection                        | El ORM de Django evita SQL injection. Usa `QuerySet` y no `.raw()`.                                        |
| A04 – Insecure Design                  | Valida entradas, sanitiza archivos subidos, define lógica de negocio segura.                               |
| A05 – Security Misconfiguration        | Configura `ALLOWED_HOSTS`, `SECURE_HSTS_SECONDS`, `DEBUG=False`, etc.                                     |
| A06 – Vulnerable Components             | Usa `pip-audit`, `requirements.txt` actualizado, virtualenvs.                                             |
| A07 – Identification and Authentication Failures | Usa `User`, MFA opcional, `SESSION_COOKIE_SECURE`, límites de sesión.                                       |
| A08 – Software and Data Integrity Failures | Firmado de datos con `django.signing`, verificación de integridad.                                         |
| A09 – Security Logging and Monitoring Failures | Logs con `logging`, integración con SIEM (opcional).                                                       |
| A10 – SSRF                             | Bloquea IP internas, válida URLs, no permitas redirecciones sin control.                                  |

---

### Nota

De todas las vulnerabilidades, usaremos solo:

- El ORM de Django evita SQL injection. Usa `QuerySet` y no `.raw()`.
- Configura `ALLOWED_HOSTS`, `SECURE_HSTS_SECONDS`, `DEBUG=False`, etc.
- Uso de `path` y `reverse` para URLs.

---

### ¿Se puede hacer vulnerable para enseñar auditoría?

Sí. Una vez que el proyecto esté construido de forma segura, puedes:

- Eliminar middleware de CSRF → vulnerable a CSRF.
- Usar `eval()` o `.raw()` → vulnerable a ejecución de código o SQL injection.
- Insertar campos ocultos mal gestionados → IDOR o acceso no autorizado.
- Exponer información sensible en mensajes de error → información leakage.
- No validar archivos → subida de shells, RCE.

---

### ¿Qué incluiría el proyecto?

- Login, logout, registro con roles (candidato/reclutador).
- Carga de portfolios (imagen, texto, links).
- Buscador de empleos.
- Panel de control para ambos tipos de usuarios.
- Protección OWASP Top 10.
- Versión insegura (copiada o en rama separada) para practicar pentesting.

---

## Instalación desde 0

### Crear un nuevo usuario

```bash
adduser falexis
usermod -aG sudo falexis

