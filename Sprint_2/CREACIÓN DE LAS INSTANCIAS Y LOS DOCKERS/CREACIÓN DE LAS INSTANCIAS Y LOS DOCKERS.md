**CREACIÓN DE LAS INSTANCIAS Y LOS DOCKERS**

1. **Despliegue de instancias EC2 con Elastic IPs**

**Creación de las instancias**

Instancia S1 (Proxy):  
**t3.micro con Ubuntu 24.04 LTS.**

Instancia S2 \- S6 (App):  
**t3.small (Minimo 2GB de RAM para poder correr PHP/Nginx cómodamente)**

Instancia S7 (Database):  
**t3.micro**

![][image1]

**Configuración de las Elastic IPs**

![][image2]

**IP publica para cada instancia**  
**![][image3]**

**Asociamos cada IP elástica a su instancia:**  
**![][image4]**

**![][image5]**

**2\. Configuración de Security Groups (Firewall)**

**SG-Proxy (S1)**

- Permitir Entrada: Puerto 80 (HTTP) desde 0.0.0.0/0 (todo el mundo).  
- Permitir Entrada: Puerto 22 (SSH) desde tu IP para gestión.  
- Permitir Entrada: ICMP desde 0.0.0.0/0 (todo el mundo) para poder hacer ping.

- Permitir Todo paquete como regla de **salida**

| ![][image6] |
| :---- |

**SG-App (S2-S6)**

- Permitir Entrada: Puertos 8080-8085 solo desde el ID del grupo de seguridad SG-Proxy.  
- Permitir Entrada: Puerto 22 (SSH) desde tu IP.  
- Permitir Entrada: ICMP desde 0.0.0.0/0 (todo el mundo) para poder hacer ping.

- Permitir Todo paquete como regla de **salida**

| ![][image7] |
| :---- |

**SG-DB (S7)**

- Permitir Entrada: Puerto 3306 (MySQL) solo desde el ID del grupo de seguridad SG-App  
- Permitir Entrada: ICMP desde 0.0.0.0/0 (todo el mundo) para poder hacer ping.

- Permitir Todo paquete como regla de **salida**

| ![][image8] |
| :---- |

**Grupos de seguridad:**

| ![][image9] |
| :---- |

**Asignamos los grupos de seguridad a sus respectivas instancias**

| ![][image10] S1: ![][image11] S2-S6: ![][image12] S3: ![][image13]  |
| :---- |

**Creación de los dockers**

He de conectarme por SSH a cada una de las maquinas y ejecutar los siguientes comandos.  
S1:

Comando para actualizar el sistema:  
```bash

sudo apt update && sudo apt upgrade \-y
```
Comando para instalar Docker y Docker-compose:
```bash  
sudo apt install \-y docker.io docker-compose
```
Permitir que el usuario local pueda ejecutar docker sin ‘sudo’:
```bash
sudo usermod \-aG docker $USER
```
Comando para habilitar Docker al arranque:
```bash 
sudo systemctl enable docker
```
S2-S6 | S7:

Comando para actualizar el sistema:
```bash
sudo dnf update \-y
```
Comando para instalar Docker:
```bash
sudo dnf install docker \-y
```
Comandos para instalar Docker compose (Plugin):
```bash
sudo curl \-L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname \-s)-$(uname \-m)" \-o /usr/local/bin/docker-compose  
```

```bash
sudo chmod \+x /usr/local/bin/docker-compose  
```
```bash
sudo ln \-s /usr/local/bin/docker-compose /usr/bin/docker-compose  
```


Comando para iniciar el servicio Docker:  
```bash
sudo systemctl start docker
```
Comando para habilitar que Docker arranque solo al encender la instancia:  
```bash
sudo systemctl enable docker
```
Permitir que el usuario local pueda ejecutar docker sin ‘sudo’:  
```bash
sudo usermod \-aG docker $USER
```
