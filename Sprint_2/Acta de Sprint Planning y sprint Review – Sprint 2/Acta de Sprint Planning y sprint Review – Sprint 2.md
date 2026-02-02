# Acta de Sprint Planning – Sprint 2

## Fecha
31 de enero de 2026

## Participantes
- Javier Méndez  
- Carlos García  
- Izan Fernández  
- Bryan Aguilera  

## Objetivo del Sprint
Desplegar la arquitectura del sistema en AWS utilizando Docker, separando los servicios en tres instancias y asegurando la persistencia de datos y la conectividad interna.

## Decisiones tomadas
- Se utilizarán **3 instancias EC2** en AWS:
  - Instancia 1: Proxy / Load Balancer (S1)
  - Instancia 2: Aplicación y servidores estáticos (S2–S6)
  - Instancia 3: Base de datos MySQL (S7)
- La comunicación entre instancias se realizará mediante **IP privada dentro de la VPC**.
- Se utilizarán **contenedores Docker** para todos los servicios.
- Se configurarán **volúmenes Docker** para garantizar la persistencia de datos.

## Reparto de tareas
- **Bloque 1 (Infraestructura AWS):** Javier Méndez  
- **Bloque 2 (Proxy y estáticos):** Carlos García  
- **Bloque 3 (Backend PHP):** Izan Fernández  
- **Bloque 4 (Base de datos y documentación):** Bryan Aguilera  

## Herramientas
- AWS EC2  
- Docker  
- GitHub  
- ProofHub  

# Acta de Sprint Review – Sprint 2

## Trabajo realizado
- Creación de las 3 instancias EC2 en AWS.
- Instalación y configuración de Docker en todas las instancias.
- Despliegue del contenedor MySQL (S7) con volumen persistente `dbdata/`.
- Verificación de la conectividad entre la aplicación y la base de datos mediante IP privada.
- Configuración inicial del esquema de red del sistema.
- Pruebas de acceso y persistencia de datos tras reinicios.

## Resultados
- La base de datos mantiene los datos tras reiniciar contenedores.
- La comunicación entre instancias funciona correctamente.
- La arquitectura cumple con el diseño propuesto.

## Problemas detectados
- Dificultades iniciales con la instalación del cliente MySQL en Amazon Linux 2023.
- Ajustes necesarios en repositorios y dependencias.

## Pendiente para el siguiente sprint
- Configuración completa del proxy Apache.
- Despliegue final de los contenedores de la aplicación y estáticos.
- Pruebas de balanceo y tolerancia a fallos.
- Mejora de la documentación técnica.

## Evidencias
Se adjuntan capturas de progreso y seguimiento del proyecto en ProofHub.

