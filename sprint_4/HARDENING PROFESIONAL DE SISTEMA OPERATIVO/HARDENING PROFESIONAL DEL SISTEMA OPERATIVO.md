HARDENING PROFESIONAL DEL SISTEMA OPERATIVO
N°: GRUPO 1

Integrantes: Bryan Aguilera Nieto - Izan Fernandez Javier - Giuseppe Suarez

Profesores: Sergi - David Sicart

ÍNDICE
CONFIGURACIÓN DE INSTANCIA 1

1. SSH

2. Banner de Acceso

3. Seguridad en SSH

4. UFW (Firewall)

5. Lynis (Auditoría)

CONFIGURACIÓN DE INSTANCIA 2

1. SSH

2. Firewalld

3. Lynis

CONFIGURACIÓN DE INSTANCIA 3

1. SSH

2. Firewalld

3. Lynis

<a name="instancia1"></a>

CONFIGURACIÓN DE INSTANCIA 1
<a name="ssh1"></a>

1. SSH
<a name="banner1"></a>

Banner de Acceso
Primero, activamos el banner editando el archivo de configuración:

Bash

sudo nano /etc/ssh/sshd_config
Buscamos la línea Banner y definimos la ruta: Banner /etc/issue.net.

Para crear el contenido del banner, ejecutamos:

Bash

echo "****************************************************************" | sudo tee /etc/issue /etc/issue.net
echo "ACCESO RESTRINGIDO: Nodo Web Extagram - Solo personal autorizado." | sudo tee -a /etc/issue /etc/issue.net
echo "Toda actividad esta siendo monitoreada y auditada." | sudo tee -a /etc/issue /etc/issue.net
echo "****************************************************************" | sudo tee -a /etc/issue /etc/issue.net
Reiniciamos el servicio:

Bash

sudo systemctl restart ssh
<a name="seguridad-ssh1"></a>

Seguridad en SSH
Editamos /etc/ssh/sshd_config para aplicar las siguientes directivas de seguridad:

LoginGraceTime 30

PermitRootLogin no

MaxAuthTries 3

MaxSessions 2

PasswordAuthentication yes

AllowTcpForwarding no

ClientAliveInterval 300

ClientAliveCountMax 0

Para verificar los cambios realizados:

Bash

sudo grep -E "PermitRootLogin|MaxAuthTries|LoginGraceTime|ClientAlive|AllowTcpForwarding|MaxSessions|PasswordAuthentication" /etc/ssh/sshd_config
<a name="ufw1"></a>

2. UFW (Firewall)
Configuramos las reglas básicas de tráfico:

Bash

sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
<a name="lynis1"></a>

3. Lynis
Instalación y ejecución de la auditoría:

Bash

sudo apt install lynis -y
sudo lynis audit system
<a name="instancia2"></a>

CONFIGURACIÓN DE INSTANCIA 2
<a name="ssh2"></a>

1. SSH
(Misma configuración de Banner y Seguridad que la Instancia 1)

<a name="firewalld2"></a>

2. Firewalld
Instalación e inicio del servicio:

Bash

sudo dnf install firewalld -y
sudo systemctl start firewalld
sudo systemctl enable firewalld
Configuración de reglas en la zona de Docker:

Bash

sudo firewall-cmd --permanent --zone=docker --add-service=ssh
sudo firewall-cmd --permanent --zone=docker --add-service=http
sudo firewall-cmd --permanent --zone=docker --add-service=https
sudo firewall-cmd --permanent --zone=docker --add-service=dns
sudo firewall-cmd --reload
<a name="lynis2"></a>

3. Lynis
Bash

sudo dnf install lynis -y
sudo lynis audit system
<a name="instancia3"></a>

CONFIGURACIÓN DE INSTANCIA 3
<a name="ssh3"></a>

1. SSH
(Configuración idéntica a las instancias anteriores)

<a name="firewalld3"></a>

2. Firewalld
Repetimos el proceso de instalación y apertura de puertos:

Bash

sudo dnf install firewalld -y
sudo systemctl start firewalld
sudo systemctl enable firewalld
sudo firewall-cmd --permanent --zone=docker --add-service=ssh
sudo firewall-cmd --permanent --zone=docker --add-service=http
sudo firewall-cmd --permanent --zone=docker --add-service=https
sudo firewall-cmd --permanent --zone=docker --add-service=dns
sudo firewall-cmd --reload
<a name="lynis3"></a>

3. Lynis
Bash

sudo dnf install lynis -y
sudo lynis audit system
