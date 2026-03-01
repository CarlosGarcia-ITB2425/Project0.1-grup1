HARDENING PROFESIONAL DEL SISTEMA OPERATIVOBlindando la infraestructura de Extagram en AWS N°: GRUPO 1 Integrantes: Bryan Aguilera Nieto - Izan Fernandez Javier - Giuseppe Suarez Profesores: Sergi - David Sicart ÍNDICECONFIGURACIÓN DE INSTANCIA 11.1 SSH1.2 Banner de Acceso1.3 Seguridad en SSH1.4 UFW (Firewall)1.5 Lynis (Auditoría)CONFIGURACIÓN DE INSTANCIA 22.1 SSH2.2 Firewalld2.3 LynisCONFIGURACIÓN DE INSTANCIA 33.1 SSH3.2 Firewalld3.3 Lynis<a name="instancia1"></a>1. CONFIGURACIÓN DE INSTANCIA 1 <a name="ssh1"></a>1.1 SSH <a name="banner1"></a>1.2 Banner de Acceso Para activarlo, editamos el archivo de configuración y buscamos la línea Banner:Bashsudo nano /etc/ssh/sshd_config
Definimos la ruta: Banner /etc/issue.net. Para crear el contenido del banner:Bashecho "****************************************************************" | sudo tee /etc/issue /etc/issue.net
echo "ACCESO RESTRINGIDO: Nodo Web Extagram - Solo personal autorizado." | sudo tee -a /etc/issue /etc/issue.net
echo "Toda actividad esta siendo monitoreada y auditada." | sudo tee -a /etc/issue /etc/issue.net
echo "****************************************************************" | sudo tee -a /etc/issue /etc/issue.net
Reiniciamos el servicio:Bashsudo systemctl restart ssh
<a name="seguridad1"></a>1.3 Seguridad en SSH Modificamos los siguientes parámetros en /etc/ssh/sshd_config:LoginGraceTime 30: 30 segundos para iniciar sesión.PermitRootLogin no: Impide acceso directo como root.MaxAuthTries 3: Desconexión tras 3 fallos.MaxSessions 2: Máximo de sesiones simultáneas.PasswordAuthentication yes: (Opcional según requerimiento).AllowTcpForwarding no: Bloquea "puentes" entre servidores.ClientAliveInterval 300: Aviso de actividad cada 5 min.ClientAliveCountMax 0: Cierre inmediato si no hay respuesta al aviso.Verificación de cambios:Bashsudo grep -E "PermitRootLogin|MaxAuthTries|LoginGraceTime|ClientAlive|AllowTcpForwarding|MaxSessions|PasswordAuthentication" /etc/ssh/sshd_config
<a name="ufw1"></a>1.4 UFW (Firewall) Configuración de reglas por defecto y apertura de puertos:Bashsudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
<a name="lynis1"></a>1.5 Lynis Instalación y auditoría:Bashsudo apt install lynis -y
sudo lynis audit system
Resultado: Hardening index 72.<a name="instancia2"></a>2. CONFIGURACIÓN DE INSTANCIA 2 <a name="ssh2"></a>2.1 SSH (Misma configuración de Banner y Seguridad que Instancia 1) Bashsudo nano /etc/ssh/sshd_config
sudo systemctl restart sshd
<a name="firewalld2"></a>2.2 Firewalld Instalación e inicio:Bashsudo dnf install firewalld -y
sudo systemctl start firewalld
sudo systemctl enable firewalld
Configuración de reglas en zona docker:Bashsudo firewall-cmd --permanent --zone=docker --add-service=ssh
sudo firewall-cmd --permanent --zone=docker --add-service=http
sudo firewall-cmd --permanent --zone=docker --add-service=https
sudo firewall-cmd --permanent --zone=docker --add-service=dns
sudo firewall-cmd --reload
<a name="lynis2"></a>2.3 Lynis Bashsudo apt yum lynis
sudo lynis audit system
<a name="instancia3"></a>3. CONFIGURACIÓN DE INSTANCIA 3 <a name="ssh3"></a>3.1 SSH(Configuración idéntica a las instancias anteriores) Bashsudo nano /etc/ssh/sshd_config
sudo systemctl restart sshd
<a name="firewalld3"></a>3.2 Firewalld Bashsudo dnf install firewalld -y
sudo systemctl start firewalld
sudo systemctl enable firewalld
sudo firewall-cmd --permanent --zone=docker --add-service=ssh
sudo firewall-cmd --permanent --zone=docker --add-service=http
sudo firewall-cmd --permanent --zone=docker --add-service=https
sudo firewall-cmd --permanent --zone=docker --add-service=dns
sudo firewall-cmd --reload
<a name="lynis3"></a>3.3 Lynis Bashsudo dnf install lynis -y
sudo lynis audit system
