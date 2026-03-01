# HARDENING PROFESIONAL DEL SISTEMA OPERATIVO

**Grupo 1**  
**Integrantes:** Bryan Aguilera Nieto – Izan Fernandez – Javier – Giuseppe Suarez  
**Profesores:** Sergi – David Sicart  

---

# CONFIGURACION DE INSTANCIA 1

## 1. SSH

### a. Banner de Acceso

Primero debemos activarlo editando el archivo de configuracion y buscando la linea Banner:

```bash
sudo nano /etc/ssh/sshd_config

Quitamos la # y dejamos la linea asi:

Banner /etc/issue.net

Para crear el contenido del banner:

echo "****************************************************************" | sudo tee /etc/issue /etc/issue.net
echo "ACCESO RESTRINGIDO: Nodo Web Extagram - Solo personal autorizado." | sudo tee -a /etc/issue /etc/issue.net
echo "Toda actividad esta siendo monitoreada y auditada." | sudo tee -a /etc/issue /etc/issue.net
echo "****************************************************************" | sudo tee -a /etc/issue /etc/issue.net

Reiniciamos el servicio SSH:

sudo systemctl restart ssh
b. Seguridad en SSH

Editamos el archivo:

sudo nano /etc/ssh/sshd_config

Configuraciones aplicadas:

LoginGraceTime 30
PermitRootLogin no
MaxAuthTries 3
MaxSessions 2
PasswordAuthentication yes
AllowTcpForwarding no
ClientAliveInterval 300
ClientAliveCountMax 0

Comprobamos configuracion:

sudo grep -E "PermitRootLogin|MaxAuthTries|LoginGraceTime|ClientAlive|AllowTcpForwarding|MaxSessions|PasswordAuthentication" /etc/ssh/sshd_config
2. UFW

Configuracion del firewall:

sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable

El mensaje:

Command may disrupt existing ssh connections. Proceed?

Indica que podriamos perder la conexion SSH si no esta permitido el puerto 22.

3. Lynis

Instalacion:

sudo apt install lynis

Auditoria del sistema:

sudo lynis audit system

Tras la auditoria se obtuvo una puntuacion aproximada de 72/100, indicando un buen nivel de hardening.

CONFIGURACION DE INSTANCIA 2
1. SSH

Editar configuracion:

sudo nano /etc/ssh/sshd_config
a. Banner de Acceso

Activamos la linea:

Banner /etc/issue.net

Creamos el banner:

echo "****************************************************************" | sudo tee /etc/issue /etc/issue.net
echo "ACCESO RESTRINGIDO: Nodo Web Extagram - Solo personal autorizado." | sudo tee -a /etc/issue /etc/issue.net
echo "Toda actividad esta siendo monitoreada y auditada." | sudo tee -a /etc/issue /etc/issue.net
echo "****************************************************************" | sudo tee -a /etc/issue /etc/issue.net

Reiniciamos:

sudo systemctl restart sshd
b. Seguridad en SSH

Configuracion aplicada:

LoginGraceTime 30
PermitRootLogin no
MaxAuthTries 3
MaxSessions 2
PasswordAuthentication yes
AllowTcpForwarding no
ClientAliveInterval 300
ClientAliveCountMax 0

Comprobacion:

sudo grep -E "PermitRootLogin|MaxAuthTries|LoginGraceTime|ClientAlive|AllowTcpForwarding|MaxSessions|PasswordAuthentication" /etc/ssh/sshd_config
2. Firewalld
Instalacion e inicio
sudo dnf install firewalld -y
sudo systemctl start firewalld
sudo systemctl enable firewalld
3. Configuracion de Reglas

Trabajamos en zona docker para segmentacion.

Permitir SSH:

sudo firewall-cmd --permanent --zone=docker --add-service=ssh

Permitir HTTP y HTTPS:

sudo firewall-cmd --permanent --zone=docker --add-service=http
sudo firewall-cmd --permanent --zone=docker --add-service=https

Permitir DNS:

sudo firewall-cmd --permanent --zone=docker --add-service=dns

Aplicar cambios:

sudo firewall-cmd --reload
4. Lynis

Instalacion:

sudo dnf install lynis -y

Auditoria:

sudo lynis audit system
CONFIGURACION DE INSTANCIA 3
1. SSH

Editar archivo:

sudo nano /etc/ssh/sshd_config
a. Banner de Acceso

Activamos:

Banner /etc/issue.net

Crear banner:

echo "****************************************************************" | sudo tee /etc/issue /etc/issue.net
echo "ACCESO RESTRINGIDO: Nodo Web Extagram - Solo personal autorizado." | sudo tee -a /etc/issue /etc/issue.net
echo "Toda actividad esta siendo monitoreada y auditada." | sudo tee -a /etc/issue /etc/issue.net
echo "****************************************************************" | sudo tee -a /etc/issue /etc/issue.net

Reiniciar:

sudo systemctl restart sshd
b. Seguridad en SSH

Configuracion:

LoginGraceTime 30
PermitRootLogin no
MaxAuthTries 3
MaxSessions 2
PasswordAuthentication yes
AllowTcpForwarding no
ClientAliveInterval 300
ClientAliveCountMax 0

Comprobacion:

sudo grep -E "PermitRootLogin|MaxAuthTries|LoginGraceTime|ClientAlive|AllowTcpForwarding|MaxSessions|PasswordAuthentication" /etc/ssh/sshd_config
2. Firewalld

Instalacion:

sudo dnf install firewalld -y

Inicio y habilitacion:

sudo systemctl start firewalld
sudo systemctl enable firewalld
3. Configuracion de Reglas

Permitir SSH:

sudo firewall-cmd --permanent --zone=docker --add-service=ssh

Permitir HTTP y HTTPS:

sudo firewall-cmd --permanent --zone=docker --add-service=http
sudo firewall-cmd --permanent --zone=docker --add-service=https

Permitir DNS:

sudo firewall-cmd --permanent --zone=docker --add-service=dns

Aplicar cambios:

sudo firewall-cmd --reload
4. Lynis

Instalacion:

sudo dnf install lynis -y

Auditoria:

sudo lynis audit system
CONCLUSION

Se ha aplicado hardening en tres instancias mediante:

Configuracion avanzada de SSH

Implementacion de firewall (UFW / Firewalld)

Auditoria de seguridad con Lynis
