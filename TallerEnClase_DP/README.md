**DEFENSA EN PROFUNDIDAD**

El modelo de **Defensa en Profundidad** es una estrategia de seguridad que se basa en la implementación de **múltiples capas de protección** para salvaguardar los activos de una organización, especialmente los datos y sistemas informáticos. Su origen se remonta a la terminología militar, donde se utilizan varias líneas defensivas consecutivas para desgastar y ralentizar al enemigo, en lugar de depender de una única y fuerte barrera.

En el ámbito de la ciberseguridad, la idea es que **ninguna capa de seguridad es infalible**. Si un atacante logra superar una defensa (por ejemplo, un firewall), se encontrará con otra capa de seguridad que intentará detenerlo (como un sistema de detección de intrusos, cifrado de datos o controles de acceso).

Este modelo se enfoca en tres aspectos clave:

* **Defensas Físicas:** Incluyen medidas para proteger los equipos y las instalaciones, como guardias de seguridad, cámaras de vigilancia, cerraduras, sistemas biométricos, etc.
* **Defensas Técnicas:** Se refieren a las soluciones tecnológicas implementadas, como firewalls, software antivirus y antimalware, sistemas de prevención de intrusiones (IPS), cifrado de datos, autenticación multifactor, segmentación de red, etc.
* **Defensas Administrativas/Organizativas:** Abarcan las políticas, procedimientos y la capacitación del personal, como la gestión de acceso, planes de respuesta a incidentes, concientización sobre seguridad, auditorías de seguridad, etc.

En resumen, la defensa en profundidad busca crear un **escudo multicapa y redundante** que dificulte el avance de un atacante, le dé tiempo a la organización para detectar y responder a la amenaza, y minimice el impacto en caso de una brecha en alguna de las capas.

---
**Trabajo en Grupo**

Desarrollar y documentar en github el siguiente reto.  

**Nota**
Por cada grupo de trabajo crear una carpeta con un README con la resolución.

---

## Defensa en Profundiadad: Fortaleza Digital de "TechSolutions Corp."


**Contexto:**
TechSolutions Corp. es una empresa global líder en el desarrollo de software y servicios en la nube, con una base de clientes masiva y una vasta cantidad de datos sensibles, incluyendo información financiera, propiedad intelectual y datos personales de clientes. Su infraestructura de TI es compleja, abarcando centros de datos locales, entornos de nube híbrida (AWS y Azure), una red de oficinas distribuidas globalmente y una fuerza laboral remota significativa.

**El Incidente**
Es el 10 de junio de 2025. Su equipo como expertos en ciberseguridadha detectado una serie de actividades anómalas:

* **Fase 1 (Vectores de Ataque Iniciales):**
    * Múltiples empleados han recibido correos electrónicos de *phishing* altamente sofisticados, suplantando a la dirección de TI, solicitando credenciales de acceso a un "nuevo portal de empleados".
    * Se ha identificado un intento de explotación de una vulnerabilidad de día cero en un servidor web público que ejecuta un software de gestión de proyectos obsoleto (versión no parcheada) dentro de la DMZ.
    * Un empleado de reciente contratación descargó accidentalmente un archivo adjunto malicioso desde una red social profesional (LinkedIn) que se hizo pasar por una oferta de capacitación.

* **Fase 2 (Movimiento Lateral y Persistencia):**
    * Parece que el atacante ha logrado comprometer una estación de trabajo de ingeniería de software a través del *phishing* exitoso, utilizando credenciales robadas.
    * Desde esta estación de trabajo, se han observado intentos de escaneo de red interno y de elevación de privilegios.
    * Hay indicios de que el atacante está intentando establecer persistencia mediante la creación de cuentas de usuario ocultas y la modificación de tareas programadas en algunos sistemas comprometidos.

* **Fase 3 (Objetivo Final - Exfiltración/Destrucción):**
    * La actividad principal del atacante parece dirigirse a la base de datos de propiedad intelectual (ubicada en la nube) y a los servidores de desarrollo que contienen el código fuente de los productos estrella de la empresa.
    * Se han detectado grandes volúmenes de tráfico saliente inusual hacia direcciones IP externas no identificadas.
    * En algunos sistemas críticos, se han encontrado archivos con extensiones cifradas y notas de rescate, lo que evidencia una actividad de *ransomware* como un segundo vector de ataque o distracción.

**La Tarea:**
Como equipo de seguridad de TechSolutions Corp., su tarea es desarrollar una estrategia de **defensa en profundidad** integral para mitigar el impacto de este incidente y proteger a la empresa contra futuros ataques. No se trata solo de reaccionar al incidente actual, sino de establecer una arquitectura de seguridad resiliente.

**Instrucciones para los Estudiantes:**

1.  **Análisis de Amenazas y Vulnerabilidades:** Identifiquen las principales amenazas y vulnerabilidades expuestas en la narrativa.
2.  **Principios de Defensa en Profundidad:** Expliquen cómo aplicarían los principios de defensa en profundidad (capas de seguridad) en este escenario.
3.  **Capas de Defensa Propuestas:** Diseñen una estrategia detallada de defensa en profundidad, especificando las medidas de seguridad y tecnologías clave en cada una de las siguientes capas:
    * **Capa 1: Perímetro/Red Externa** 
    * **Capa 2: Red Interna/Segmentación** 
    * **Capa 3: Endpoint/Dispositivos** 
    * **Capa 4: Aplicaciones** 
    * **Capa 5: Datos** 
    * **Capa 6: Identidad y Acceso** 
    * **Capa 7: Operaciones y Concienciación** 
    
4.  **Respuesta al Incidente:** Describan los pasos iniciales que tomarían para contener y erradicar el incidente actual basándose en su estrategia de defensa en profundidad.
5.  **Monitoreo y Mejora Continua:** ¿Cómo garantizarían que su estrategia de defensa en profundidad se mantenga efectiva a lo largo del tiempo?
