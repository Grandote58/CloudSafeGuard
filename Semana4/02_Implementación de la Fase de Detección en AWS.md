![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **Implementación de la Fase de Detección en AWS**

Continuamos con la empresa **"TechSolutions"**, que ofrece una plataforma en línea para gestionar proyectos. La plataforma está alojada en AWS y utiliza servicios como **Amazon EC2** (para servidores), **Amazon S3** (para almacenamiento de archivos) y **Amazon RDS** (para la base de datos). En esta fase, el objetivo es **detectar posibles amenazas o vulnerabilidades** antes de que causen daños significativos.

------

### **Paso 1: Monitoreo Continuo**

#### **Objetivo:**

Vigilar el entorno en tiempo real para detectar actividades inusuales o sospechosas.

#### **Acciones:**

1. **Usar Amazon CloudWatch:**
   - Amazon CloudWatch es como un "sistema de alarmas" que monitorea métricas y logs en tiempo real.
   - Configura CloudWatch para monitorear métricas como el uso de CPU, tráfico de red y acceso a S3.
   - Ejemplo: Si el uso de CPU de una instancia EC2 aumenta repentinamente, CloudWatch puede enviar una alerta.

### **Paso 2: Detección de Amenazas**

#### **Objetivo:**

Identificar actividades maliciosas o comportamientos anómalos.

#### **Acciones:**

1. **Implementar Amazon GuardDuty:**
   - Amazon GuardDuty es como un "detective" que analiza logs y eventos para detectar amenazas.
   - GuardDuty utiliza inteligencia artificial para identificar comportamientos sospechosos, como intentos de acceso no autorizado o actividad maliciosa desde IPs conocidas.
   - Ejemplo: Si GuardDuty detecta un intento de acceso desde una IP maliciosa, te enviará una alerta.

### **Paso 3: Análisis de Vulnerabilidades**

#### **Objetivo:**

Identificar y corregir vulnerabilidades en los recursos.

#### **Acciones:**

1. **Usar AWS Inspector:**
   - AWS Inspector es como un "inspector de seguridad" que evalúa la configuración y el estado de las instancias EC2 y aplicaciones.
   - Inspector realiza evaluaciones automáticas y genera informes con recomendaciones para corregir vulnerabilidades.
   - Ejemplo: Si Inspector encuentra que una instancia EC2 tiene puertos abiertos innecesariamente, te recomendará cerrarlos.

### **Paso 4: Gestión de Eventos**

#### **Objetivo:**

Registrar y auditar todas las actividades en la cuenta de AWS.

#### **Acciones:**

1. **Configurar AWS CloudTrail:**
   - AWS CloudTrail es como un "registro de actividades" que graba todas las acciones realizadas en tu cuenta de AWS.
   - CloudTrail registra eventos como cambios en la configuración, accesos a recursos y acciones de usuarios.
   - Ejemplo: Si alguien cambia la política de un bucket S3, CloudTrail registrará quién lo hizo y cuándo.



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)
