# Sprint 5 – Monitorización y Automatización

## Arquitectura final del sistema

La arquitectura final del proyecto Extagram está compuesta por varios servicios distribuidos en diferentes instancias de AWS y contenedores Docker. El objetivo es separar responsabilidades entre los distintos componentes de la aplicación para mejorar la escalabilidad y facilitar la monitorización del sistema.

La arquitectura se compone de los siguientes elementos:

- **S1 – Load Balancer:** encargado de distribuir el tráfico entrante entre los servidores de aplicación.
- **S2 y S3 – Servidores Extagram:** ejecutan la aplicación principal en contenedores Docker con PHP.
- **S4 – Upload Server:** gestiona la subida de imágenes por parte de los usuarios.
- **S5 – Image Server:** sirve las imágenes almacenadas a los clientes.
- **S6 – Static Server:** proporciona contenido estático de la aplicación.
- **S7 – Base de datos MySQL:** almacena la información de la aplicación Extagram.

Para la monitorización del sistema se ha implementado una solución basada en **Grafana, Loki y Promtail**.

- **Promtail** se ejecuta en la instancia donde se encuentra el contenedor MySQL (S7) y se encarga de recoger los logs del contenedor Docker.
- **Loki** se ejecuta en la instancia donde se encuentran los servidores de aplicación y almacena los logs recibidos.
- **Grafana** permite visualizar los logs y analizar el comportamiento del sistema en tiempo real.

---

## Configuración realizada en cada instancia

### Instancia 2

En esta instancia se desplegaron los siguientes contenedores Docker:

- **Grafana:** plataforma utilizada para visualizar los logs del sistema.
- **Loki:** sistema de almacenamiento de logs.
- **S2 y S3:** servidores de aplicación que ejecutan el servicio Extagram.
- **S4:** servidor encargado de gestionar la subida de archivos.
- **S5:** servidor encargado de servir las imágenes almacenadas.
- **S6:** servidor que proporciona contenido estático de la aplicación.

También se configuró la conexión entre **Grafana y Loki** para poder consultar los logs generados por los servicios monitorizados.

---

### Instancia 3

En esta instancia se encuentra desplegado el contenedor:

- **S7 – MySQL**, que actúa como base de datos del sistema.

Además, se desplegó un contenedor **Promtail** cuya función es recoger los logs generados por el contenedor de MySQL. Promtail monitoriza el archivo de logs generado por Docker (`*-json.log`) y envía estos registros al servidor Loki que se encuentra en la Instancia 2.

Gracias a esta configuración es posible centralizar los logs del sistema y analizarlos desde Grafana.
