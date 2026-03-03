**Firewall S1 (Hardening SO)**

**Hardening del Sistema Operativo del S1 (Firewall Interno)**

Configurar un firewall a nivel de host en la instancia EC2 que aloja a S1 (Nginx). Al estar usando Amazon Linux (dnf), utilizaremos firewalld

**Instalamos y activamos el servicio**  
**Comandos:**  

    sudo apt install firewalld \-y
    sudo systemctl start firewalld
    sudo systemctl enable firewalld
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/main/sprint_4/Seguridad%20S1%20(Hardening%20SO)/img/Seguridad_S1_1.png)

**Permitir solo lo estrictamente necesario (HTTP y SSH)**  
**Comandos:**

    sudo firewall-cmd \--permanent \--add-service=http
    sudo firewall-cmd \--permanent \--add-service=ssh

**Aplicar reglas**  
**Comandos:**

    sudo firewall-cmd \--reload
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/main/sprint_4/Seguridad%20S1%20(Hardening%20SO)/img/Seguridad_S1_2.png)

Aunque AWS tiene Security Groups, un firewall interno protege la instancia si hay un error de configuración en la red de la nube.

Mínimo privilegio: Se cierran todos los puertos excepto el 80 (tráfico web) y el 22 (administración), reduciendo la superficie de ataque.

Para poder verificar los comandos activos **que permanecerán despues del reinicio**, podemos utilizar el siguiente comando:

    sudo firewall-cmd \--list-all \--permanent 
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/main/sprint_4/Seguridad%20S1%20(Hardening%20SO)/img/Seguridad_S1_3.png)  
(Confirmamos que los servicios necesarios están aplicados correctamente).
