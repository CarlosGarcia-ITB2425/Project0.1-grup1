# **HARDENING PROFESIONAL DE MYSQL**

1\. Revisión y eliminación de usuarios:  
	Miramos los usuarios creados:  
```bash
SELECT user, host FROM mysql.user; 
```	
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180057.png) 

Eliminamos el usuario root @ % para que solo quede root | localhot:  
```bash
DROP USER IF EXISTS 'root'@'%';
```  
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180109.png) 

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
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180117%20-%20copia.png)

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
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180123%20-%20copia.png) 

Damos solo los permisos necesarios: 
```bash
GRANT SELECT, INSERT, UPDATE, DELETE ON extagram.\* TO 'extauser'@'172.31.%';  
GRANT SELECT, INSERT, UPDATE, DELETE ON extagram.\* TO 'extauser'@'172.17.%';  
FLUSH PRIVILEGES;
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180130%20-%20copia.png)

Eliminar el usuario abierto por el cual pueden acceder desde cualquier host:  
```bash
DROP USER 'extauser'@'%';
``` 
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180135%20-%20copia.png)

3\. Activamos la política de contraseñas fuertes:  
	Instalamos el componente: 
```bash
INSTALL COMPONENT 'file://component\_validate\_password';
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180143.png) 

Configuración de la política de contraseñas (Las contraseñas deben tener mínimo 14 caracteres, deben incluir mayúsculas y minúsculas, deben, tener números, deben tener caracteres especiales,  y que se guarde de forma persistente para que no se pierda al reiniciar: 
```bash
SET PERSIST validate\_password.length \= 14;  
SET PERSIST validate\_password.policy \= STRONG;  
SET PERSIST validate\_password.mixed\_case\_count \= 1;  
SET PERSIST validate\_password.number\_count \= 1;  
SET PERSIST validate\_password.special\_char\_count \= 1;
```
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180148%20-%20copia.png)

4\. Desactivamos funciones peligrosas:  
	Evitar ataques vía carga de archivos:  

	SET PERSIST local\_infile \= OFF;

![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180153.png) 
Comprobación:  

	SHOW VARIABLES LIKE 'local\_infile';  
	
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180158.png)
5\. Activamos el modo SQL estricto:  
Evitar datos corruptos silenciosos:  

	SET PERSIST sql\_mode \= 'STRICT\_TRANS\_TABLES,ERROR\_FOR\_DIVISION\_BY\_ZERO,NO\_ENGINE\_SUBSTITUTION';  
	
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180208.png)

6\. Limitamos el abuso de conexiones:  
Limitamos el número máximo de conexiones simultaneas que puede aceptar Mysql:  

	SET PERSIST max\_connections \= 150;
	
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180214.png)

Limitamos los intentos fallidos de conexiones desde una misma IP, si una ip falla 10 veces seguidas el MYSQL bloqueará automáticamente la ip:  

	SET PERSIST max\_connect\_errors \= 10;  
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180219.png)

Limitamos por usuario:  

	ALTER USER 'extauser'@'172.31.%' WITH MAX\_USER\_CONNECTIONS 20;  
	ALTER USER 'extauser'@'172.17.%' WITH MAX\_USER\_CONNECTIONS 20;  
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180226.png)

7\. Activamos los logs de consultas lentas:  
Activa el registro de consultas lentas (Slow Query Log):  

	SET PERSIST slow\_query\_log \= ON; 
	
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180232.png)  

Define el tiempo (en segundos) que debe tardar una consulta para considerarse “lenta”:  

	SET PERSIST long\_query\_time \= 2;  
	
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180236.png) 

8\. Verificación de todo lo configurado:  
	Comprobamos los usuarios existente y desde donde se pueden conectar:  
	
	SELECT user, host FROM mysql.user;  
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180243.png)  

Permisos exactos del usuario de la aplicación:  

	SHOW GRANTS FOR 'extauser'@'172.31.%';  
	SHOW GRANTS FOR 'extauser'@'172.17.%';  
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180252.png)

Comprobamos que la política de contraseñas fuerte está activa:
	
	SHOW VARIABLES LIKE 'validate\_password%';  
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180303.png)

Comprobamos  que LOCAL INFILE está desactivado, LOCAL INFILE permite cargar archivos del sistema al servidor:  

	SHOW VARIABLES LIKE 'local\_infile';  
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180309.png)

Comprobamos que el modo SQL estricto este activo:  

	SHOW VARIABLES LIKE 'sql\_mode';  
![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/333b1c49bcb44cd828594262029c33c248c90ebe/sprint_4/img/Screenshot%202026-02-25%20180317.png)




















