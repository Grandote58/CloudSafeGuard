![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **Matriz Comparativa de Seguridad y Costos en AWS, Azure y Google Cloud**

📌 *Objetivo:* Evaluar **seguridad, cumplimiento y costos** en los principales proveedores de nube (AWS, Azure y Google Cloud) para seleccionar la mejor opción según las necesidades de la empresa.

📌 *Caso Aplicado:* **Fintech BankX** → Empresa financiera con operaciones en América Latina que maneja **datos de tarjetas de crédito y transacciones**. Necesita migrar su infraestructura a la nube asegurando **cumplimiento con PCI DSS, ISO 27001 y GDPR**.

------

## **🔐 Comparación de Seguridad en AWS, Azure y Google Cloud**

📊 **Tabla 1: Comparación de Seguridad y Cumplimiento**

| **Categoría**                        | **AWS**                           | **Azure**                         | **Google Cloud**                         |
| ------------------------------------ | --------------------------------- | --------------------------------- | ---------------------------------------- |
| **Gestión de Identidades y Accesos** | ✅ AWS IAM (Roles y MFA)           | ✅ Azure AD, RBAC                  | ✅ Google Cloud IAM                       |
| **Protección de Datos Sensibles**    | ✅ AWS KMS, S3 Encryption          | ✅ Azure Key Vault                 | ✅ Google Cloud KMS                       |
| **Protección contra DDoS**           | ✅ AWS Shield + AWS WAF            | ✅ Azure DDoS Protection           | ✅ Cloud Armor                            |
| **Detección de Amenazas**            | ✅ AWS GuardDuty, Security Hub     | ✅ Microsoft Defender for Cloud    | ✅ Security Command Center                |
| **Cumplimiento Normativo**           | ✅ ISO 27001, PCI DSS, GDPR, HIPAA | ✅ ISO 27001, PCI DSS, GDPR, HIPAA | ✅ ISO 27001, PCI DSS, GDPR, FedRAMP      |
| **Auditoría y Monitoreo**            | ✅ AWS CloudTrail, AWS Config      | ✅ Azure Monitor, Azure Policy     | ✅ Cloud Logging, Security Command Center |
| **Cifrado por defecto**              | ✅ Datos en reposo y en tránsito   | ✅ Datos en reposo y en tránsito   | ✅ Datos en reposo y en tránsito          |
| **Protección contra Malware**        | ✅ AWS Inspector                   | ✅ Microsoft Defender for Endpoint | ✅ VirusTotal (Google Cloud)              |

📌 *Conclusión de Seguridad:*
🔹 **AWS** tiene la solución más robusta en cumplimiento y cifrado para el sector financiero.
🔹 **Azure** es una mejor opción si ya se usan herramientas Microsoft (Ej: Office 365, Active Directory).
🔹 **Google Cloud** se enfoca en Zero Trust y es mejor en análisis de datos.

------

## **💰 Comparación de Costos en Seguridad Cloud**

📊 **Tabla 2: Costos Aproximados de Seguridad en AWS, Azure y Google Cloud (Mensual)**

| **Servicio de Seguridad**                                   | **AWS**                   | **Azure**                       | **Google Cloud**           |
| ----------------------------------------------------------- | ------------------------- | ------------------------------- | -------------------------- |
| **Gestión de Identidades (IAM, RBAC, MFA)**                 | Gratis                    | Gratis                          | Gratis                     |
| **Protección contra DDoS (Shield, Defender, Armor)**        | $3,000+ (Shield Advanced) | $2,944+ (Azure DDoS Protection) | $3,000+ (Cloud Armor)      |
| **Detección de amenazas (GuardDuty, Sentinel, SCC)**        | $1 por millón de eventos  | $2 por GB de datos analizados   | $0.50 por GB analizado     |
| **Cifrado y gestión de claves (KMS, Key Vault, Cloud KMS)** | $1 por clave activa/mes   | $1 por clave activa/mes         | $0.06 por clave activa/mes |
| **Auditoría y cumplimiento (Security Hub, Defender, SCC)**  | $100 - $500               | $120 - $700                     | $90 - $500                 |
| **Costos totales estimados (Mensual)**                      | **$100 - $5,000**         | **$120 - $4,500**               | **$90 - $4,000**           |

📌 *Conclusión de Costos:*
🔹 **Google Cloud es la opción más económica** en seguridad.
🔹 **AWS y Azure tienen costos similares**, pero AWS permite optimización con Savings Plans.
🔹 **DDoS Protection es el mayor gasto**, pero puede reducirse usando reglas personalizadas en WAF.

------

## **📌 Análisis Final: ¿Cuál es la Mejor Opción?**

📊 **Tabla 3: Evaluación de la Mejor Nube para Seguridad y Costos (Puntuación 1-10)**

| **Criterio**                    | **AWS** | **Azure** | **Google Cloud** |
| ------------------------------- | ------- | --------- | ---------------- |
| **Seguridad Total**             | ✅ 10/10 | ✅ 9/10    | ✅ 8/10           |
| **Protección de Datos**         | ✅ 10/10 | ✅ 9/10    | ✅ 8/10           |
| **Cumplimiento Normativo**      | ✅ 10/10 | ✅ 9/10    | ✅ 8/10           |
| **Costo de Seguridad**          | ⚠️ 7/10  | ⚠️ 7/10    | ✅ 8/10           |
| **Facilidad de Implementación** | ✅ 8/10  | ✅ 9/10    | ✅ 8/10           |

📌 **Resultado:** **AWS es la mejor opción** para una empresa financiera, aunque con costos más altos. **Google Cloud es más barato, pero menos especializado en cumplimiento financiero**.

------

## **✅ Recomendación: Estrategia Óptima para BankX**

**🏆 Opción Recomendada: AWS** (Por cumplimiento y seguridad avanzada)
🔹 Implementar **AWS IAM + MFA** para accesos seguros.
🔹 Usar **AWS Shield Standard + WAF** en lugar de Shield Advanced para reducir costos.
🔹 Configurar **AWS Config + GuardDuty** para monitoreo en tiempo real.
🔹 Activar **AWS KMS** solo en datos críticos para evitar sobrecostos.

**💰 Estrategia de Reducción de Costos:**
✅ Usar **instancias spot para monitoreo de seguridad** y reducir costos de computación.
✅ Aplicar **niveles gratuitos de AWS Security Hub y GuardDuty** antes de pagar licencias avanzadas.
✅ Usar **S3 con cifrado nativo (SSE-S3)** en lugar de **AWS KMS** para archivos no críticos.

📌 **Resultado esperado:** **Seguridad óptima con ahorro de hasta 40% en costos**.

![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)