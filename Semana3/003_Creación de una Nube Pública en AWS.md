![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Creación de una Nube Pública en AWS**

**MediaStream** es una empresa ficticia que desea ofrecer un servicio de streaming de contenido para usuarios de todo el mundo. La solución requiere alta escalabilidad, disponibilidad global y un tiempo mínimo de configuración. Para esto, deciden implementar una nube pública utilizando servicios de AWS.

------

### **Requisitos de la Empresa:**

1. Almacenamiento de Contenido:
   - Videos y archivos estáticos (imágenes, CSS, JavaScript) accesibles globalmente.
2. Computación Escalable:
   - Instancias que procesen las solicitudes de los usuarios y sirvan contenido dinámico.
3. Balanceo de Carga:
   - Distribuir las solicitudes entre múltiples instancias para mejorar el rendimiento.
4. Entrega Global:
   - Acelerar la distribución de contenido a través de una red de entrega de contenido (CDN).
5. Base de Datos:
   - Almacenar información sobre los usuarios y sus preferencias.
6. Alta Disponibilidad:
   - Implementación en múltiples regiones para garantizar disponibilidad incluso en caso de fallos.

------

### **Arquitectura Planeada:**

- **S3:** Para almacenar contenido estático y videos.
- **CloudFront:** Para distribuir contenido globalmente a baja latencia.
- **EC2:** Instancias para procesar solicitudes dinámicas.
- **RDS (MySQL):** Base de datos relacional para almacenar datos de usuarios.
- **Elastic Load Balancer:** Para distribuir tráfico entre las instancias EC2.
- **Auto Scaling Group:** Escalar las instancias EC2 según la demanda.

------

### **Paso a Paso: Implementación de la Nube Pública en AWS**

#### **1. Configurar el Almacenamiento del Contenido**

1. **Crear un Bucket S3:**
   - Ve a la consola de AWS y selecciona **S3**.
   - Haz clic en "Crear Bucket".
   - Asigna un nombre único al bucket, como `mediastream-content`.
   - Configura el bucket como "Público" para permitir que los usuarios accedan al contenido.
   - Sube archivos estáticos (videos, imágenes, etc.) al bucket.
2. **Habilitar Políticas de Acceso Público:**
   - Ve a las configuraciones del bucket.
   - Configura una política para permitir acceso público a los archivos estáticos.

------

#### **2. Configurar la Entrega Global del Contenido**

1. Configurar CloudFront:
   - Ve a **CloudFront** en la consola.
   - Crea una nueva distribución.
   - Selecciona el bucket S3 como origen.
   - Configura los ajustes para permitir acceso público y optimizar para streaming de videos.
   - Genera la URL de distribución para acceder al contenido globalmente.

------

#### **3. Configurar la Computación Escalable**

1. **Crear Instancias EC2:**
   - Ve a **EC2** y selecciona "Lanzar Instancia".
   - Elige Amazon Linux o Ubuntu como sistema operativo.
   - Selecciona el tamaño de la instancia (t2.micro para pruebas).
   - Configura el almacenamiento, red y grupos de seguridad para permitir acceso HTTP/HTTPS.
   - Lanza la instancia y usa un cliente SSH para instalar un servidor web (como Apache o Nginx).
2. **Configurar Auto Scaling Group:**
   - Ve a **Auto Scaling Groups** y selecciona "Crear".
   - Define un grupo con reglas para escalar automáticamente las instancias según el uso de CPU o el tráfico entrante.
3. **Configurar un Elastic Load Balancer (ELB):**
   - Ve a **Elastic Load Balancer** y selecciona "Crear Balanceador de Carga".
   - Configura el balanceador para enrutar tráfico a las instancias EC2.
   - Agrega las instancias EC2 al grupo del balanceador.

------

#### **4. Configurar la Base de Datos**

1. Crear una Base de Datos RDS:
   - Ve a **RDS** y selecciona "Crear Base de Datos".
   - Elige MySQL como motor de base de datos.
   - Selecciona "Free Tier" o configura según las necesidades de tu aplicación.
   - Define las credenciales de administrador y configura la conectividad pública.
   - Conecta las instancias EC2 a la base de datos utilizando su endpoint.

------

#### **5. Configurar Seguridad**

1. **Configurar Grupos de Seguridad:**
   - Configura reglas de entrada para permitir acceso HTTP (puerto 80) y HTTPS (puerto 443) a tus instancias EC2 y al balanceador de carga.
   - Configura reglas de salida para permitir a las instancias EC2 comunicarse con la base de datos RDS.
2. **Habilitar IAM Roles:**
   - Crea un rol IAM para que las instancias EC2 accedan a S3 sin exponer credenciales.
   - Asigna el rol a las instancias EC2.

------

#### **6. Validar la Arquitectura**

1. **Probar el Acceso Público:**
   - Accede a la URL generada por CloudFront para verificar que el contenido estático está disponible globalmente.
   - Usa la URL del balanceador de carga para probar las instancias EC2.
2. **Simular Cargas:**
   - Usa herramientas como Apache JMeter para simular tráfico y validar la capacidad de escalado de tu aplicación.

------

### **Resultado Final: Arquitectura Funcional**

- **Nube Pública:** Totalmente funcional, con alta escalabilidad y acceso público a través de servicios de AWS.
- **Costo Optimizado:** Uso de servicios como S3 y CloudFront reduce costos al distribuir contenido estático eficientemente.
- **Escalabilidad:** EC2 con Auto Scaling y ELB asegura que el sistema pueda manejar picos de tráfico.
- **Alta Disponibilidad:** Implementación en múltiples zonas de disponibilidad para minimizar fallos.