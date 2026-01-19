# **INSTALACIÓN Y CONFIGURACIÓN DE MYSQL**

Instalamos mariadb:  
```bash
sudo yum install mariadb105-server \-y
```
![imagen1](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/41bb7dd48c67281a7336e68223fc84c14592c47d/docs/INSTALCION_Y_CONFIGURACION_DE_MYSQL/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-01-49.png)

Iniciamos el servicio, lo habilitamos y por último miramos que el estado este en running:  
```bash
sudo systemctl start mariadb && sudo systemctl enable mariadb && sudo systemctl status mariadb  
```

![imagen2](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/41bb7dd48c67281a7336e68223fc84c14592c47d/docs/INSTALCION_Y_CONFIGURACION_DE_MYSQL/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-02-14.png)

Creamos la Base De Datos:   
```bash
CREATE DATABASE extagram\_db;
```
![imagen3](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/41bb7dd48c67281a7336e68223fc84c14592c47d/docs/INSTALCION_Y_CONFIGURACION_DE_MYSQL/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-02-35.png)

Creamos el usuario, añadimos los permisos y actualizamos  :  
```bash
CREATE USER 'extagram\_admin'@'localhost' IDENTIFIED BY 'pirineus';  
GRANT ALL PRIVILEGES ON extagram\_db.\ TO 'extagram\_admin'@'localhost';  
FLUSH PRIVILEGES;*  
```

![imagen4](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/41bb7dd48c67281a7336e68223fc84c14592c47d/docs/INSTALCION_Y_CONFIGURACION_DE_MYSQL/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-03-03.png)
![imagen5](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/41bb7dd48c67281a7336e68223fc84c14592c47d/docs/INSTALCION_Y_CONFIGURACION_DE_MYSQL/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-03-18.png)

Creamos la tabla post:  
```bash
USE extagram\_db;  
CREATE TABLE posts (  
    id INT AUTO\_INCREMENT PRIMARY KEY,  
    title VARCHAR(255) NOT NULL,  
    image\_url VARCHAR(255), 
    description TEXT,*  
    created\_at TIMESTAMP DEFAULT CURRENT\_TIMESTAMP 
);  
```
![imagen6](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/41bb7dd48c67281a7336e68223fc84c14592c47d/docs/INSTALCION_Y_CONFIGURACION_DE_MYSQL/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-04-04.png)
