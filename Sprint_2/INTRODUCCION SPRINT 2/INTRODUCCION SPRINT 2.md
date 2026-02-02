**Introducción:**

Crearemos 3 instancias en amazon web service que se repartirán con diferentes dockers cada una:  
![]() 
**Instancia 1:** S1 (Apahce Proxy)  
Recibe el tráfico del navegador y lo reparte.

**Instancia 2:** S2, S3, S4, S5, S6  
Aquí vive el PHP (Extagram y Uploads) y los apache de archivos estáticos.

**Instancia 3:** S7 (MySQL)  
Base de datos persistente.

Teniendo en cuenta esto, dividiremos las tareas en **4 bloques**. Cada uno se hace cargo de un bloque.

## **1\. Administrador de Infraestructura Cloud (AWS)**

* **Despliegue de Instancias EC2:** Crear y lanzar las 3 instancias (Proxy, App, DB) con sus respectivas Elastic IPs para asegurar el acceso estático.  
* **Configuración de Security Groups:** Configurar las reglas de firewall para permitir tráfico HTTP (80) en la Instancia 1, y restringir los puertos de App y DB solo a las IPs internas necesarias.  
* **Instalación del Runtime de Docker:** Instalar **`docker.io`** y `docker-compose` en las tres máquinas y configurar los permisos de usuario.  
* **Diseño del esquema de red:** Crear el esquema técnico de la red utilizando Packet Tracer o una herramienta similar, definiendo cómo se conectan los Docker.

## **2\. Proxy y Estáticos (S1, S5, S6)**

* **Configuración del Balanceador S1:** Crear el archivo **`apache.conf`** para el contenedor S1 que realice el balanceo de carga entre S2 y S3.  
* **Segregación de rutas:** Configurar el proxy para que las peticiones a **`/upload.php`** vayan a S4, y las de archivos estáticos (**`style.css`, `preview.svg`**) se sirvan desde S6.  
* **Despliegue de Servidores Estáticos:** Configurar los contenedores S5 (para imágenes subidas) y S6 (estilos y recursos del sistema).  
* **Pruebas de Redundancia:** Verificar que si un nodo (S2 o S3) cae, la web sigue operativa a través del otro.

## **3\. Desarrollador Backend y Lógica PHP (S2, S3, S4)**

* **Adaptación de Scripts PHP:** Modificar **`extagram.php`** y **`upload.php`** para que utilicen la IP privada de la Instancia 3 para conectar con la base de datos.  
* **Configuración de PHP-FPM:** Desplegar los contenedores S2, S3 y S4 utilizando la imagen **`php:fpm`** y asegurar que procesan correctamente la parte dinámica de la web.  
* **Gestión de Rutas de Almacenamiento:** Configurar S4 para que las imágenes se guarden en el volumen compartido que luego leerá S5.  
* **Sincronización de Base de Datos:** Implementar la lógica para que las imágenes se guarden también como "blobs" en la BBDD como medida de redundancia.

## **4\. Administrador de Datos y Documentación (S7 \+ QA)**

* **Despliegue de Base de Datos S7:** Configurar el contenedor MySQL con el esquema proporcionado (DB: **`extagram_db`**, Usuario: **`extagram_admin`**).  
* **Persistencia de Datos:** Configurar los volúmenes de Docker en la Instancia 3 para que los datos de la carpeta **`dbdata/`** no se pierdan al reiniciar contenedores.  
* **Control de versiones y Git:** Mantener el repositorio de GitHub actualizado, gestionando las claves público/privada para el acceso del servidor.  
* **Documentación de Actas:** Redactar en Markdown las actas de *sprint planning* y *sprint review*, incluyendo capturas de pantalla del progreso en ProofHub.

**Responsables de cada bloque**

| Bloque 1 | Javier Méndez |
| :---- | :---- |
| **Bloque 2** | Carlos García |
| **Bloque 3** | Izan Fernández |
| **Bloque 4** | Bryan Aguilera |
