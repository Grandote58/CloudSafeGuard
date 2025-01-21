![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Implementación de la Capa de Datos en AWS**

**DataInsights** es una empresa ficticia que analiza grandes volúmenes de datos en tiempo real para ofrecer informes a sus clientes. Necesitan implementar una capa de datos eficiente y segura en AWS, que pueda manejar:

1. **Almacenamiento estructurado:** Para datos relacionales.
2. **Almacenamiento no estructurado:** Para análisis de logs y datos JSON.
3. **Procesamiento:** Para consultas rápidas sobre datos almacenados.
4. **Seguridad:** Para proteger los datos sensibles de accesos no autorizados.

------

### **Componentes y Servicios AWS**

#### 1. **Amazon RDS (Relational Database Service):**

- **Uso:** Almacenar datos relacionales (estructurados).
- **Razón:** Es un servicio gestionado que permite configurar bases de datos como MySQL, PostgreSQL o MariaDB fácilmente. Ofrece alta disponibilidad, recuperación ante fallos y cifrado de datos.

#### 2. **Amazon DynamoDB:**

- **Uso:** Almacenar datos no relacionales, como documentos JSON.
- **Razón:** Es un servicio NoSQL rápido y totalmente gestionado, ideal para manejar grandes cantidades de datos con acceso rápido.

#### 3. **Amazon S3 (Simple Storage Service):**

- **Uso:** Almacenar grandes volúmenes de datos no estructurados (logs, backups, datos de consulta).
- **Razón:** Ofrece almacenamiento escalable y seguro con opciones de versionado y cifrado.

#### 4. **Amazon Athena:**

- **Uso:** Realizar consultas directamente sobre los datos almacenados en S3 usando SQL.
- **Razón:** Proporciona análisis rápido y serverless, eliminando la necesidad de mover datos a otro sistema para procesarlos.

#### 5. **AWS Glue:**

- **Uso:** Catalogar, transformar y preparar los datos para análisis.
- **Razón:** Automatiza la extracción, transformación y carga (ETL) de datos entre servicios.

#### 6. **AWS KMS (Key Management Service):**

- **Uso:** Cifrar datos en reposo.
- **Razón:** Ofrece gestión centralizada de claves y cifrado robusto para proteger datos sensibles.

#### 7. **IAM Roles y Policies:**

- **Uso:** Controlar el acceso a los servicios y datos.

- **Razón:** Implementar el principio de privilegios mínimos para mejorar la seguridad.

  

------

### **Explicación de los Servicios y Componentes**

1. **IAM Roles & Policies**:
   - Controla el acceso a los servicios y datos, asegurando que solo las entidades autorizadas puedan interactuar con la infraestructura.
2. **Amazon RDS (Relational Data)**:
   - Maneja datos estructurados, como información transaccional o registros de usuarios, con cifrado habilitado mediante AWS KMS.
3. **Amazon DynamoDB (NoSQL Data)**:
   - Almacena datos no estructurados como documentos JSON para consultas rápidas.
4. **Amazon S3 (Unstructured Data)**:
   - Ofrece almacenamiento escalable para logs, backups y datos no estructurados que no necesitan procesamiento en tiempo real.
5. **AWS Glue (ETL)**:
   - Cataloga y transforma los datos provenientes de diferentes fuentes (RDS, DynamoDB, S3) para preparar su análisis.
6. **Amazon Athena (SQL Queries)**:
   - Permite consultas ad hoc sobre los datos almacenados en S3, eliminando la necesidad de mover los datos a un sistema de bases de datos separado.
7. **AWS KMS (Cryptography)**:
   - Cifra todos los datos en reposo y en tránsito, asegurando el cumplimiento de normativas de seguridad.

------

### **Por qué Usar esta Arquitectura**

- **Seguridad:** La integración de KMS y IAM asegura un control de acceso robusto y cifrado.
- **Escalabilidad:** Los servicios como DynamoDB y S3 manejan cargas dinámicas de datos sin problemas.
- **Flexibilidad:** Glue y Athena facilitan análisis rápidos y serverless sobre grandes volúmenes de datos.
- **Gestión Simplificada:** La capa de datos está diseñada para ser gestionada y mantenida automáticamente por AWS.