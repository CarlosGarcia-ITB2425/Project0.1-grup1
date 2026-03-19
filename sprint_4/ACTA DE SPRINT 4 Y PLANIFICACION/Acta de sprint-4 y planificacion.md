# ACTA DE SPRINT 4 Y PLANIFICACIÓN

## Proyecto
Hardening de Infraestructura Extagram

---

## Sprint 4

### Integrantes
- Bryan Aguilera Nieto  
- Izan Fernandez  
- Javier  
- Giuseppe Suarez  

---

## Objetivo del Sprint

Durante este sprint se pretende implementar medidas avanzadas de seguridad en la infraestructura del sistema, enfocadas en:

- Protección de la capa web mediante WAF  
- Hardening del sistema operativo en las instancias  
- Mejora de la seguridad en la base de datos MySQL  
- Auditoría y validación de la seguridad del sistema  

---

## Plan de Trabajo

### 1. Hardening del Sistema Operativo

Se realizará la configuración de seguridad en las instancias:

- Configuración segura de SSH (deshabilitar acceso root, limitar intentos, etc.)  
- Implementación de firewall (UFW o Firewalld)  
- Restricción de puertos y servicios innecesarios  
- Ejecución de auditorías con herramientas como Lynis  

Resultado esperado: Reducción de la superficie de ataque del sistema.

---

### 2. Implementación de WAF (ModSecurity)

Se procederá a desplegar un Web Application Firewall en Nginx:

- Instalación de ModSecurity  
- Configuración inicial en modo DetectionOnly  
- Integración de reglas OWASP CRS  
- Monitorización de logs de tráfico  
- Simulación de ataques para validar el funcionamiento  

Resultado esperado: Capacidad de detección de ataques en capa de aplicación.

---

### 3. Hardening de MySQL

Se aplicarán medidas de seguridad en la base de datos:

- Revisión y eliminación de usuarios inseguros  
- Restricción de accesos por IP  
- Configuración de políticas de contraseñas fuertes  
- Activación de logs y modo SQL estricto  
- Limitación de conexiones simultáneas  

Resultado esperado: Protección de la base de datos frente a accesos no autorizados.

---

### 4. Seguridad de Red (Firewall Interno)

Se configurará un firewall a nivel de instancia:

- Permitir únicamente tráfico necesario (HTTP y SSH)  
- Aplicar el principio de mínimo privilegio  
- Validar reglas configuradas  

Resultado esperado: Control del tráfico y reducción de accesos no autorizados.

---

## Resultados Esperados

- Mejora del nivel de seguridad global del sistema  
- Reducción de vulnerabilidades  
- Sistema preparado para entornos más exigentes  
- Validación mediante auditorías y pruebas de seguridad  

---

## Riesgos Identificados

- Posibles falsos positivos en el WAF  
- Problemas de compatibilidad con configuraciones del sistema  
- Complejidad en la configuración de reglas de seguridad  

---

## Planificación Sprint 5

### Objetivos

- Activar el WAF en modo bloqueo  
- Optimizar las configuraciones de seguridad  
- Implementar monitorización avanzada  
- Finalizar la documentación del proyecto  
- Preparar la entrega final  

---

### Tareas

- Ajuste de reglas de ModSecurity  
- Revisión completa del sistema  
- Ejecución de pruebas de seguridad  
- Elaboración de documentación final  
- Preparación de presentación  

---


---

## Conclusión

El Sprint 4 estará enfocado en reforzar la seguridad del sistema mediante la implementación de medidas en diferentes capas (sistema operativo, red, aplicación y base de datos), con el objetivo de disponer de una infraestructura más robusta y segura.
