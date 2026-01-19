# **INSTALACIÓN Y CONFIGURACIÓN DE MYSQL**

Instalamos mariadb:  
```bash
sudo yum install mariadb105-server \-y
```
![][image1]  
Iniciamos el servicio, lo habilitamos y por último miramos que el estado este en running:  
```bash

sudo systemctl start mariadb && sudo systemctl enable mariadb && sudo systemctl status mariadb  
```
![][image2]  
Creamos la Base De Datos:   
```bash
CREATE DATABASE extagram\_db;
```
![][image3]  
Creamos el usuario, añadimos los permisos y actualizamos  :  
```bash
CREATE USER 'extagram\_admin'@'localhost' IDENTIFIED BY 'pirineus';  
GRANT ALL PRIVILEGES ON extagram\_db.\ TO 'extagram\_admin'@'localhost';  
FLUSH PRIVILEGES;*  
```
*![][image4]  
*![][image5]*  
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
*![][image6]*
