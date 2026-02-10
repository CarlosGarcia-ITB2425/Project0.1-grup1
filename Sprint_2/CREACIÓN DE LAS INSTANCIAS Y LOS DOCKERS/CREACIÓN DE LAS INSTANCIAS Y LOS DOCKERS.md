# **CREACIÓN DE LAS INSTANCIAS Y LOS DOCKERS**
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-50-13.png)
## 1.Despliegue de instancias EC2 con Elastic IPs

**Creación de las instancias**

Instancia S1 (Proxy):  
**t3.micro con Ubuntu 24.04 LTS.**

Instancia S2 \- S6 (App):  
**t3.small (Minimo 2GB de RAM para poder correr PHP/Nginx cómodamente)**

Instancia S7 (Database):  
**t3.micro**

![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-50-21.png)

**Configuración de las Elastic IPs**

![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-50-25.png)

**IP publica para cada instancia**  
**![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-50-28.png)**

**Asociamos cada IP elástica a su instancia:**  
**![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-50-36.png)**

**![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-50-41.png)**

**2\. Configuración de Security Groups (Firewall)**

**SG-Proxy (S1)**

- Permitir Entrada: Puerto 80 (HTTP) desde 0.0.0.0/0 (todo el mundo).  
- Permitir Entrada: Puerto 22 (SSH) desde tu IP para gestión.  
- Permitir Entrada: ICMP desde 0.0.0.0/0 (todo el mundo) para poder hacer ping.

- Permitir Todo paquete como regla de **salida**

| ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-50-50.png) |
| :---- |

**SG-App (S2-S6)**

- Permitir Entrada: Puertos 8080-8085 solo desde el ID del grupo de seguridad SG-Proxy.  
- Permitir Entrada: Puerto 22 (SSH) desde tu IP.  
- Permitir Entrada: ICMP desde 0.0.0.0/0 (todo el mundo) para poder hacer ping.

- Permitir Todo paquete como regla de **salida**

| ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-50-54.png) |
| :---- |

**SG-DB (S7)**

- Permitir Entrada: Puerto 3306 (MySQL) solo desde el ID del grupo de seguridad SG-App  
- Permitir Entrada: ICMP desde 0.0.0.0/0 (todo el mundo) para poder hacer ping.

- Permitir Todo paquete como regla de **salida**

| ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-50-59.png)|
| :---- |

**Grupos de seguridad:**

| ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-51-06.png) |
| :---- |

**Asignamos los grupos de seguridad a sus respectivas instancias**

| ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-51-14.png) S1: ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-51-19.png) S2-S6: ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-51-23.png) S3: ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/c9e147cfc56791b8b12a7b861e6630b9b4bdda57/Sprint_2/CREACI%C3%93N%20DE%20LAS%20INSTANCIAS%20Y%20LOS%20DOCKERS/img/Captura%20de%20pantalla%20de%202026-02-02%2015-51-27.png)  |
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
