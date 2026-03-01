# HARDENING PROFESIONAL DEL SISTEMA OPERATIVO

**Autor:** Giuseppe Suarez  
**Curso:** ASIX – Ciberseguridad  

---

## INDICE

- [CONFIGURACION DE INSTANCIA 1](#configuracion-de-instancia-1)
  - [SSH](#ssh)
    - [Banner de Acceso](#banner-de-acceso)
    - [Seguridad en SSH](#seguridad-en-ssh)
  - [UFW](#ufw)
  - [Lynis](#lynis)

- [CONFIGURACION DE INSTANCIA 2](#configuracion-de-instancia-2)
  - [SSH](#ssh-1)
    - [Banner de Acceso](#banner-de-acceso-1)
    - [Seguridad en SSH](#seguridad-en-ssh-1)
  - [Firewalld](#firewalld)
  - [Lynis](#lynis-1)

- [CONFIGURACION DE INSTANCIA 3](#configuracion-de-instancia-3)
  - [SSH](#ssh-2)
    - [Banner de Acceso](#banner-de-acceso-2)
    - [Seguridad en SSH](#seguridad-en-ssh-2)
  - [Firewalld](#firewalld-1)
  - [Lynis](#lynis-2)

---

# CONFIGURACION DE INSTANCIA 1

## SSH

SSH permite la administracion remota segura del servidor mediante cifrado.

### Instalacion

```bash
sudo apt install openssh-server
Comprobacion del servicio
sudo systemctl status ssh
Banner de Acceso

Editar archivo:

sudo nano /etc/issue.net

Contenido ejemplo:

ACCESO RESTRINGIDO
Sistema monitorizado.
Solo personal autorizado.

Configurar en SSH:

sudo nano /etc/ssh/sshd_config

Añadir:

Banner /etc/issue.net

Reiniciar servicio:

sudo systemctl restart ssh
Seguridad en SSH

Editar:

sudo nano /etc/ssh/sshd_config

Configuraciones aplicadas:

PermitRootLogin no
Port 2222
MaxAuthTries 3
PasswordAuthentication no

Reiniciar:

sudo systemctl restart ssh
UFW

Activar firewall:

sudo ufw enable

Permitir SSH personalizado:

sudo ufw allow 2222/tcp

Permitir HTTP y HTTPS:

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

Ver reglas:

sudo ufw status verbose
Lynis

Instalacion:

sudo apt install lynis

Auditoria:

sudo lynis audit system

Lynis revisa:

Configuracion SSH

Firewall

Permisos

Servicios activos

Vulnerabilidades

CONFIGURACION DE INSTANCIA 2
SSH

Instalacion:

sudo dnf install openssh-server

Activar servicio:

sudo systemctl enable sshd
sudo systemctl start sshd
Banner de Acceso

Editar:

sudo nano /etc/issue.net

Configurar en:

sudo nano /etc/ssh/sshd_config

Añadir:

Banner /etc/issue.net

Reiniciar:

sudo systemctl restart sshd
Seguridad en SSH
PermitRootLogin no
Port 2222
MaxAuthTries 3
PasswordAuthentication no

Reiniciar servicio:

sudo systemctl restart sshd
Firewalld

Activar:

sudo systemctl enable firewalld
sudo systemctl start firewalld

Permitir puerto SSH:

sudo firewall-cmd --permanent --add-port=2222/tcp

Permitir HTTP y HTTPS:

sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https

Recargar:

sudo firewall-cmd --reload

Ver reglas:

sudo firewall-cmd --list-all
Lynis

Instalacion:

sudo dnf install lynis

Auditoria:

sudo lynis audit system
CONFIGURACION DE INSTANCIA 3
SSH

Instalacion:

sudo apt install openssh-server

Activar:

sudo systemctl enable ssh
sudo systemctl start ssh
Banner de Acceso

Editar:

sudo nano /etc/issue.net

Añadir en sshd_config:

Banner /etc/issue.net

Reiniciar:

sudo systemctl restart ssh
Seguridad en SSH
PermitRootLogin no
Port 2222
MaxAuthTries 3
PasswordAuthentication no

Reiniciar:

sudo systemctl restart ssh
Firewalld

Instalacion:

sudo apt install firewalld

Activar:

sudo systemctl enable firewalld
sudo systemctl start firewalld

Permitir puertos:

sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https

Recargar:

sudo firewall-cmd --reload
Lynis

Instalacion:

sudo apt install lynis

Auditoria:

sudo lynis audit system
