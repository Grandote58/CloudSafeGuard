![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **Práctica: Reconociendo Procesos y Protocolos de Seguridad en la Nube**

TechNova Inc. es una empresa tecnológica que desarrolla y opera una plataforma web para la gestión de proyectos colaborativos (similar a Trello o Asana). Sus servicios incluyen:

1. Una **aplicación web** accesible para los usuarios finales.
2. Una **API REST** utilizada por clientes y socios comerciales para integrarse con sus sistemas.
3. Almacenamiento en la nube para **archivos de proyectos y datos sensibles de los usuarios**.

La empresa está migrando su infraestructura a AWS y necesita garantizar altos estándares de **seguridad en la nube**, implementando los **procesos y protocolos correctos**.

### **Objetivo de la práctica**

Diseñar e implementar una arquitectura en AWS que cumpla con los siguientes requisitos:

- **Protección de datos**: cifrado en reposo y en tránsito.
- **Seguridad de red**: configurar VPC, subredes, y reglas de firewall.
- **Gestión de accesos e identidades (IAM)**: definir roles, permisos y políticas.
- **Supervisión y respuesta**: implementar monitoreo continuo y alertas.
- Cumplir con principios del **AWS Well-Architected Framework** y el **modelo de responsabilidad compartida**.

------

## **Parte 1: Arquitectura base de TechNova Inc. en AWS**

### **Diagrama de la arquitectura**

1. **VPC**:
   - Una VPC con **dos subredes públicas** y **dos subredes privadas** distribuidas en **dos zonas de disponibilidad (AZs)** para alta disponibilidad.
2. **Servicios clave**:
   - Un **Application Load Balancer (ALB)** en la subred pública para distribuir tráfico HTTP/HTTPS.
   - Servidores EC2 en las subredes privadas para la aplicación backend.
   - Una base de datos administrada (Amazon RDS) en las subredes privadas.
   - Un bucket S3 con cifrado habilitado para almacenar archivos de usuario.
3. **Seguridad**:
   - **Security Groups** para restringir tráfico entrante y saliente.
   - Uso de **AWS WAF** para proteger contra ataques como SQL Injection y XSS.
   - **KMS** para el cifrado de datos sensibles.

![PracticaArquitectura_001](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/PracticaArquitectura_001.png)

### **Requisitos específicos en seguridad**

| Área                      | Implementación técnica en AWS                                |
| ------------------------- | ------------------------------------------------------------ |
| **Red**                   | - Crear VPC con subredes públicas y privadas.  - Usar NACLs para filtrar tráfico a nivel de subred.  - Configurar Security Groups para EC2 y RDS. |
| **Cifrado de datos**      | - Activar cifrado en reposo con KMS para S3 y RDS.  - Habilitar HTTPS con certificados de ACM en el ALB. |
| **Gestión de accesos**    | - Implementar roles IAM específicos para desarrolladores y administradores.  - Habilitar MFA para todas las cuentas. |
| **Supervisión y alertas** | - Configurar CloudWatch para logs y métricas.  - Implementar AWS GuardDuty y AWS Config para auditorías y alertas. |

------

## **Parte 2: Paso a Paso**

### **Paso 1: Configuración de la VPC y subredes**

1. **Crea una VPC** con un rango de direcciones CIDR, por ejemplo, `10.0.0.0/16`.
2. Crea **2 subredes públicas** (`10.0.1.0/24` y `10.0.2.0/24`) y **2 subredes privadas** (`10.0.3.0/24` y `10.0.4.0/24`).
3. Configura una **Gateway de Internet (IGW)** y conéctala a la VPC.
4. Añade una **NAT Gateway** en la subred pública para que las instancias privadas puedan acceder a internet de manera segura.

### **Paso 2: Configuración de seguridad de red**

1. Crea Security Groups:
   - Para el ALB: permite tráfico entrante en el puerto 443 (HTTPS) desde cualquier dirección pública.
   - Para EC2: permite tráfico entrante solo desde el ALB (puerto 80 o 443).
   - Para RDS: permite tráfico entrante solo desde las instancias EC2 en la VPC.
2. Configura **Network ACLs** para bloquear IPs maliciosas o accesos no deseados.

### **Paso 3: Configuración de cifrado**

1. Habilita el **cifrado en reposo** para RDS (por ejemplo, PostgreSQL o MySQL). Usa claves gestionadas por AWS (KMS).
2. En el bucket S3, habilita:
   - **Cifrado por defecto con SSE-KMS**.
   - **Bloqueo de acceso público** para evitar configuraciones erróneas.
3. Usa **ACM (AWS Certificate Manager)** para emitir un certificado SSL/TLS para el ALB.

### **Paso 4: Configuración de IAM**

1. Crea políticas IAM específicas:
   - Un rol para instancias EC2 con permisos limitados (acceso al bucket S3).
   - Políticas para desarrolladores con permisos restringidos a sus áreas de trabajo.
   - Habilita **MFA obligatorio** en la cuenta root y en los usuarios IAM.

### **Paso 5: Configuración de supervisión y respuesta**

1. Activa **CloudTrail** para auditoría de acciones en la cuenta.
2. Configura **CloudWatch** para monitorear logs del ALB, instancias EC2 y RDS.
3. Habilita **AWS GuardDuty** para identificar amenazas como accesos no autorizados o configuraciones inseguras.

------

# **Implementación de subredes y VPC en AWS**

#### 1. **Crear una VPC**

1. Ve a la consola de AWS, busca el servicio **VPC** y selecciona **Your VPCs**.

2. Haz clic en 

   Create VPC:

   - **Name tag**: `TechNova-VPC`.
   - **IPv4 CIDR block**: `10.0.0.0/16` (rango de direcciones IP para la red).
   - Selecciona **No IPv6 CIDR block** (opcional para esta práctica).
   - **Tenancy**: default (uso estándar de recursos compartidos).

3. Haz clic en **Create VPC**.

#### 2. **Crear subredes**

Crea cuatro subredes (dos públicas y dos privadas) distribuidas en **dos zonas de disponibilidad**.

1. Ve a Subnets \> Create subnet:
   - **VPC ID**: Selecciona `TechNova-VPC`.
   - Asigna los siguientes valores:

| Subred            | Zona de disponibilidad | CIDR Block  | Propósito        |
| ----------------- | ---------------------- | ----------- | ---------------- |
| Public-Subnet-1A  | us-east-1a             | 10.0.1.0/24 | Subred pública 1 |
| Public-Subnet-1B  | us-east-1b             | 10.0.2.0/24 | Subred pública 2 |
| Private-Subnet-1A | us-east-1a             | 10.0.3.0/24 | Subred privada 1 |
| Private-Subnet-1B | us-east-1b             | 10.0.4.0/24 | Subred privada 2 |

1. Marca las dos **subredes públicas** y habilita la opción **Auto-assign public IPv4** para que las instancias lancen con una dirección IP pública automáticamente.

#### 3. **Configurar una Internet Gateway**

1. Ve a Internet Gateways \> Create Internet Gateway:
   - **Name tag**: `TechNova-IGW`.
2. Una vez creada, selecciona el IGW y haz clic en Actions > Attach to VPC:
   - Asocia el IGW a la VPC `TechNova-VPC`.

#### 4. **Configurar una tabla de rutas para las subredes públicas**

1. Ve a Route Tables \> Create Route Table:
   - **Name tag**: `Public-RouteTable`.
   - **VPC ID**: Selecciona `TechNova-VPC`.
2. Selecciona esta tabla y agrega la siguiente ruta:
   - **Destination**: `0.0.0.0/0` (todo el tráfico).
   - **Target**: `TechNova-IGW`.
3. Asocia esta tabla de rutas a las dos **subredes públicas**.

#### 5. **Configurar una NAT Gateway para las subredes privadas**

1. Ve a NAT Gateways \> Create NAT Gateway:
   - **Name tag**: `TechNova-NAT`.
   - **Subnet**: Selecciona una de las subredes públicas (`Public-Subnet-1A`).
   - **Elastic IP allocation ID**: Asigna una Elastic IP.
2. Una vez creada, actualiza la tabla de rutas de las subredes privadas:
   - **Destination**: `0.0.0.0/0`.
   - **Target**: `TechNova-NAT`.

------

### **Paso 2: Configuración de Security Groups**

#### 1. **Crear Security Group para el ALB**

1. Ve a Security Groups \> Create Security Group:
   - **Name**: `SG-ALB`.
   - **Description**: Security Group para Application Load Balancer.
   - **VPC**: Selecciona `TechNova-VPC`.
2. Configura las reglas de ingreso:
   - **Type**: HTTPS.
   - **Protocol**: TCP.
   - **Port Range**: `443`.
   - **Source**: `0.0.0.0/0` (permite acceso desde cualquier lugar).
3. Configura las reglas de egreso:
   - **Type**: All traffic.
   - **Protocol**: All.
   - **Port Range**: All.
   - **Destination**: `0.0.0.0/0`.

#### 2. **Crear Security Group para instancias EC2**

1. Crea un nuevo Security Group:
   - **Name**: `SG-EC2`.
   - **Description**: Seguridad para instancias backend.
   - **VPC**: `TechNova-VPC`.
2. Configura las reglas de ingreso:
   - **Type**: HTTP.
   - **Protocol**: TCP.
   - **Port Range**: `80`.
   - **Source**: Selecciona el Security Group del ALB (`SG-ALB`).
3. Configura las reglas de egreso:
   - **Type**: All traffic.
   - **Protocol**: All.
   - **Port Range**: All.
   - **Destination**: `0.0.0.0/0`.

#### 3. **Crear Security Group para RDS**

1. Crea un nuevo Security Group:
   - **Name**: `SG-RDS`.
   - **Description**: Seguridad para la base de datos.
   - **VPC**: `TechNova-VPC`.
2. Configura las reglas de ingreso:
   - **Type**: MySQL/Aurora (o el motor de base de datos que uses).
   - **Protocol**: TCP.
   - **Port Range**: `3306`.
   - **Source**: Selecciona el Security Group de las instancias EC2 (`SG-EC2`).
3. Configura las reglas de egreso:
   - Igual que las reglas de egreso de los otros Security Groups.

![PracticaArquitectura_002](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/PracticaArquitectura_002.png)

# **Application Load Balancer (ALB) en AWS** 

### **Paso 1: Crear una instancia EC2**

1. Ve al servicio **EC2** en la consola de AWS.
2. Haz clic en Launch Instances  y configura lo siguiente:
   - **Nombre**: `Backend-Instance`.
   - **AMI**: Selecciona una AMI de Amazon Linux 2 (o la AMI de tu preferencia).
   - **Tipo de instancia**: `t2.micro` (gratis si está en el Free Tier).
   - **Red (Network)**: Selecciona la VPC `TechNova-VPC`.
   - **Subred**: Elige una de las **subredes privadas**, por ejemplo, `Private-Subnet-1A`.
   - **Auto-assign Public IP**: **Disabled** (la instancia no tendrá IP pública).
   - **Security Group**: Usa el **SG-EC2** que creaste anteriormente (permite tráfico solo desde el ALB).
3. Agrega un **par de claves (Key Pair)** para acceso SSH, o crea uno nuevo si no tienes uno.
4. Revisa los detalles y lanza la instancia.

### **Paso 2: Crear un Application Load Balancer (ALB)**

1. Ve al servicio EC2 \> Load Balancers y haz clic en Create Load Balancer:
   - **Tipo**: Selecciona **Application Load Balancer**.
   - **Nombre**: `TechNova-ALB`.
   - **Esquema**: Internet-facing.
   - **VPC**: Selecciona `TechNova-VPC`.
   - **Subredes públicas**: Selecciona `Public-Subnet-1A` y `Public-Subnet-1B`.
2. Configura Listeners:
   - Añade un **Listener** para HTTPS (puerto `443`).
   - Asocia un **Certificado SSL** desde ACM (puedes crear uno si no tienes).
3. Grupo de Seguridad (Security Group):
   - Usa el **SG-ALB** que permite tráfico HTTPS (`443`) desde cualquier lugar (`0.0.0.0/0`).
4. Configura el grupo de destino (Target Group):
   - Tipo de destino: **Instance**.
   - Nombre: `TechNova-TG`.
   - Protocolo: HTTP (puerto `80`).
   - Añade la instancia EC2 creada anteriormente como destino.
5. Revisa y crea el ALB.

### **Paso 3: Verificación de acceso**

1. Ve al DNS del ALB:
   - Desde el servicio EC2 > Load Balancers, copia el **DNS público** del ALB (algo como `TechNova-ALB-1234567890.us-east-1.elb.amazonaws.com`).
2. Abre el DNS en un navegador:
   - Si todo está configurado correctamente, deberías ver la aplicación en la instancia EC2.
   - Si intentas acceder directamente a la IP privada de la EC2 desde la VPC, no responderá (bloqueado por el **SG-EC2**).

### **Paso 4: Estructura para el diagrama**

#### **Componentes principales del diagrama**

Incluye los siguientes elementos para representar esta configuración:

1. **VPC**:
   - Representa el espacio de red que engloba toda la infraestructura (CIDR `10.0.0.0/16`).
2. **Subredes**:
   - Dibuja **dos subredes públicas** (`Public-Subnet-1A`, `Public-Subnet-1B`) y **una subred privada** (`Private-Subnet-1A`).
   - Coloca cada componente dentro de la subred correspondiente.
3. **Load Balancer (ALB)**:
   - Colócalo en las subredes públicas.
   - Muestra que acepta tráfico entrante desde internet (puerto `443`) con su Security Group (**SG-ALB**).
4. **Instancia EC2**:
   - Colócala en la subred privada.
   - Indica que su tráfico solo es accesible desde el ALB a través del puerto `80` (HTTP) gracias al Security Group (**SG-EC2**).
5. **Security Groups**:
   - **SG-ALB**: Representa que permite tráfico HTTPS desde `0.0.0.0/0`.
   - **SG-EC2**: Muestra que acepta tráfico solo desde **SG-ALB**.
6. **Conexión a Internet**:
   - Agrega una **Internet Gateway** conectada a las subredes públicas.

![PracticaArquitectura_003](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/PracticaArquitectura_003.png)

# **Subir un archivo a S3 y verificar que está cifrado**

### **Paso 1: Crear un bucket en Amazon S3 y habilitar cifrado**

1. Ve al servicio **S3** en la consola de AWS.
2. Haz clic en Create bucket y configura lo siguiente:
   - **Bucket name**: `tecnova-secure-bucket` (elige un nombre único a nivel global).
   - **Region**: Elige la misma región que la arquitectura (por ejemplo, `us-east-1`).
   - **Block Public Access settings**: Deja **todas las opciones habilitadas** para bloquear accesos públicos.
   - Habilita la opción de **Versioning** (opcional, pero útil para auditorías).
   - En Default encryption, selecciona:
     - **Enable**: Activa el cifrado.
     - **Encryption type**: Elige **SSE-KMS** y selecciona una clave KMS administrada por AWS o crea una nueva.
3. Haz clic en **Create bucket**.

### **Paso 2: Configurar un Bucket Policy para restringir el acceso desde el ALB**

1. Ve al bucket `tecnova-secure-bucket`, haz clic en la pestaña **Permissions**.
2. En la sección **Bucket Policy**, añade la siguiente política para permitir acceso solo desde el ALB:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowALBAccessOnly",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::tecnova-secure-bucket/*",
      "Condition": {
        "StringEquals": {
          "aws:SourceVpce": "vpce-XXXXXXXXXXXXXX"
        }
      }
    }
  ]
}
```

- **`aws:SourceVpce`**: Es el ID del **Endpoint VPC** que configurarás para conectar el ALB al bucket S3 (lo hacemos en el siguiente paso).

### **Paso 3: Configurar un VPC Endpoint para el bucket S3**

1. Ve al servicio **VPC** > **Endpoints** > **Create Endpoint**.
2. Configura el endpoint:
   - **Service Name**: Selecciona el servicio de S3 en tu región (por ejemplo, `com.amazonaws.us-east-1.s3`).
   - **VPC**: Selecciona `TechNova-VPC`.
   - **Subnets**: Selecciona las **subredes privadas y públicas**.
   - **Security Group**: Usa un **Security Group** que permita tráfico solo desde el ALB.
3. Haz clic en **Create Endpoint**.
4. Copia el **ID del Endpoint (vpce-XXXXXXXXXXXXXX)** e intégralo en la política del bucket.

------

### **Paso 4: Subir un archivo al bucket S3**

1. Ve al bucket `tecnova-secure-bucket` y selecciona la pestaña **Objects**.
2. Haz clic en Upload  > Add Files :
   - Sube un archivo de prueba (por ejemplo, `test-file.txt`).
3. Asegúrate de que el archivo subido esté cifrado:
   - Ve a **Properties** del archivo subido.
   - Bajo **Encryption**, verifica que dice **SSE-KMS**.

### **Paso 5: Configurar ALB para acceder al bucket S3**

1. Backend Application Configuration :
   - Configura tu aplicación backend (en la instancia EC2) para que el ALB sirva el archivo desde S3.
   - Usa el SDK de AWS o la CLI para descargar el archivo desde S3 en la aplicación backend.
   - Código ejemplo en Python (boto3):

```python
import boto3

s3 = boto3.client('s3')
bucket_name = "tecnova-secure-bucket"
file_name = "test-file.txt"
local_file_path = "/var/www/html/test-file.txt"

s3.download_file(bucket_name, file_name, local_file_path)
```

1. Prueba desde el ALB:
   - Asegúrate de que el archivo sea accesible **solo a través del ALB**. Intenta acceder directamente al bucket S3 y verifica que el acceso está bloqueado.

### **Paso 6: Verificar restricciones de acceso**

1. **Acceso directo al bucket S3**:
   - Intenta acceder al archivo usando su URL pública (algo como `https://tecnova-secure-bucket.s3.amazonaws.com/test-file.txt`).
   - Debería bloquear el acceso con un error `Access Denied`.
2. **Acceso a través del ALB**:
   - Ve al **DNS público del ALB** (algo como `TechNova-ALB-1234567890.us-east-1.elb.amazonaws.com/test-file.txt`).
   - Deberías poder descargar/ver el archivo correctamente, ya que el ALB tiene permisos para acceder al bucket.

![PracticaArquitectura_004](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/PracticaArquitectura_004.png)

### **Preguntas de reflexión**

1. ¿Por qué es importante usar subredes privadas para las instancias backend y la base de datos?
2. ¿Qué ventajas ofrece el uso de KMS para el cifrado de datos?
3. ¿Cómo protegerías el bucket S3 contra accesos accidentales desde internet?

## **Resultados esperados**

Al finalizar esta práctica, el estudiante deberá:

1. Comprender los conceptos básicos de seguridad en red, cifrado y gestión de accesos.
2. Configurar una arquitectura de red segura en AWS.
3. Implementar procesos clave para proteger datos en la nube.

![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)