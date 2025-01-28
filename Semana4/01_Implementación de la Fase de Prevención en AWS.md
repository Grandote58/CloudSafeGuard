![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **Implementación de la Fase de Prevención en AWS**



Imagina que trabajas en una empresa llamada **"TechSolutions"**, que ofrece una plataforma en línea para gestionar proyectos. La plataforma está alojada en AWS y utiliza servicios como **Amazon EC2** (para servidores), **Amazon S3** (para almacenamiento de archivos) y **Amazon RDS** (para la base de datos). Tu tarea es implementar medidas de prevención para proteger la plataforma.

------

### **Paso 1: Identificación de Activos**

#### **Objetivo:**

Saber qué recursos existen en tu entorno de AWS y cómo están configurados.

#### **Acciones:**

1. **Usar AWS Config:**
   - AWS Config es como un "inventario inteligente" que te permite ver todos los recursos de AWS en tu cuenta.
   - Configura AWS Config para monitorear recursos como instancias EC2, buckets S3 y bases de datos RDS.
   - Ejemplo: Si alguien crea un bucket S3 sin cifrado, AWS Config te alertará.



### **Paso 2: Gestión de Identidades y Accesos (IAM)**

#### **Objetivo:**

Asegurarte de que solo las personas y servicios autorizados puedan acceder a los recursos.

#### **Acciones:**

1. **Crear Usuarios y Roles:**
   - Usa **AWS IAM** para crear usuarios y roles con permisos específicos.
   - Ejemplo: Crea un rol llamado "Desarrollador" que solo permita acceso de lectura a los buckets S3.
2. **Implementar MFA (Autenticación Multifactor):**
   - Requiere que los usuarios habiliten MFA para acceder a la consola de AWS.
   - Ejemplo: Es como poner un segundo candado en la puerta de tu casa.

### **Paso 3: Cifrado de Datos**

#### **Objetivo:**

Proteger los datos para que no puedan ser leídos por personas no autorizadas.

#### **Acciones:**

1. **Cifrado en Reposo:**
   - Usa **AWS KMS (Key Management Service)** para crear claves de cifrado.
   - Aplica cifrado a los buckets S3 y a las bases de datos RDS.
   - Ejemplo: Si alguien roba un disco duro, no podrá leer los datos sin la clave.
2. **Cifrado en Tránsito:**
   - Usa **SSL/TLS** para cifrar la comunicación entre los usuarios y la plataforma.
   - Ejemplo: Es como enviar una carta en un sobre sellado en lugar de una postal.

#### **Explicación para Estudiantes:**

- El cifrado es como un "lenguaje secreto" que solo tú y tus amigos (los servicios autorizados) pueden entender. Si alguien intercepta el mensaje, no podrá leerlo.

### **Paso 4: Protección de Redes**

#### **Objetivo:**

Proteger la plataforma de ataques externos.

#### **Acciones:**

1. **Configurar Grupos de Seguridad (Security Groups):**
   - Usa Security Groups para controlar el tráfico hacia y desde las instancias EC2.
   - Ejemplo: Permite solo tráfico HTTP/HTTPS desde cualquier lugar, pero bloquea el acceso SSH desde IPs desconocidas.
2. **Implementar AWS WAF y AWS Shield:**
   - Usa **AWS WAF** para proteger la aplicación web de ataques como inyecciones SQL.
   - Usa **AWS Shield** para mitigar ataques DDoS.
   - Ejemplo: AWS WAF es como un "filtro" que bloquea mensajes maliciosos, y AWS Shield es como un "escudo" que protege contra ataques masivos.

### **Paso 5: Cumplimiento y Auditoría**

#### **Objetivo:**

Asegurarte de que la plataforma cumple con las normativas de seguridad.

#### **Acciones:**

1. **Usar AWS Artifact:**
   - Descarga informes de cumplimiento como ISO 27001 o GDPR desde AWS Artifact.
   - Ejemplo: Es como obtener un "certificado de seguridad" para tu casa.
2. **Configurar AWS CloudTrail:**
   - Usa CloudTrail para registrar todas las actividades en tu cuenta de AWS.
   - Ejemplo: Es como tener una "cámara de seguridad" que graba todo lo que sucede.



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete2.png)
