![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **Auditoría de Seguridad en la Nube con CloudSploit**

🔹 **Objetivo:** Aprender a utilizar **CloudSploit** para auditar configuraciones de seguridad en **AWS, Azure y Google Cloud**.

📌 **Lo que aprenderás:**

✅ Instalar y configurar **CloudSploit**.

✅ Identificar **vulnerabilidades y malas configuraciones** en la nube.

✅ Implementar **mejores prácticas de seguridad** en entornos cloud.

📌 **Herramientas necesarias:**

🔹 Google Colab o Kali Linux
🔹 Cuenta en AWS, Azure o Google Cloud
🔹 CloudSploit CLI



## **🔹 Módulo 1: Introducción a CloudSploit**

### **📌 1.1 ¿Qué es CloudSploit?**

CloudSploit es una **herramienta de seguridad en la nube** que permite auditar configuraciones en **AWS, Azure y Google Cloud** para detectar:

✅ Errores de configuración

✅ Permisos inseguros

✅ Riesgos de acceso público

✅ Fallos en cifrado y autenticación

**¿Por qué usar CloudSploit?**

🔹 Automatiza la auditoría de seguridad en entornos cloud.
🔹 Funciona en múltiples plataformas (AWS, Azure, GCP).
🔹 Genera reportes detallados con recomendaciones.

### **📌 1.2 Instalación y Configuración**

CloudSploit puede ejecutarse desde **Google Colab** o **Kali Linux**.

#### **💻 Opción 1: Instalación en Google Colab**

Ejecuta el siguiente código para instalar dependencias:

```python
!apt-get install -y git nodejs npm
!git clone https://github.com/aquasecurity/cloudsploit.git
!cd cloudsploit && npm install
```

📌 Esto instala **Node.js**, clona CloudSploit y descarga las dependencias.

#### **🐧 Opción 2: Instalación en Kali Linux**

Ejecuta los siguientes comandos en **Kali Linux**:

```python
sudo apt update && sudo apt install -y nodejs npm git
git clone https://github.com/aquasecurity/cloudsploit.git
cd cloudsploit && npm install
```

📌 Esto instala **CloudSploit CLI** en tu máquina.

## **🔹 Módulo 2: Configuración de Credenciales en CloudSploit**

### **📌 2.1 Creación de una Cuenta en AWS para Auditoría**

Para auditar AWS con CloudSploit, necesitamos una cuenta de AWS y un usuario con permisos de lectura.

🔹 **Pasos para crear un usuario en AWS IAM:**
1️⃣ **Accede a AWS IAM**: https://aws.amazon.com/iam/
2️⃣ **Crea un usuario** con permisos de **ReadOnlyAccess**.
3️⃣ **Obtén las credenciales de acceso (Access Key y Secret Key).**

### **📌 2.2 Configurar CloudSploit con AWS**

Una vez tengas las credenciales, configúralas en CloudSploit:

```less
export AWS_ACCESS_KEY_ID="TU_ACCESS_KEY"
export AWS_SECRET_ACCESS_KEY="TU_SECRET_KEY"
```

Verifica que CloudSploit tenga acceso con:

```less
aws sts get-caller-identity
```

✅ **Si devuelve un ARN, significa que la conexión es correcta.**

## **🔹 Módulo 3: Auditoría de Seguridad con CloudSploit**

### **📌 3.1 Ejecutar un escaneo en AWS**

Ejecuta el siguiente comando para auditar toda la cuenta de AWS:

```less
node cloudsploit scan --config aws
```

Ejemplo de salida:

```less
[✓] S3 Public Access Block Enabled ✔ PASSED
[✗] Security Groups Allowing Inbound 0.0.0.0/0 ❌ FAILED
[✗] IAM Users Without MFA ❌ FAILED
```

✅ **Explicación de los resultados:**

- ✔ **PASSED**: Configuración segura.
- ❌ **FAILED**: Riesgo detectado.

------

### **📌 3.2 Ejecutar un escaneo en Azure**

Si trabajas con **Azure**, sigue estos pasos:

1️⃣ **Instala el CLI de Azure**:

```less
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

2️⃣ **Inicia sesión en Azure**:

```less
az login
```

3️⃣ **Ejecuta el escaneo con CloudSploit**:

```less
node cloudsploit scan --config azure
```

### **📌 3.3 Ejecutar un escaneo en Google Cloud (GCP)**

1️⃣ **Instala el CLI de GCP**:

```less
curl https://sdk.cloud.google.com | bash
exec -l $SHELL
gcloud init
```

2️⃣ **Ejecuta el escaneo con CloudSploit**:

```less
node cloudsploit scan --config gcp
```

✅ **Esto auditará configuraciones en GCP y generará un reporte.**

## **🔹 Módulo 4: Interpretación de Resultados y Mitigación**

### **📌 4.1 Análisis de Resultados**

CloudSploit genera un informe detallado con hallazgos de seguridad.

Ejemplo de hallazgo crítico:

```less
[✗] Security Groups Allowing Inbound 0.0.0.0/0 ❌ FAILED
```

📌 **¿Qué significa?**

- Indica que hay **puertos abiertos a internet**, lo que permite ataques.

📌 **¿Cómo mitigarlo en AWS?**

- Accede a **EC2 > Security Groups**.
- Edita las reglas de tráfico y restringe el acceso.

### **📌 4.2 Recomendaciones de Seguridad**

🔹 **Habilitar MFA en usuarios de IAM.**
🔹 **Restringir el acceso público a S3.**
🔹 **Configurar registros de auditoría con AWS CloudTrail.**
🔹 **Usar redes privadas en Azure y GCP.**
🔹 **Configurar políticas de cifrado para bases de datos.**

✅ **Aplicar estas mejores prácticas mejora la seguridad en la nube.**



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)