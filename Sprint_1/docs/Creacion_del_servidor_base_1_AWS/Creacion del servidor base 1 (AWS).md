# **Creación del servidor base 1 (AWS)**

**Introducción**

Tras una reunión grupal, dando nuestros puntos de vista entre las opciones de la ejecución de este proyecto en el “nuvolet” o en AWS, hemos llegado a la decisión de utilizar el servicio Amazon Web Service.

**Por qué en aws?**

**AWS:**  
\- Versatilidad a la hora de escoger servicios y configuración.  
\- Mayor soporte y estabilidad  
\- Conocer una herramienta que utilizan muchas empresas de manera real (hacear curriculum)  
\- Mayor seguridad  
\- Més disponibilitat  
\- Mayor escalabilidad  
\- No tener que utilizar la pòtencia de la máquina (puede limitarnos)

**Nuvulet:**  
\- Más fácil de utilizar y más intuitivo(creación de máquinas)  
\- Acceder de una forma más fácil  
\- No es de pago (no nos limita el tiempo a la hora de utilizar la máquinas virtuales)

**Conclusión:**  
Pese a que AWS es más complejo de utilizar, podemos aprender a utilizar herramientas que se utilizan en las empresas, con lo cual podemos adquirir conocimientos y aplicarlo en un futuro. También hemos tenido en cuenta el hecho de que AWS ofrece servicios más estables, lo cual es una ventaja para el proyecto.

**Creación del servidor en AWS**

Dentro de Amazon Web Service crearemos una instancia llamada **MainServer-01**.  
Este nombre es debido a que comenzaremos con un único servidor ofreciendo todos los servicios. El “01” es porque somos el grupo 1 de la clase.

**Creación de una VPC**

| El primer paso que llevaremos a cabo será crear una VPC. Básicamente para tener una red aislada dentro de la nube. Esto nos servirá sobre todo en un futuro, cuando queramos segmentar los servicios entre varios servidores.  ![image1](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a098a352f304909028e79d383fc9af785a167525/docs/Creacion_del_servidor_base_1_AWS/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-25-58.png)  El nombre de la VPC será “VPC-G01”, haciendo referencia a que somos el grupo 1\. IP de red: 192.168.1.0/24 ![image2](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a098a352f304909028e79d383fc9af785a167525/docs/Creacion_del_servidor_base_1_AWS/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-26-22.png) |
| :---- |

**Creación de un Security Group**

| La creación de un grupo de seguridad es necesaria para asignarles reglas firewall a las máquinas, tanto a la base que crearemos como a los futuros servidores. ![image3](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a098a352f304909028e79d383fc9af785a167525/docs/Creacion_del_servidor_base_1_AWS/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-26-28.png) En los detalles básicos le asignamos un nombre al grupo de seguridad, una descripción y especificamos la VPC que hemos creado previamente. ![image4](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a098a352f304909028e79d383fc9af785a167525/docs/Creacion_del_servidor_base_1_AWS/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-26-44.png) Reglas de entrada:  ![image5](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a098a352f304909028e79d383fc9af785a167525/docs/Creacion_del_servidor_base_1_AWS/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-26-52.png) Reglas de salida: ![image6](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a098a352f304909028e79d383fc9af785a167525/docs/Creacion_del_servidor_base_1_AWS/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-26-59.png) |
| :---- |

**Creación de la EC2**

| En la sección ec2, localizamos el apartado de “Lanzar la instancia” y le damos al botón con el mismo nombre. ![image7](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a098a352f304909028e79d383fc9af785a167525/docs/Creacion_del_servidor_base_1_AWS/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-27-07.png) Datos de la instancia: Nombre: MainServer-01 Imagen: Linux 2023 kernel-6.1 Tipo de instancia: t3.micro Par de claves: vockey VPC: BIJc-G01 (previamente creado) Almacenamiento: (predeterminado) ![image8](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a098a352f304909028e79d383fc9af785a167525/docs/Creacion_del_servidor_base_1_AWS/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-27-13.png) ![image9](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a098a352f304909028e79d383fc9af785a167525/docs/Creacion_del_servidor_base_1_AWS/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-27-21.png) ![image10](https://github.com/CarlosGarcia-ITB2425/Project0.1-grup1/blob/a098a352f304909028e79d383fc9af785a167525/docs/Creacion_del_servidor_base_1_AWS/imgs/Captura%20de%20pantalla%20de%202026-01-19%2016-27-43.png) |
| :---- |

