# **HARDENING PROFESIONAL DEl SISTEMA OPERATIVO**

![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2710818a636ba2c078fdf18520920ec3124fc8cc/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Portada.png)

**N°:** GRUPO 1

**Integrantes:** Bryan Aguilera Nieto \- Izan Fernandez   
Javier \- Giuseppe Suarez

**Profesores:** Sergi \- David Sicart

## **ÍNDICE**

**[1. CONFIGURACIÓN DE INSTANCIA 1](#configuración-de-instancia-1)**
* [1.1. SSH](#ssh)
    * [a. Banner de Acceso](#banner-de-acceso)
    * [b. Seguridad en SSH](#seguridad-en-ssh)
* [1.2. UFW (Firewall)](#ufw)
    * [a. Modificaciones del firewall](#ahora-indicaremos-las-modificaciones-del-firewall)
* [1.3. Lynis (Auditoría)](#lynis)

---

**[2. CONFIGURACIÓN DE INSTANCIA 2](#configuración-de-instancia-2)**
* [2.1. SSH](#ssh-1)
    * [a. Banner de Acceso](#banner-de-acceso-1)
    * [b. Seguridad en SSH](#seguridad-en-ssh-1)
* [2.2. Firewalld](#firewalld)
    * [a. Instalación e Inicio](#instalación-e-inicio)
    * [b. Configuración de Reglas](#configuración-de-reglas)
* [2.3. Lynis (Auditoría)](#lynis-1)

---

**[3. CONFIGURACIÓN DE INSTANCIA 3](#configuración-de-instancia-3)**
* [3.1. SSH](#ssh-2)
    * [a. Banner de Acceso](#banner-de-acceso-2)
    * [b. Seguridad en SSH](#seguridad-en-ssh-2)
* [3.2. Firewalld](#firewalld-1)
    * [a. Instalación e Inicio](#instalación-e-inicio-1)
    * [b. Configuración de Reglas](#configuración-de-reglas-1)
* [3.3. Lynis (Auditoría)](#lynis-2)

[**CONFIGURACIÓN DE INSTANCIA 2	10**](#configuración-de-instancia-2)

1. [SSH	10](#ssh-1)

   [a. Banner de Acceso	10](#banner-de-acceso-1)

   [b. Seguridad en SSH	12](#seguridad-en-ssh-1)

2. [Firewalld	13](#firewalld)

   [a. Instalación e Inicio	13](#instalación-e-inicio)

   b. [Configuración de Reglas	14](#configuración-de-reglas)

3. [Lynis	15](#lynis-1)

[**CONFIGURACIÓN DE INSTANCIA 3	17**](#configuración-de-instancia-3)

[1\. SSH	17](#ssh-2)

[a. Banner de Acceso	17](#banner-de-acceso-2)

[b. Seguridad en SSH	19](#seguridad-en-ssh-2)

[2\. Firewalld	20](#firewalld-1)

[a. Instalación e inicio	20](#instalación-e-inicio-1)

 b. [Configuración de Reglas	21](#configuración-de-reglas-1)

3\. [Lynis	22](#lynis-2)

# 

# **CONFIGURACIÓN DE INSTANCIA 1** {#configuración-de-instancia-1}

1. ## **SSH** {#ssh}


1. ### **Banner de Acceso** {#banner-de-acceso}

     
   Primero, debemos de activarlo, para ello editaremos el archivo de configuración y buscamos la línea Banner  

         sudo nano /etc/ssh/sshd\_config
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/ae7cdb3cbafd8902ed370cbc656094b7fa77eafb/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Config-SSH-INST1.png)
   
   Quitamos la \# y pondremos lo siguiente Banner /etc/issue.net  
     
   Para crear el contenido del banner, ejecutaremos lo siguiente:  
     
         echo "\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*" | sudo tee /etc/issue /etc/issue.net
         echo "ACCESO RESTRINGIDO: Nodo Web Extagram \- Solo personal autorizado." | sudo tee \-a /etc/issue /etc/issue.net
         echo "Toda actividad esta siendo monitoreada y auditada." | sudo tee \-a /etc/issue /etc/issue.net
         echo "\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*" | sudo tee \-a /etc/issue /etc/[issue.net](http://issue.net)

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/ae7cdb3cbafd8902ed370cbc656094b7fa77eafb/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Banner-INST1.png)

   Reiniciamos el servicio ssh

         sudo systemctl restart sshd

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/ae7cdb3cbafd8902ed370cbc656094b7fa77eafb/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/SSH-RESTART-INST1.png)

   Ahora cuando entremos nos aparecerá este mensaje

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/1468bc038b0d11868c73e7416379ad4441b9d289/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Mensaje-Banner-INST1.png)

3. ### **Seguridad en SSH** {#seguridad-en-ssh}

   Aquí hemos modificado 3 cosas en el archivo

         sudo nano /etc/ssh/sshd\_config

   Luego los parametros serian estos

         LoginGraceTime 30
   Solo tienes 30 segundos para poder iniciar sesión.

         PermitRootLogin no
   Esto lo que hace es que impida que si se conectan por ssh tengan acceso directo como el usuario root

         MaxAuthTries 3
   En caso de fallar 3 veces la contraseña, el servidor corta conexión automáticamente

         MaxSessions 2
   En este caso indicamos el número máximo de sesiones que puede haber tanto por shell, el inicio de sesión o sistemas como sftp etc

         PasswordAuthentication yes
   Solo puedes entrar con ssh si tienes la clave privada, en caso contrario no podras acceder

         AllowTcpForwarding no
   Con esto lo que hacemos es bloquear las conexiones para que puedan acceder entre servidor a otro, es decir estamos quitando un “puente” que hay entre servidores 

         ClientAliveInterval 300
   Enviamos como un “aviso” cada 5 min (equivale a 300 seg) para saber si el usuario sigue conectado pero no escribe o si ha perdido conexión

         ClientAliveCountMax 0
   Acorde a la anterior, es la cantidad de “aviso” que se le da al cliente.

   Si pasan 5 min y se muestra el “aviso” y no tenemos respuesta no lo pregunta por segunda vez, simplemente cierra la conexión

   Comprobación
   
         sudo grep \-E "PermitRootLogin|MaxAuthTries|LoginGraceTime|ClientAlive|AllowTcpForwarding|MaxSessions|PasswordAuthentication" /etc/ssh/sshd\_config

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/1468bc038b0d11868c73e7416379ad4441b9d289/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Comprobaci%C3%B3n-SSH-INST1.png)

5. ### **UFW** {#ufw}

   ### Ahora indicaremos las modificaciones del firewall {#ahora-indicaremos-las-modificaciones-del-firewall}

         sudo ufw default deny incoming
   Bloqueamos todo por defecto menos a lo que yo le permiso explícitamente  
      
         sudo ufw default allow outgoing
      
   Damos permiso de para que nuestro servidor pueda tener acceso a internet hacia fuera  
      
         sudo ufw allow 22/tcp
      
   Habilitamos el puerto del ssh   
      
         sudo ufw allow 80/tcp
      
   Abrimos puerto 80 para la web con servicio HTTP  
      
         sudo ufw allow 443/tcp
      
   Abrimos puerto 8443 para la web con servicio HTTPS
      
         sudo ufw enable
      
   Iniciamos y habilitamos el firewall UFW

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/8710f9edd131f10ce5450decb5b5f602f9d70b02/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/REGLAS-UFW.INST1.png)
   
   El mensaje de Command may disrupt existing ssh connections. Proceed? nos indica que habilitando el firewall podríamos perder la conexión mediante ssh, desea continuar?\!  
   
   En nuestro caso, decimos que Yes porque ya pusimos que permitimos el servicio de ssh
   
   Finalmente, aparece que el firewall ya esta activado e iniciado en el sistema

4. ### **Lynis** {#lynis}

     
   Con esta herramienta, podremos hacer una auditoría de nuestro servidor  
     
   Primero de todo debemos instalarnos esta herramienta  
     
         sudo apt install lynis

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/8710f9edd131f10ce5450decb5b5f602f9d70b02/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Instalaci%C3%B3n-LYNIS-INST1.png)

   Ahora ejecutamos el lynis para ver como esta nuestro servidor tras los cambios realizados

         sudo lynis audit system

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/8710f9edd131f10ce5450decb5b5f602f9d70b02/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/1r%20Scaneo%20LYNIS-INST1.png)
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/8710f9edd131f10ce5450decb5b5f602f9d70b02/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Fin-Scaneo-LYNIS-INST1.png)
   
   Tras la auditoría con Lynis, observamos que tenemos una puntuación de 72/100 esto es un éxito ya que hemos implementado seguridad mientras que a su vez hemos optimizado el servidor.  
   Ciertas recomendaciones o avisos no se han implementado, que en este caso es por relación al Kernel, para evitar posibles incompatibilidades con los servidores y servicios desplegados

# **CONFIGURACIÓN DE INSTANCIA 2** {#configuración-de-instancia-2}

1. ### **SSH** {#ssh-1}

   Modificaremos el archivo de configuración de SSH para implementar seguridad en cuanto al acceso al servidor web de Extagram  

1. #### **Banner de Acceso**  {#banner-de-acceso-1}

     
   Como las otras veces, pondremos un banner, para ello activaremos y descomentamos la linea de Banner  
     
         sudo nano /etc/ssh/sshd\_config

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Banner-Inst2.png)

   Quitamos la \# y pondremos lo siguiente Banner /etc/issue.net  
   Para crear el contenido del banner, ejecutaremos lo siguiente:  
     
         echo "\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*" | sudo tee /etc/issue /etc/issue.net
         echo "ACCESO RESTRINGIDO: Nodo Web Extagram \- Solo personal autorizado." | sudo tee \-a /etc/issue /etc/issue.net
         echo "Toda actividad esta siendo monitoreada y auditada." | sudo tee \-a /etc/issue /etc/issue.net
         echo "\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*" | sudo tee \-a /etc/issue /etc/[issue.net](http://issue.net)
     
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Crear-Banner-Inst2.png)

   Reiniciamos el servicio ssh

         sudo systemctl restart sshd

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Restart-SSH-Inst2.png)

   Ahora cuando entremos nos aparecerá este mensaje


3. #### **Seguridad en SSH** {#seguridad-en-ssh-1}

   Misma configuración que en la instancia 1  
     
   Modificaremos el archivo ssh con

         sudo nano /etc/ssh/sshd\_config

   Luego los parametros serian estos

   LoginGraceTime

         LoginGraceTime 30
   PermitRootLogin
   
         PermitRootLogin no   

   MaxAuthTries
   
         MaxAuthTries 3

   MaxSessions
   
         MaxSessions 2

   PasswordAuthentication
   
         PasswordAuthentication yes  

   AllowTcpForwarding
   
         AllowTcpForwarding no 

   ClientAliveInterval
   
         ClientAliveInterval 300

   ClientAliveCountMax
   
         ClientAliveCountMax 0 

   Comprobación
   
         sudo grep \-E "PermitRootLogin|MaxAuthTries|LoginGraceTime|ClientAlive|AllowTcpForwarding|MaxSessions|PasswordAuthentication" /etc/ssh/sshd\_config
   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Comprobar-SSH-Inst2.png)

5. ### **Firewalld** {#firewalld}

   En esta instancia usaremos el cortafuegos llamado Firewalld

1. #### **Instalación e Inicio** {#instalación-e-inicio}

   Primero vamos a instalarlo  
     
         sudo dnf install firewalld \-y

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Instalar-Firewalld-Inst2.png)

     
   Ahora iniciaremos el servicio  
     
         sudo systemctl start firewalld

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Iniciar-Firewalld-Inst2.png)

     
   Para finalizar habilitamos para el arranque automático  
     
         sudo systemctl enable firewalld
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Habilitar-Firewalld-Inst2.png)
   
6. ### **Configuración de Reglas** {#configuración-de-reglas}

   Hemos decidido trabajar en las zonas de docker para segmentar el tráfico y que sea más seguro

   **Servicio SSH**

   Vamos a permmitir el servicio de ssh

         sudo firewall-cmd \--permanent \--zone=docker \--add-service=ssh

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Regla-Firewalld-SSH-Inst2.png)

   **Servicios Web**

   Permitimos el tráfico para las conexiones HTTP y HTTPS

   **HTTP**
   
         sudo firewall-cmd \--permanent \--zone=docker \--add-service=http
   **HTTPS**
   
         sudo firewall-cmd \--permanent \--zone=docker \--add-service=https


   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Regla-WEB-Inst2.png)

   **Servicio DNS**   

   Necesario para que nuestro servidor pueda resolver nombres de dominios externos

         sudo firewall-cmd \--permanent \--zone=docker \--add-service=dns

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Regla-DNS.Inst2.png)

   **Aplicar Cambios**

   Para que las reglas se apliquen, deberemos recargar el firewall

         sudo firewall-cmd \--reload
   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Reload-Firewalld-Inst2.png)
   
8. ### **Lynis** {#lynis-1}
  
   Igual como en la instancia 1 procederemos a la implementación de dicha herramienta   

         sudo apt yum lynis
   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/00e30f1c516fb0adde356c61026b5b60c69dfd67/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Instalar-Firewalld-Inst2.png)
   
   Ahora ejecutamos el lynis parala auditoría del sistema

         sudo lynis audit system

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/90089e2e1c9c9a6db978182a27ce8393c79f568a/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/2o-ScaneoInst2.png)
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/90089e2e1c9c9a6db978182a27ce8393c79f568a/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/2o-Fin-Scaneo-Inst2.png)
   
   Tras el análisis de Lynis, obtenemos un índice de hardening alto, pero al igual que el anterior, se han descartado ciertas optimizaciones y / o seguridad para implementar relativas al Kernel, para evitar            posibles incompatibilidades

# **CONFIGURACIÓN DE INSTANCIA 3** {#configuración-de-instancia-3}

1. ### **SSH** {#ssh-2}

   Modificaremos el archivo de configuración de SSH para implementar seguridad en cuanto al acceso al servidor web de Extagram  

1. #### **Banner de Acceso**  {#banner-de-acceso-2}

     
   Primero, debemos de activarlo, para ello editaremos el archivo de configuración y buscamos la línea Banner  
     
         sudo nano /etc/ssh/sshd\_config

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Config-SSHD-Inst3.png) 
     
   Quitamos la \# y pondremos lo siguiente Banner /etc/[issue.net](http://issue.net)  
     
   Para crear el contenido del banner, ejecutaremos lo siguiente:

         echo "\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*" | sudo tee /etc/issue /etc/issue.net
         echo "ACCESO RESTRINGIDO: Nodo Web Extagram \- Solo personal autorizado." | sudo tee \-a /etc/issue /etc/issue.net
         echo "Toda actividad esta siendo monitoreada y auditada." | sudo tee \-a /etc/issue /etc/issue.net
         echo "\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*" | sudo tee \-a /etc/issue /etc/[issue.net](http://issue.net)

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Crear-Mensaje.Inst3.png)
    
   Reiniciamos el servicio ssh  

         sudo systemctl restart sshd
   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Restart-SSH-Inst3.png)   

   Ahora cuando entremos nos aparecerá este mensaje

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Mensaje-Inicio-Inst3.png)
   
3. #### **Seguridad en SSH** {#seguridad-en-ssh-2}

     
   Misma configuración que en las otras instancias  
     
   Modificaremos el archivo ssh con

         sudo nano /etc/ssh/sshd\_config

   LoginGraceTime

         LoginGraceTime 30

   PermitRootLogin
   
         PermitRootLogin no   

   MaxAuthTries
   
         MaxAuthTries 3
   
   MaxSessions
   
         MaxSessions 2
   
   PasswordAuthentication

         PasswordAuthentication yes  

   AllowTcpForwarding
   
         AllowTcpForwarding no
   
   ClientAliveInterval
   
         ClientAliveInterval 300

   ClientAliveCountMax
   
         ClientAliveCountMax 0

   Comprobación
   
         sudo grep \-E "PermitRootLogin|MaxAuthTries|LoginGraceTime|ClientAlive|AllowTcpForwarding|MaxSessions|PasswordAuthentication" /etc/ssh/sshd\_config

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Comprobar-SSH-Inst3.png)

2. ### **Firewalld** {#firewalld-1}

   En esta instancia usaremos el cortafuegos llamado Firewalld como en la instancia 2

1. #### **Instalación e inicio** {#instalación-e-inicio-1}
     
   Primero vamos a instalarlo  
     
         sudo dnf install firewalld \-y

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Instalar-Firewall-Inst3.png)
        
   Ahora iniciaremos el servicio  
     
         sudo systemctl start firewalld
   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Iniciar-Firewalld-Inst3.png)
  
   Para finalizar habilitamos para el arranque automático  
     
         sudo systemctl enable firewalld

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Habilitar-Firewalld-Inst3.png)  

3. ### **Configuración de Reglas** {#configuración-de-reglas-1}

   Hemos decidido trabajar en las zonas de docker para segmentar el tráfico y que sea más seguro

   **Servicio SSH**

   Vamos a permmitir el servicio de ssh

         sudo firewall-cmd \--permanent \--zone=docker \--add-service=ssh

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Regla-SSH-Inst3.png)  

   **Servicios Web**

   Permitimos el tráfico para las conexiones HTTP y HTTPS

   **HTTP**

         sudo firewall-cmd \--permanent \--zone=docker \--add-service=http

   **HTTPS**

         sudo firewall-cmd \--permanent \--zone=docker \--add-service=https

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Regla-WEB-Inst3.png)  

   **Servicio DNS**

   Necesario para que nuestro servidor pueda resolver nombres de dominios externos
   
         sudo firewall-cmd \--permanent \--zone=docker \--add-service=dns

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Regla-DNS-Inst3.png)  

   **Aplicar Cambios**

   Para que las reglas se apliquen, deberemos recargar el firewall

         sudo firewall-cmd \--reload
   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Reload-Firewalld-Inst3.png)  
   
5. ### **Lynis** {#lynis-2}
  
   Igual como en las otras procederemos a la implementación de dicha herramienta  

         sudo dnf install lynis -y

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/Instalar-LYNIS-Inst3.png)  

   Ahora ejecutamos el lynis para la auditoría del sistema

         sudo lynis audit system

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/3r-Scaneo-Inst3.png)  
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/6a6af1061c580d3a81769c154bf46b67488a7b10/sprint_4/HARDENING%20PROFESIONAL%20DE%20SISTEMA%20OPERATIVO/img/3r-Fin-Scaneo-Inst3.png)  

   Tras el análisis de Lynis, obtenemos un índice de hardening alto, omitimos cierta parte de la configuración por lo mencionado anteriormente 
