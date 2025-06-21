# FIREWALL-ZERO
Simular la preparación y despliegue inicial de un servidor expuesto a Internet en una organización real. Cada equipo será responsable de configurar, asegurar y documentar su servidor, que servirá como base para las siguientes fases del ejercicio.
# Proyecto FIREWALL ZERO – Fase 1

## Simulación de entorno vulnerable y fortificación básica

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
