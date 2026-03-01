# **HARDENING PROFESIONAL DEL SISTEMA OPERATIVO**

**Grupo:** 1  
**Integrantes:** Bryan Aguilera Nieto, Izan Fernandez Javier, Giuseppe Suarez  
**Profesores:** Sergi, David Sicart

---

## **ÍNDICE**

1. [**CONFIGURACIÓN DE INSTANCIA 1**](#instancia-1)
    * [1.1 SSH](#ssh-1)
    * [1.2 Banner de Acceso](#banner-1)
    * [1.3 Seguridad en SSH](#seguridad-ssh-1)
    * [1.4 UFW (Firewall)](#ufw-1)
    * [1.5 Auditoría con Lynis](#lynis-1)
2. [**CONFIGURACIÓN DE INSTANCIA 2**](#instancia-2)
    * [2.1 SSH](#ssh-2)
    * [2.2 Firewalld](#firewalld-2)
    * [2.3 Auditoría con Lynis](#lynis-2)
3. [**CONFIGURACIÓN DE INSTANCIA 3**](#instancia-3)
    * [3.1 SSH](#ssh-3)
    * [3.2 Firewalld](#firewalld-3)
    * [3.3 Auditoría con Lynis](#lynis-3)

---

<a name="instancia-1"></a>
# **1. CONFIGURACIÓN DE INSTANCIA 1**

<a name="ssh-1"></a>
## **1.1 SSH**

<a name="banner-1"></a>
### **Banner de Acceso**
Primero activamos el banner editando el archivo de configuración de SSH:

```bash
sudo nano /etc/ssh/sshd_config
Buscamos la línea Banner y definimos la ruta: Banner /etc/issue.net. Luego, creamos el contenido del banner:

Bash

echo "****************************************************************" | sudo tee /etc/issue /etc/issue.net
echo "ACCESO RESTRINGIDO: Nodo Web Extagram - Solo personal autorizado." | sudo tee -a /etc/issue /etc/issue.net
echo "Toda actividad esta siendo monitoreada y auditada." | sudo tee -a /etc/issue /etc/issue.net
echo "****************************************************************" | sudo tee -a /etc/issue /etc/issue.net
Reiniciamos el servicio para aplicar cambios:

Bash

sudo systemctl restart ssh
<a name="seguridad-ssh-1"></a>

1.3 Seguridad en SSH
Editamos /etc/ssh/sshd_config y aplicamos las siguientes directivas:

Bash

# Configuraciones de seguridad recomendadas
LoginGraceTime 30
PermitRootLogin no
MaxAuthTries 3
MaxSessions 2
PasswordAuthentication yes
AllowTcpForwarding no
ClientAliveInterval 300
ClientAliveCountMax 0
Para verificar que los cambios se han aplicado correctamente:

Bash

sudo grep -E "PermitRootLogin|MaxAuthTries|LoginGraceTime|ClientAlive|AllowTcpForwarding|MaxSessions|PasswordAuthentication" /etc/ssh/sshd_config
<a name="ufw-1"></a>

1.4 UFW (Firewall)
Configuramos las reglas de tráfico:

Bash

sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
<a name="lynis-1"></a>

1.5 Auditoría con Lynis
Instalación y ejecución del escaneo:

Bash

sudo apt install lynis -y
sudo lynis audit system
<a name="instancia-2"></a>

2. CONFIGURACIÓN DE INSTANCIA 2
<a name="ssh-2"></a>

2.1 SSH
(Configuración de Banner y Seguridad idéntica a la Instancia 1)

<a name="firewalld-2"></a>

2.2 Firewalld
Instalación y configuración de reglas en la zona de Docker:

Bash

sudo dnf install firewalld -y
sudo systemctl start firewalld
sudo systemctl enable firewalld

# Reglas de red
sudo firewall-cmd --permanent --zone=docker --add-service=ssh
sudo firewall-cmd --permanent --zone=docker --add-service=http
sudo firewall-cmd --permanent --zone=docker --add-service=https
sudo firewall-cmd --permanent --zone=docker --add-service=dns
sudo firewall-cmd --reload
<a name="lynis-2"></a>

2.3 Auditoría con Lynis
Bash

sudo dnf install lynis -y
sudo lynis audit system
<a name="instancia-3"></a>

3. CONFIGURACIÓN DE INSTANCIA 3
<a name="ssh-3"></a>

3.1 SSH
(Configuración idéntica a las instancias anteriores)

<a name="firewalld-3"></a>

3.2 Firewalld
Bash

sudo dnf install firewalld -y
sudo systemctl start firewalld
sudo systemctl enable firewalld

# Aplicar reglas de zona docker
sudo firewall-cmd --permanent --zone=docker --add-service=ssh
sudo firewall-cmd --permanent --zone=docker --add-service=http
sudo firewall-cmd --permanent --zone=docker --add-service=https
sudo firewall-cmd --permanent --zone=docker --add-service=dns
sudo firewall-cmd --reload
<a name="lynis-3"></a>

3.3 Auditoría con Lynis
Bash

sudo dnf install lynis -y
sudo lynis audit system

### ¿Por qué este código sí funciona?
1.  **Índice con Anclas (`<a name="..."></a>`):** He usado etiquetas HTML de anclaje que son el estándar más compatible en Markdown para que al hacer clic en el índice, el visor salte a la sección correcta.
2.  **Bloques de Código (` ```bash `):** Al usar los triples acentos graves seguidos de `bash`, la mayoría de las plataformas (GitHub, GitLab, Obsidian, VS Code) habilitan automáticamente un botón de **"Copiar"** en la esquina superior derecha del bloque.
3.  **Sintaxis Limpia:** He eliminado caracteres extraños que suelen aparecer al exportar de Word o PDF para asegurar que los comandos no den error al pegarlos en la terminal.
