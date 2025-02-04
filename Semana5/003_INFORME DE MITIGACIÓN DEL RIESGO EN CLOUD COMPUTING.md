![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **📑 INFORME DE MITIGACIÓN DEL RIESGO EN CLOUD COMPUTING**

📌 *Objetivo:* Identificar y evaluar los principales riesgos en Cloud Computing y recomendar los controles más adecuados para **minimizar amenazas y garantizar la seguridad** en la infraestructura en la nube.

📌 *Alcance:* Evaluación de seguridad en **AWS, Azure y Google Cloud**, considerando aspectos clave como **identidad y acceso, cifrado, cumplimiento normativo y protección contra amenazas**.

📌 *Ejemplo Aplicado:* Evaluación de seguridad en **una empresa fintech que maneja datos financieros y debe cumplir con PCI DSS y GDPR**.

------

## **📌 1. INTRODUCCIÓN**

### **1.1 Contexto y Justificación**

La adopción de servicios en la nube ha aumentado significativamente debido a su **escalabilidad, flexibilidad y reducción de costos**. Sin embargo, esta transformación también introduce **nuevos riesgos de seguridad**, como **filtraciones de datos, accesos no autorizados, ataques DDoS y errores de configuración**.

Este informe presenta una **lista de verificación de seguridad en la nube** con controles recomendados para minimizar los riesgos más críticos en entornos como **AWS, Azure y Google Cloud**.

### **1.2 Objetivos del Informe**

✅ Identificar **principales amenazas en Cloud Computing**.
✅ Evaluar el nivel de riesgo en **infraestructura y servicios en la nube**.
✅ Proponer **controles de seguridad efectivos y optimizados en costos**.
✅ Asegurar el **cumplimiento normativo** con PCI DSS, ISO 27001 y GDPR.

------

## **📌 2. IDENTIFICACIÓN DE RIESGOS EN CLOUD COMPUTING**

📊 **Tabla 1: Principales amenazas y riesgos en Cloud Computing**

| **Categoría**                     | **Riesgo Identificado**                          | **Impacto** | **Ejemplo Real**                                             |
| --------------------------------- | ------------------------------------------------ | ----------- | ------------------------------------------------------------ |
| **Accesos No Autorizados**        | Robo de credenciales                             | Alto        | Un atacante obtiene acceso a la cuenta de AWS por una clave filtrada en GitHub. |
| **Filtración de Datos Sensibles** | Configuración incorrecta de S3 o Storage Buckets | Alto        | Datos financieros expuestos públicamente en un bucket S3 sin restricciones. |
| **Ataques DDoS**                  | Sobrecarga de tráfico en servidores cloud        | Medio-Alto  | Un sitio de pagos en línea cae durante un ataque masivo.     |
| **Errores de Configuración**      | Permisos IAM demasiado amplios                   | Alto        | Un usuario con acceso excesivo borra accidentalmente bases de datos críticas. |

------

## **📌 3. LISTA DE CHEQUEO Y CONTROLES DE MITIGACIÓN**

📊 **Tabla 2: Controles recomendados para mitigar amenazas en Cloud Computing**

| **Amenaza**                       | **Control Recomendado**                                      | **Herramienta en AWS / Azure / GCP**                         |
| --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Accesos No Autorizados**        | Implementación de MFA y control de acceso basado en roles (RBAC) | 🔹 AWS IAM + MFA 🔹 Azure AD Conditional Access 🔹 Google Cloud IAM |
| **Filtración de Datos Sensibles** | Configuración de cifrado en reposo y en tránsito             | 🔹 AWS KMS 🔹 Azure Key Vault 🔹 Google Cloud KMS               |
| **Ataques DDoS**                  | Protección con servicios específicos contra DDoS             | 🔹 AWS Shield 🔹 Azure DDoS Protection 🔹 Cloud Armor           |
| **Errores de Configuración**      | Auditoría automática de configuraciones inseguras            | 🔹 AWS Config 🔹 Azure Policy 🔹 Google Security Command Center |

📌 **Ejemplo Aplicado:**
Una empresa fintech con datos en **AWS** implementó **AWS IAM con MFA obligatorio** y restringió el acceso a buckets S3, evitando que datos financieros fueran expuestos públicamente.

------

## **📌 4. IMPLEMENTACIÓN DE CONTROLES Y RECOMENDACIONES**

### **4.1 Estrategia de Mitigación por Prioridad**

🔹 **Alto Riesgo:** Accesos no autorizados → Implementar IAM con MFA y permisos mínimos.
🔹 **Riesgo Medio:** Errores de configuración → Monitoreo automático con AWS Config o Azure Policy.
🔹 **Riesgo Bajo:** DDoS → Activar protección estándar en AWS Shield, Azure DDoS Protection o Cloud Armor.

### **4.2 Checklist de Seguridad en Cloud Computing**

✅ **Control de Identidad y Acceso**
🔲 Habilitar **MFA obligatorio** para usuarios administrativos.
🔲 Implementar **roles y permisos con privilegios mínimos (PoLP)**.
🔲 Usar **AWS IAM, Azure AD o Google Cloud IAM** para gestionar accesos.

✅ **Protección de Datos Sensibles**
🔲 Configurar **cifrado en reposo y en tránsito** en S3, RDS y bases de datos.
🔲 Aplicar **políticas de acceso restringido en almacenamiento cloud**.
🔲 Auditar accesos con **CloudTrail, Azure Monitor o Google Cloud Logging**.

✅ **Monitoreo y Auditoría**
🔲 Habilitar **AWS Security Hub, Azure Security Center o Google SCC**.
🔲 Configurar **alertas en tiempo real** para accesos sospechosos.
🔲 Revisar periódicamente **logs de actividad en la nube**.

------

## **📌 5. CONCLUSIONES Y RECOMENDACIONES**

📌 La mitigación de riesgos en Cloud Computing requiere un enfoque **proactivo** con herramientas nativas de seguridad en AWS, Azure y Google Cloud.
📌 Se recomienda implementar **autenticación MFA, cifrado obligatorio y auditorías de seguridad** como medidas mínimas de protección.
📌 Adoptar un modelo de **responsabilidad compartida**, donde el proveedor asegura la infraestructura y el cliente protege la configuración y accesos.

🔹 **Próximos pasos:**

✅ Realizar una **prueba de penetración** en la infraestructura en la nube.
✅ Automatizar **configuración segura con Terraform o AWS Config Rules**.
✅ Implementar **un plan de respuesta ante incidentes cloud**.

------

## **📚 BIBLIOGRAFÍA (2023 - 2024)**

📌 **1. Amazon Web Services. (2024). AWS Well-Architected Framework – Security Pillar.**
Disponible en: https://aws.amazon.com/architecture/well-architected/

📌 **2. Microsoft Azure. (2023). Azure Security Best Practices and Patterns.**
Disponible en: https://docs.microsoft.com/en-us/security/

📌 **3. Google Cloud. (2024). Security Foundations Blueprint for Cloud Computing.**
Disponible en: https://cloud.google.com/security



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)