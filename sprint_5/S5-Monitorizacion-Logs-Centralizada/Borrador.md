# Desplegar Grafana y Loki - Validación completa del sistema

![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/7cc34c0d6f79cc5b801297d1be9df3821c75a5ee/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/PORTADA.png)

**N°:** GRUPO 1

**Integrantes:** Bryan Aguilera Nieto \- Izan Fernandez   
Javier \- Giuseppe Suarez

**Profesores:** Sergi \- David Sicart

## ÍNDICE

- [S5-01 - Desplegar Grafana y Loki](#s5-01---desplegar-grafana-y-loki)

- [CONFIGURACIÓN DE INSTANCIA 2 – Grafana y Loki](#configuracion-de-instancia-2--grafana-y-loki)
  - [MONITOREAR](#monitorear)
    - [Creación de directorios](#creacion-de-directorios)
    - [Creación de archivos de configuración](#creacion-de-archivos-de-configuracion)
    - [Crear nuevo docker](#crear-nuevo-docker)
    - [Lanzar docker y comprobamos](#lanzar-docker-y-comprobamos)
  - [FIREWALL](#firewall)
    - [Crearemos nuevas reglas de Firewall](#crearemos-nuevas-reglas-de-firewall)
  - [REGLA DE AWS](#regla-de-aws)
    - [Crearemos la regla en AWS](#crearemos-la-regla-en-aws)
  - [PRUEBAS](#pruebas)
    - [Grafana](#grafana)
    - [Conectar con Loki](#conectar-con-loki)

- [S5-07 - Validación completa del sistema](#s5-07---validacion-completa-del-sistema)

- [CONFIGURACIÓN DE INSTANCIA 1](#configuracion-de-instancia-1)
  - [Promtail – Instancia 1](#promtail--instancia-1)
    - [Envío de datos](#envio-de-datos)
    - [Regla AWS](#regla-aws)
    - [Comprobar](#comprobar)

- [CONFIGURACIÓN DE INSTANCIA 3 (S7)](#configuracion-de-instancia-3-s7)
  - [Promtail – Instancia 3](#promtail--instancia-3)
    - [Envío de datos](#envio-de-datos-1)
    - [Regla AWS](#regla-aws-1)
    - [Comprobar](#comprobar-1)

- [CONFIGURACIÓN DE INSTANCIA 2 – Promtail](#configuracion-de-instancia-2--promtail)
  - [Promtail – Instancia 2](#promtail--instancia-2)
    - [Envío de datos](#envio-de-datos-2)
    - [Comprobar](#comprobar-2)# 

# **S5-01 - Desplegar Grafana y Loki**

## **CONFIGURACIÓN DE INSTANCIA 2 – Grafana y Loki**

### MONITOREAR
   

#### Creación de directorios     
   Crearemos una carpeta para realizar este sprint, lo llamaremos monitoratge  
     
         mkdir ~/monitoratge  
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/crear-directorio-inst2.png)  
     
   Ahora accedemos a esta carpeta creada  
     
         cd ~/monitoratge  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/acceder-directorio-inst2.png)

     
   Para tenerlo mas organizado lo tendríamos en otra carpeta la config del Loki  
     
         mkdir loki-config   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/crear-directorio2-inst2.png)

#### Creación de archivos de configuración

     
   Dentro del anterior directorio que hemos creado, crearemos un archivo yaml. que contendrá lo siguiente  
     
         sudo nano loki-config.yaml
   #
         auth_enabled: false
         server:
            http_listen_port: 3100
         common:
            path_prefix: /tmp/loki
           storage:
             filesystem:
               chunks_directory: /tmp/loki/chunks
               rules_directory: /tmp/loki/rules
           replication_factor: 1
           ring:
             kvstore:
               store: inmemory
         schema_config:
           configs:
             - from: 2020-10-24
                  store: boltdb-shipper
                  object_store: filesystem
                  schema: v11
                  index:
                    prefix: index_   
                    period: 24h

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/loki-config.png)

3. #### **Crear nuevo docker**

     
   Crearemos un nuevo docker para levantar el Grafana y Loki  
     
   Primero deberemos de irnos a la carpeta de monitoratge  
     
         cd ..
   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/Volver-inst2.png)
     
   Ahora creamos el archivo .yml  
     
         nano docker-compose.yml
   #
         version: '3.8'  
     
         services:  
           grafana:  
             image: grafana/grafana:latest  
             container_name: grafana  
             ports:  
               - "3000:3000"  
             restart: unless-stopped  
             networks:  
               - extagram-net
   
        loki:  
          image: grafana/loki:latest  
          container_name: loki  
          command: -config.file=/etc/loki/local-config.yaml  
          restart: unless-stopped  
          networks:  
            - extagram-net  

           s6-static:  
             image: nginx:alpine  
             container_name: s6-static  
             volumes:  
               - ./static-content:/usr/share/nginx/html:ro  
             networks:  
               - extagram-net  
         networks:  
           extagram-net:  
             driver: bridge  

      ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/docker-config.png)

5. #### **Lanzar docker y comprobamos**

     
   Ahora lanzaremos el docker   
     
         sudo docker compose up -d

   Ahora con este comando vemos los que están activos
   
         sudo docker ps

![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/estado-dockers.png)

2. ### **FIREWALL**


1. #### **Crearemos nuevas reglas de Firewall**

     
   Abriremos el puerto del Grafana que es el 3000  
     
         sudo firewall-cmd --permanent --add-port=3000/tcp  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/firewall-inst2.png)  
     
   Abriremos el sitio estático que en este caso es el 8080  
     
         sudo firewall-cmd --permanent --add-port=8080/tcp
   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/firewall2-inst2.png)  

   Recargamos las reglas   
     
         sudo firewall-cmd --reload  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/recargar-firewall.png)  
     
   Verificamos reglas  
     
         sudo firewall-cmd --list-all  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/verificar-firewall.png)  

3. ### **REGLA DE AWS**
   
1. #### **Crearemos la regla en AWS**
     
   Ahora en AWS creamos dos nuevas reglas una para GRAFANA y otro para LOKI, para podernos conectarnos  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/firewall-AWS.png)  

4. ### **PRUEBAS**

1. #### **Grafana**
     
   Ahora probamos a acceder a la página web de Grafana que es esta
   
   [http://44.223.184.184:3000](http://44.223.184.184:3000)  

   Nos carga satisfactoriamente  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/grafana-admin.png)  
     
   Al entrar nos pedirá cambiar la contraseña, en nuestro caso pondremos la siguiente

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/contra-grafana.png)  

3. #### **Conectar con Loki**

   Ahora lo conectaremos con nuestro receptor de logs que seria Loki en nuestro caso  
     
   Para ello nos vamos al apartado donde nos dice  
     
   Connections y despues a Data Source  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/a%C3%B1adir-loki.png)  
     
   Damos click aqui  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/a%C3%B1adir-data-source.png)
   
   En el buscador, pondremos Loki que es el que estamos utilizando
     
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/a%C3%B1adir-loki-grafana.png)
   
   Ahora al darle click pondremos aqui esta url  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/url-loki.png)

   Ahora al darle a Save & Test nos saldrá Successful
   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/save%20%26%20test.png)
     
   Ahora provocamos un Log manualmente

         curl -H "Content-Type: application/json" -XPOST "http://localhost:3100/loki/api/v1/push" \
         --data '{
        "streams": [
       {
         "stream": { "job": "test-manual", "level": "info" },
         "values": [ [ "'$(date +%s%N)'", "Hola Profe, esto es un log de prueba" ] ]
       }
        ]
         }'

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/provocar-log.png)
     
   Para verlo nos vamos aquí \- Explorer \- Label Browser
     
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/label-browser.png)

   Y al darle a Label Browser, obtendremos esto
   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/label-browser2.png)
     
   Ahora le damos a **Job**, y después a **Test Manual** y después **Show Logs**  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/comprobar-label-browser.png)
     
   Al darle nos aparece lo siguiente, tanto los filtros que hemos puesto como el mensaje del Log que hemos generado  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/39db87dedf17bb41cb42df343bf712519dd9b74d/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/show-logs.png)   

   # 

# **S5-07 - Validación completa del sistema**

## **CONFIGURACIÓN DE INSTANCIA 1**

1. ### **Promtail – Instancia 1**


     
   Creamos una carpeta llamada promtail  
     
         mkdir -p ~/promtail  
     
   Accedemos a la carpeta  
     
         cd ~/promtail  
     
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/93c794d202f25d26658fd044e095e8c5a4e50ce4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/crear-directorio-inst1.png)   
         
   Creamos un archivo  
     
         nano promtail-config.yaml  
     
   Y el archivo tiene esto  
     
         server:  
           http_listen_port: 9080  
           grpc_listen_port: 0  

         positions:  
              filename: /tmp/positions.yaml  

         clients:  
           - url: http://172.31.31.80:3100/loki/api/v1/push  
     
         scrape_configs:  
           - job_name: system  
          static_configs:  
             - targets:  
              - localhost  
         labels:  
            job: varlogs  
            instance: instancia-1   
            __path__: /var/log/*.log  
     
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/93c794d202f25d26658fd044e095e8c5a4e50ce4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/config-grafana-inst1.png)   

3. ####  **Envío de datos**

   Ejecuta este comando para que empiece a enviar datos

         sudo docker run -d
   
           --name promtail \

           -v $(pwd):/etc/promtail \

           -v /var/log:/var/log \

           grafana/promtail:latest \

        -config.file=/etc/promtail/promtail-config.yaml

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/93c794d202f25d26658fd044e095e8c5a4e50ce4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/correr-docker-inst1.png)   

4. #### **Regla AWS**

     
   Ahora debemos de añadir la siguiente regla en AWS para que funcione  
     
   Puerto 3100 y la IP privada de la instancia 1, en nuestro caso pusimos la siguiente descripción  
     
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/93c794d202f25d26658fd044e095e8c5a4e50ce4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/firewall-AWS-inst1.png)   

5. ####  **Comprobar**

   Como en la configuración de la instancia pusimos que se cree una etiqueta llamado instancia nos aparece aqui, ademas de poder ver que la que se ha enviado es la Instancia 1  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/93c794d202f25d26658fd044e095e8c5a4e50ce4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/label-inst1.png)   

   Nos sale los logs 

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/93c794d202f25d26658fd044e095e8c5a4e50ce4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/show-logs.png)   

   ## **CONFIGURACIÓN DE INSTANCIA 3 (S7)**

1. ### **Promtail – Instancia 3**


     
   Creamos una carpeta llamada promtail  
     
         mkdir -p ~/promtail  
     
   Accedemos a la carpeta  
     
         cd ~/promtail  
     
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/519c8faf365e9f424ba3843462b7dd600cd94ac4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/directorio-inst3.png)   
     
   Creamos un archivo  
     
         nano promtail-config.yaml  
     
   Y el archivo tiene esto  
     
         server:  
           http_listen_port: 9080  
           grpc_listen_port: 0  
     
         positions:  
           filename: /tmp/positions.yaml  
     
         clients:  
           - url: http://172.31.31.80:3100/loki/api/v1/push  
     
         scrape_configs:  
           - job_name: system_s7  
          static_configs:  
          - targets:  
           - localhost  
            labels:  
              job: varlogs_s7  
              instance: instancia-s7    
              __path__: /var/log/*.log  
     
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/519c8faf365e9f424ba3843462b7dd600cd94ac4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/config-yaml-inst3.png)   

2. #### **Envío de datos**

   Ejecuta este comando para que empiece a enviar datos


         sudo docker run -d \

           --name promtail \

           -v $(pwd):/etc/promtail \

           -v /var/log:/var/log \

           grafana/promtail:latest \

           -config.file=/etc/promtail/promtail-config.yaml

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/519c8faf365e9f424ba3843462b7dd600cd94ac4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/correr-docker-inst3.png)   

3. #### **Regla AWS**

     
   Ahora debemos de añadir la siguiente regla en AWS para que funcione  
     
   Puerto 3100 y la IP privada de la instancia 3, en nuestro caso pusimos la siguiente descripción  
     
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/519c8faf365e9f424ba3843462b7dd600cd94ac4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/firewall-AWS-inst3.png)   

4. #### **Comprobar**

   Como en la configuración de la instancia pusimos que se cree una etiqueta llamado instancia nos aparece aqui, ademas de poder ver que la que se ha enviado es la Instancia 3  
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/519c8faf365e9f424ba3843462b7dd600cd94ac4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/label-inst3.png)   
     
   Nos sale los logs   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/519c8faf365e9f424ba3843462b7dd600cd94ac4/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/show-logs-inst3.png)      

   ## **CONFIGURACIÓN DE INSTANCIA 2 – Promtail**

1. ### **Promtail – Instancia 2**

     
   Creamos una carpeta llamada promtail  
     
         mkdir -p ~/promtail  
     
   Accedemos a la carpeta  
     
         cd ~/promtail  
     
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/04a8c2566fb75a07ee44d26d007c76d7c21f632e/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/directorio-inst2.png)      
     
   Creamos un archivo  
     
         nano promtail-config.yaml  
     
   Y el archivo tiene esto  
     
         server:  
           http_listen_port: 9081  
           grpc_listen_port: 0  
     
         positions:  
           filename: /tmp/positions.yaml  
     
         clients:  
           - url: http://172.31.31.80:3100/loki/api/v1/push  
     
         scrape_configs:  
           - job_name: system_s2  
             static_configs:  
               - targets:  
               - localhost  
            labels:  
              job: varlogs_s2  
              instance: instancia-2  
              __path__: /var/log/*.log  
     
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/04a8c2566fb75a07ee44d26d007c76d7c21f632e/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/yaml-inst2.png)         

2. ####  **Envío de datos**

   Ejecuta este comando para que empiece a enviar datos

         sudo docker run -d \

           --name promtail-s2 \

           -v $(pwd):/etc/promtail \

           -v /var/log:/var/log \

           grafana/promtail:latest \

           -config.file=/etc/promtail/promtail-config.yaml

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/04a8c2566fb75a07ee44d26d007c76d7c21f632e/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/docker-inst2.png)         

3. #### **Comprobar**

   Como en la configuración de la instancia pusimos que se cree una etiqueta llamado instancia nos aparece aqui, ademas de poder ver que la que se ha enviado es la Instancia 2  

   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/04a8c2566fb75a07ee44d26d007c76d7c21f632e/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/label-inst2.png)         
     
   Nos sale los logs   
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/04a8c2566fb75a07ee44d26d007c76d7c21f632e/sprint_5/S5-Monitorizacion-Logs-Centralizada/img/logs-inst2.png)         
 
