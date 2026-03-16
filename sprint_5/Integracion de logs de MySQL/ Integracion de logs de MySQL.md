# **Integración de logs de MySQL**

1. Activamos los logs de MYSQL:  
   Activamos los registros de actividad sober el funcionamiento del servidor MySQL, esto para poder detectar errores del sistema, analizar consultas ejecutadas, identificar problemas de rendimiento y detectar posibles accesos no autorizados:
    
         SHOW VARIABLES LIKE 'general\_log';  
         SET GLOBAL general\_log \= 'ON';  
         SHOW VARIABLES LIKE 'general\_log\_file';  
         SHOW VARIABLES LIKE 'log\_error';  
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a450db75efcef52d4b9fdde37e62f693b65042d6/sprint_5/Integracion%20de%20logs%20de%20MySQL/img/Screenshot%202026-03-16%20154235.png)  
2. Verificamos la ubicación de los logs del mysql:
Podemos ver que un log  “\[SERVER\] ready for connection:  

         docker logs s7-mysql  
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a450db75efcef52d4b9fdde37e62f693b65042d6/sprint_5/Integracion%20de%20logs%20de%20MySQL/img/Screenshot%202026-03-16%20154242.png)

3. Podemos ver que los logs que guarda docker como en el host como “/var/lib/docker/containers/860359e92a88/860359e92a88-json.log” estos archivos son que el Promtail va a leer:
    
         ls \-l /var/lib/docker/containers/310c011156fb70a610d9ffa8cb5228b3c806cdb82b1a8abc1ff3471fccc1290d/\*  
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a450db75efcef52d4b9fdde37e62f693b65042d6/sprint_5/Integracion%20de%20logs%20de%20MySQL/img/Screenshot%202026-03-16%20154249.png)
5. Recogemos los logs con promatail:  
   Primero abrimos el archivo:
   
         nano promtail/promtail-config.yaml  
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a450db75efcef52d4b9fdde37e62f693b65042d6/sprint_5/Integracion%20de%20logs%20de%20MySQL/img/Screenshot%202026-03-16%20154255.png)

   Y pegamos lo siguiente dentro del archivo:
   
         server:  
           http\_listen\_port: 9080  
           grpc\_listen\_port: 0  
         positions:  
           filename: /tmp/positions.yaml  
         clients:  
           \- url: http://172.31.31.80:3100/loki/api/v1/push  
         scrape\_configs:  
           \- job\_name: system\_s7  
             static\_configs:  
             \- targets:  
                 \- localhost  
               labels:  
                 job: varlogs\_s7  
                 instance: instancia-3  
                 \_\_path\_\_: /var/lib/docker/containers/860359e92a88/860359e92a88-json.log  
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a450db75efcef52d4b9fdde37e62f693b65042d6/sprint_5/Integracion%20de%20logs%20de%20MySQL/img/Screenshot%202026-03-16%20154302.png)
      
   Dentro de la carpeta promtail lanazamos el contenedor:
   
         sudo docker rm \-f promtail  
         sudo docker run \-d \\  
           \--name promtail \\  
           \-v /home/ec2-user/promtail:/etc/promtail \\  
           \-v /var/lib/docker/containers:/var/lib/docker/containers:ro \\  
           grafana/promtail:latest \\  
           \-config.file=/etc/promtail/promtail-config.yaml  
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a450db75efcef52d4b9fdde37e62f693b65042d6/sprint_5/Integracion%20de%20logs%20de%20MySQL/img/Screenshot%202026-03-16%20154309.png)

   Comprobamos que se estén enviando logs correctamente:
   
         docker logs \-f promtail  
   ![](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a450db75efcef52d4b9fdde37e62f693b65042d6/sprint_5/Integracion%20de%20logs%20de%20MySQL/img/Screenshot%202026-03-16%20154315.png)
   
Al revisar los logs generados por Promtail pode ver que el sistema está monitorizando correctamente el contenedor s7-mysql y enviando los eventos al servidor Loki. Gracias a esta monitorización es posible detectar actividades del servidor de base de datos, como por ejemplo intentos fallidos de acceso. En este caso, al intentar conectarse con credenciales incorrectas a MySQL, se genera un registro de error que queda almacenado en los logs y puede visualizarse posteriormente en Grafana, permitiendo identificar y analizar posibles problemas o intentos de acceso no autorizados.  
