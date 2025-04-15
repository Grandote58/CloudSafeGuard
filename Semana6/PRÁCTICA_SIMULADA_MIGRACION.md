![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **🛠️ PRÁCTICA SIMULADA: MIGRACIÓN SEGURA A AWS PARA CURARE S.A.**

📌 *Objetivo:* Simular la migración segura de la infraestructura TI de **Curare S.A.**, una empresa sanitaria, a **AWS**, utilizando **servicios gratuitos o de bajo costo**, garantizando **cumplimiento normativo** y mejorando **rendimiento y seguridad**.

📌 *¿Qué aprenderás?*
🔹 Evaluar la infraestructura actual y definir la **estrategia de migración**.
🔹 Implementar una **arquitectura segura en AWS** con cifrado, IAM y monitoreo.
🔹 Simular la **migración de datos y servidores** utilizando herramientas gratuitas de AWS.
🔹 Configurar servicios esenciales: **EC2, RDS, S3, VPN y CloudTrail**.

------

## **📌 1. Situación Inicial: Infraestructura de Curare S.A.**

📍 *Curare S.A.* es una empresa de servicios sanitarios que maneja **historias clínicas electrónicas (EHR), registros de pacientes y reportes médicos**. Actualmente, su infraestructura está alojada **en un datacenter físico**, lo que presenta **varios problemas**:

📊 **Estado Actual de la Infraestructura On-Premise**

| **Componente**                 | **Estado Actual**                     | **Problema Identificado**                     |
| ------------------------------ | ------------------------------------- | --------------------------------------------- |
| **Servidores de Aplicaciones** | 5 servidores físicos en un datacenter | Alto costo de mantenimiento, riesgo de fallas |
| **Base de Datos**              | MySQL en un servidor físico           | Sin redundancia, posible pérdida de datos     |
| **Almacenamiento**             | 2 TB en discos NAS                    | No escalable, riesgo de pérdida de datos      |
| **Conectividad**               | VPN local                             | Latencia alta, sin redundancia                |
| **Seguridad**                  | Firewall perimetral tradicional       | No cumple con normativas como HIPAA y GDPR    |

📌 **¿Por qué necesita migrar Curare S.A.?**
🔹 **Reducción de costos:** Mantener servidores físicos es caro y poco escalable.
🔹 **Cumplimiento normativo:** Necesitan **cifrado de datos, auditoría y control de accesos**.
🔹 **Alta disponibilidad:** El sistema no tiene redundancia y podría fallar.
🔹 **Escalabilidad y rendimiento:** AWS permite crecimiento sin necesidad de hardware físico.

📌 **Estrategia de Migración:** **Replatforming** (Mejorar la infraestructura en la nube sin cambiar completamente las aplicaciones).

------

## **📌 2. Diseño de Arquitectura en AWS**

📊 **Diagrama de Arquitectura de Curare S.A. en AWS**

```css
Usuarios → ALB (Balanceador de Carga) → EC2 (Servidor de Aplicaciones) → RDS (Base de Datos) → S3 (Almacenamiento de Informes)  
                 |                    |                     |  
                 ↓                    ↓                     ↓  
            AWS WAF                 IAM                    AWS Backup  
            (Protección)       (Control de Acceso)    (Respaldo de datos)  
```

📌 **Servicios AWS Utilizados**
✅ **EC2** – Para hospedar los servidores de aplicaciones sanitarias.
✅ **RDS (MySQL)** – Base de datos para historias clínicas.
✅ **S3** – Para almacenar reportes médicos y archivos de pacientes.
✅ **AWS VPN + Direct Connect** – Conexión segura con hospitales.
✅ **IAM** – Para gestionar accesos y permisos.
✅ **AWS Backup** – Respaldo automático de bases de datos y archivos.
✅ **CloudTrail + GuardDuty** – Auditoría y detección de amenazas.
✅ **AWS WAF** – Protección contra ataques a la web.

------

## **📌 3. Instalación y Configuración de Servicios en AWS**

### **🔹 3.1 Creación de Infraestructura Base en AWS**

📌 **1️⃣ Creación de Red Segura en AWS (VPC, Subnets y Security Groups)**

🔹 **Acceder a AWS Console**
🔹 Ir a **VPC → Crear una nueva VPC**

- Nombre: `Curare-VPC`
- CIDR: `10.0.0.0/16`
  🔹 Crear **Subnets Privadas y Públicas**
  🔹 Configurar **Security Groups**
- **Permitido:** SSH (solo IP de administradores), HTTP/HTTPS, MySQL solo desde EC2

📌 **2️⃣ Creación del Servidor de Aplicaciones en EC2**

🔹 Ir a **EC2 → Lanzar Instancia**

- **AMI:** Amazon Linux 2 (gratis)
- **Tipo:** t3.micro (Free Tier)
- **Clave SSH:** `CurareKey.pem`
  🔹 **Configurar Reglas de Seguridad**
- Solo acceso SSH desde la IP del equipo administrador.
- Permitir tráfico HTTP/S solo desde el balanceador de carga.
  🔹 **Instalar dependencias en la instancia EC2**

```bash
sudo yum update -y
sudo yum install httpd mysql -y
sudo systemctl start httpd
```

📌 **3️⃣ Creación de la Base de Datos en Amazon RDS**

🔹 Ir a **Amazon RDS → Crear Base de Datos**

- **Motor:** MySQL
- **Instancia:** db.t3.micro (gratis)
- **Almacenamiento:** 20GB
- **Cifrado:** Activado con AWS KMS
- **Acceso:** Solo desde la VPC de la aplicación

📌 **4️⃣ Configuración de Almacenamiento Seguro con S3**

🔹 Ir a **S3 → Crear un Bucket**

- **Nombre:** `curare-reports`
- **Acceso Público:** Bloqueado
- **Cifrado:** Activado (AWS KMS)

🔹 Configurar políticas IAM para que solo los servidores puedan acceder.

------

## **📌 4. Seguridad y Cumplimiento en AWS**

📌 **5️⃣ Implementación de Control de Acceso con IAM**

🔹 Ir a **IAM → Crear Roles**

- **Rol para EC2:** Acceso a S3 y CloudWatch Logs
- **Rol para Administradores:** Solo acceso a RDS y EC2

📌 **6️⃣ Auditoría y Monitoreo con AWS CloudTrail y GuardDuty**

🔹 Ir a **CloudTrail → Habilitar auditoría**

- **Registro de eventos en RDS, EC2 y S3**
  🔹 Ir a **GuardDuty → Activar**
- **Detectar accesos sospechosos o cambios no autorizados**

📌 **7️⃣ Protección contra Ataques con AWS WAF**

🔹 Ir a **AWS WAF → Crear Reglas**

- **Bloquear ataques SQL Injection y XSS**
- **Activar limitación de tráfico sospechoso**

------

## **📌 5. Migración de Datos y Aplicaciones**

📌 **8️⃣ Transferencia de Datos desde el Servidor Físico a AWS**

🔹 **Conectar el servidor físico a AWS VPN**
🔹 **Usar AWS DataSync para mover los archivos a S3**
🔹 **Importar Base de Datos a RDS**

```bash
mysqldump -u root -p database_name > backup.sql
scp backup.sql ec2-user@<EC2-IP>:/home/ec2-user/
mysql -h <RDS-ENDPOINT> -u admin -p database_name < backup.sql
```

📌 **9️⃣ Validación y Pruebas**

🔹 **Pruebas de carga en EC2 con Apache Benchmark**

```bash
ab -n 1000 -c 10 http://<EC2-IP>/
```

🔹 **Pruebas de acceso y recuperación de datos desde S3**

------

## **📌 6. Conclusión y Beneficios de la Migración**

✅ **Reducción de costos en un 50%** eliminando servidores físicos.
✅ **Cumplimiento de normativas** con cifrado en RDS y S3.
✅ **Alta disponibilidad** con balanceo de carga y backups automatizados.
✅ **Seguridad mejorada** con IAM, WAF y CloudTrail.

![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)