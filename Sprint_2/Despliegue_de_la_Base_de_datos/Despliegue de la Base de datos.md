# Despliegue de la Base de datos

Primero creamos la estructura de datos persistente:  
```bash
mkdir \-p \~/mysql/dbdata
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20181923.png) 
Creamos un archivo de instalación para el contenedor de docker:   
```bash
nano [setup-mysql.sh](http://setup-mysql.sh)
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20181927.png) 
Dentro del archivo [setup-mysql.sh](http://setup-mysql.sh) añadimos:  
```bash
echo "Lanzando contenedor MySQL..."  
docker run \-d \\  
  \--name s7-mysql \\  
  \-e MYSQL\_ROOT\_PASSWORD=rootpass \\  
  \-e MYSQL\_DATABASE=extagram \\  
  \-e MYSQL\_USER=extauser \\  
  \-e MYSQL\_PASSWORD=extapass \\  
  \-p 3306:3306 \\  
  \-v \~/mysql/dbdata:/var/lib/mysql \\  
  mysql:8.0
```bash
echo "Instalación completada."
``` 
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20181938.png)  
Le damos permisos de ejecución:  
```bash
chmod \+x setup-mysql.sh
``` 
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20181943.png) 
Ejecutamos el script con permisos de sudo:  
```bash
sudo ./setup-mysql.sh
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20182002.png) 
Comprobamos que podemos ver el contendor:  
```bash
docker ps
``` 
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20182010.png)

Comprobamos que MySQL está realmente accesible desde fuera de la instancia.  
```bash
sudo ss \-lntp | grep 3306
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20182016.png)
Configuramos la política de reinicio de docker para que se incie automáticamente cada vez que se inicie la instancia:  
```bash
docker update \--restart unless-stopped s7-mysql
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20182022.png)
Verificamos, nos debe salir unless-stopped:  
```bash
docker inspect \-f '{{.HostConfig.RestartPolicy.Name}}' s7-mysql
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20182029.png)
Entrar a MySQL y comprobar acceso:  
```bash
docker exec \-it s7-mysql mysql \-u extauser \-p
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20182034.png)
Comprobamos que la base de datos este creada y accedemos a esta:  
```bash
SHOW DATABASES;
``` 
USE extagram;  
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20182042.png)

Creamos las tablas:  
```bash
CREATE TABLE posts (  
  id INT AUTO\_INCREMENT PRIMARY KEY,  
  image VARCHAR(255),  
  description TEXT,  
  created\_at TIMESTAMP DEFAULT CURRENT\_TIMESTAMP  
);
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20182058.png)
Desde otra instancia como por ejemplo la 2 instalamos el mysql cliente y comprobamos que pueda conectar a la base de datos:  
Instalamos el cliente de MAriaDB oficial de amazon:  
```bash
sudo dnf install \-y mariadb105
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20182135.png)
Nos conectamos a la base de datos mediante la ip privada y el usuario extauser en la base de datos extagram con la contraseña extapass:  
```bash
mysql \-h 172.31.67.233 \-u extauser \-p extagram
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/2c67ae2c73741f6d256d0b7a3be8855976f919af/Sprint_2/Despliegue_de_la_Base_de_datos/img/Screenshot%202026-01-31%20182142.png)

Datos que se usaran en la aplicación en los demás contenedores para poderse conectar a la base de datos extagram:  
```bash
DB\_HOST=172.31.67.233  
DB\_NAME=extagram  
DB\_USER=extauser  
DB\_PASS=extapass  
DB\_PORT=3306
```
