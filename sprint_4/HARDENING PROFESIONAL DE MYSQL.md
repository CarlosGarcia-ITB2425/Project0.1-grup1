# **HARDENING PROFESIONAL DE MYSQL**

1\. Revisión y eliminación de usuarios:  
	Miramos los usuarios creados:  
```bash
SELECT user, host FROM mysql.user; 
```	
![][image1]  
Eliminamos el usuario root @ % para que solo quede root | localhot:  
```bash
DROP USER IF EXISTS 'root'@'%';
```  
![][image2]  
2\. Corrección de usuarios de la aplicación:  
Docker por default crea los usuarios para que se puedan conectar desde cualquier host, esto lo restringimos a la red privada:  
Creamos usuario restringido con la contraseña:   
```bash
CREATE USER IF NOT EXISTS 'extauser'@'172.17.%'  
IDENTIFIED WITH caching\_sha2\_password  
BY 'ExtaPass\!2026\_Fuerte';

GRANT SELECT, INSERT, UPDATE, DELETE  
ON extagram.\*  
TO 'extauser'@'172.17.%';

FLUSH PRIVILEGES;
```
![][image3]

Si ya tenemos creados los usuarios cambiamos las contraseñas por “ExtaPass\!2026\_Fuerte“  
```bash
ALTER USER 'extauser'@'172.31.%'  
IDENTIFIED WITH caching\_sha2\_password  
BY 'ExtaPass\!2026\_Fuerte';

ALTER USER 'extauser'@'172.17.%'  
IDENTIFIED WITH caching\_sha2\_password  
BY 'ExtaPass\!2026\_Fuerte';

FLUSH PRIVILEGES;
```
![][image4]  
Damos solo los permisos necesarios: 
```bash
GRANT SELECT, INSERT, UPDATE, DELETE ON extagram.\* TO 'extauser'@'172.31.%';  
GRANT SELECT, INSERT, UPDATE, DELETE ON extagram.\* TO 'extauser'@'172.17.%';  
FLUSH PRIVILEGES;
```
![][image5]

Eliminar el usuario abierto por el cual pueden acceder desde cualquier host:  
```bash
DROP USER 'extauser'@'%';
``` 
![][image6]  
	

3\. Activamos la política de contraseñas fuertes:  
	Instalamos el componente: 
```bash
INSTALL COMPONENT 'file://component\_validate\_password';
```
![][image7]  
Configuración de la política de contraseñas (Las contraseñas deben tener mínimo 14 caracteres, deben incluir mayúsculas y minúsculas, deben, tener números, deben tener caracteres especiales,  y que se guarde de forma persistente para que no se pierda al reiniciar: 
```bash
SET PERSIST validate\_password.length \= 14;  
SET PERSIST validate\_password.policy \= STRONG;  
SET PERSIST validate\_password.mixed\_case\_count \= 1;  
SET PERSIST validate\_password.number\_count \= 1;  
SET PERSIST validate\_password.special\_char\_count \= 1;
```
![][image8]

4\. Desactivamos funciones peligrosas:  
	Evitar ataques vía carga de archivos:  

	SET PERSIST local\_infile \= OFF;

![][image9]	  
Comprobación:  

	SHOW VARIABLES LIKE 'local\_infile';  
	
![][image10]

5\. Activamos el modo SQL estricto:  
Evitar datos corruptos silenciosos:  

	SET PERSIST sql\_mode \= 'STRICT\_TRANS\_TABLES,ERROR\_FOR\_DIVISION\_BY\_ZERO,NO\_ENGINE\_SUBSTITUTION';  
	
![][image11]

6\. Limitamos el abuso de conexiones:  
Limitamos el número máximo de conexiones simultaneas que puede aceptar Mysql:  

	SET PERSIST max\_connections \= 150;
	
![][image12]  
Limitamos los intentos fallidos de conexiones desde una misma IP, si una ip falla 10 veces seguidas el MYSQL bloqueará automáticamente la ip:  

	SET PERSIST max\_connect\_errors \= 10;  
![][image13]  
Limitamos por usuario:  

	ALTER USER 'extauser'@'172.31.%' WITH MAX\_USER\_CONNECTIONS 20;  
	ALTER USER 'extauser'@'172.17.%' WITH MAX\_USER\_CONNECTIONS 20;  
![][image14]  
7\. Activamos los logs de consultas lentas:  
Activa el registro de consultas lentas (Slow Query Log):  

	SET PERSIST slow\_query\_log \= ON; 
	
![][image15]  
Define el tiempo (en segundos) que debe tardar una consulta para considerarse “lenta”:  

	SET PERSIST long\_query\_time \= 2;  
	
![][image16]  
8\. Verificación de todo lo configurado:  
	Comprobamos los usuarios existente y desde donde se pueden conectar:  
	
	SELECT user, host FROM mysql.user;  
![][image17]  
Permisos exactos del usuario de la aplicación:  

	SHOW GRANTS FOR 'extauser'@'172.31.%';  
	SHOW GRANTS FOR 'extauser'@'172.17.%';  
![][image18]

Comprobamos que la política de contraseñas fuerte está activa:
	
	SHOW VARIABLES LIKE 'validate\_password%';  
![][image19]  
Comprobamos  que LOCAL INFILE está desactivado, LOCAL INFILE permite cargar archivos del sistema al servidor:  

	SHOW VARIABLES LIKE 'local\_infile';  
![][image20]  
Comprobamos que el modo SQL estricto este activo:  

	SHOW VARIABLES LIKE 'sql\_mode';  
![][image21]




















