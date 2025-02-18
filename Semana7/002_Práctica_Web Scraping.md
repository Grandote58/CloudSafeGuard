

![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **🍎📊 Práctica de Web Scraping para Análisis de Precios de Productos de la Competencia **

## **📌 Introducción**

Imagina que trabajas para una empresa de venta de productos comestibles y necesitas conocer los precios de un competidor en línea. En esta práctica, aprenderás a realizar **web scraping** en una tienda de alimentos en línea para extraer información sobre productos y precios.

Para este ejercicio, utilizaremos la web **https://scrapeme.live/shop/**, que está diseñada para la práctica de scraping y simula una tienda de comestibles.

------

## **📌 Objetivos**

🔹 Realizar web scraping sobre una tienda de alimentos en línea.
🔹 Extraer información relevante: **nombre del producto, precio e imagen**.
🔹 Identificar los selectores CSS o XPath para acceder a los datos.
🔹 Guardar los datos en un archivo CSV para su análisis.

------

## **📌 Paso 1: Configurar Google Colab**

1. Abre Google Colab y crea un nuevo cuaderno.
2. Cambia el nombre del archivo a **"Web_Scraping_Competencia"**.

------

## **📌 Paso 2: Instalar y Importar Librerías**

Ejecuta la siguiente celda para instalar y cargar las librerías necesarias:

```python
# Instalación de librerías necesarias
!pip install requests beautifulsoup4 pandas lxml

# Importar las librerías necesarias
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time  # Para evitar ser bloqueados por la web
```

🔹 **Explicación de librerías:**

- `requests`: Para realizar solicitudes HTTP y obtener el contenido de la página.
- `BeautifulSoup`: Para analizar y extraer datos del HTML.
- `pandas`: Para organizar y guardar los datos en un archivo CSV.
- `lxml`: Motor de análisis de HTML más eficiente.

------

## **📌 Paso 3: Inspeccionar la Web y Obtener la URL**

Utilizaremos la página **Scrapeme Shop**, una tienda ficticia de productos de supermercado.

**🔍 Exploración del sitio:**

1. Abre la web en tu navegador.
2. Haz clic derecho en cualquier producto y selecciona **"Inspeccionar"**.
3. Observa la estructura HTML y busca los elementos relevantes como:
   - Nombre del producto
   - Precio
   - Imagen

------

## **📌 Paso 4: Identificar los Selectores CSS o XPath**

Tras analizar el código HTML, identificamos los siguientes selectores CSS:

- **Nombre del producto**: `h2.woocommerce-loop-product__title`
- **Precio**: `span.woocommerce-Price-amount`
- **Imagen del producto**: `img.attachment-woocommerce_thumbnail`

También podemos usar XPath (alternativa a CSS Selectors):

- **Nombre del producto**: `//h2[@class="woocommerce-loop-product__title"]`
- **Precio**: `//span[@class="woocommerce-Price-amount amount"]`
- **Imagen**: `//img[@class="attachment-woocommerce_thumbnail size-woocommerce_thumbnail"]`

------

## **📌 Paso 5: Extraer los Datos de una Página**

Ahora haremos una solicitud HTTP para obtener la página y extraer los datos.

```python
# URL de la tienda
url = "https://scrapeme.live/shop/"

# Hacer la solicitud HTTP
response = requests.get(url)

# Verificar si la solicitud fue exitosa
if response.status_code == 200:
    print("Conexión exitosa")
else:
    print("Error en la conexión", response.status_code)

# Analizar el contenido HTML con BeautifulSoup
soup = BeautifulSoup(response.text, "lxml")

# Encontrar todos los productos en la página
productos = soup.find_all("li", class_="product")

# Lista para almacenar los datos
datos_productos = []

# Extraer datos de cada producto
for producto in productos:
    nombre = producto.find("h2", class_="woocommerce-loop-product__title").text
    precio = producto.find("span", class_="woocommerce-Price-amount").text
    imagen = producto.find("img", class_="attachment-woocommerce_thumbnail")["src"]

    # Guardar en la lista
    datos_productos.append([nombre, precio, imagen])

# Mostrar los primeros 5 resultados
for i in range(5):
    print(datos_productos[i])
```

✅ **Verificación**: Deberías ver una lista con el nombre, precio y enlace de la imagen del producto.

------

## **📌 Paso 6: Guardar los Datos en un DataFrame**

Ahora organizamos los datos en una tabla usando `pandas`.

```python
# Convertir los datos en un DataFrame de pandas
df = pd.DataFrame(datos_productos, columns=["Nombre", "Precio", "Imagen"])

# Mostrar los primeros registros
df.head()
```

📊 **Explicación**: `pandas` estructura los datos en un formato tabular para su análisis.

------

## **📌 Paso 7: Almacenar los Datos en un Archivo CSV**

```python
# Guardar el DataFrame en un archivo CSV
df.to_csv("precios_competencia.csv", index=False, encoding="utf-8")

print("Archivo CSV guardado exitosamente.")
```

✅ **Resultado esperado**: Se genera un archivo `precios_competencia.csv` con los datos extraídos.

------

## **📌 Paso 8: Descargar el Archivo CSV**

Para descargar el archivo desde Google Colab a tu computadora:

```python
from google.colab import files
files.download("precios_competencia.csv")
```

📥 **Explicación**: Permite descargar el archivo desde Google Colab a tu equipo.

------

## **📌 Paso 9: Scraping de Múltiples Páginas**

Si la web tiene paginación, podemos recorrer varias páginas automáticamente.

```python
# URL base con paginación
base_url = "https://scrapeme.live/shop/page/{}/"

# Lista para almacenar todos los productos
todos_los_productos = []

# Extraer datos de las primeras 3 páginas
for pagina in range(1, 4):
    print(f"Scraping página {pagina}...")

    # Hacer la solicitud HTTP
    response = requests.get(base_url.format(pagina))

    # Verificar que la página existe
    if response.status_code != 200:
        break

    # Analizar el contenido
    soup = BeautifulSoup(response.text, "lxml")

    # Encontrar los productos en la página
    productos = soup.find_all("li", class_="product")

    # Extraer los datos
    for producto in productos:
        nombre = producto.find("h2", class_="woocommerce-loop-product__title").text
        precio = producto.find("span", class_="woocommerce-Price-amount").text
        imagen = producto.find("img", class_="attachment-woocommerce_thumbnail")["src"]

        todos_los_productos.append([nombre, precio, imagen])

    # Pausa de 2 segundos para evitar sobrecarga del servidor
    time.sleep(2)

# Crear DataFrame con todos los productos
df_todos = pd.DataFrame(todos_los_productos, columns=["Nombre", "Precio", "Imagen"])

# Guardar en CSV
df_todos.to_csv("precios_competencia_multiples_paginas.csv", index=False, encoding="utf-8")

print("Scraping completado y datos guardados.")
```

✅ **Verificación**: Se genera un archivo con datos de múltiples páginas.

------

## **📌 Conclusión**

Has aprendido a: 

✅ Inspeccionar un sitio web y extraer información relevante.
✅ Identificar selectores CSS y XPath para scraping.
✅ Extraer y organizar datos de productos de un competidor.
✅ Guardar los datos en un archivo CSV.
✅ Automatizar la extracción de datos de múltiples páginas.

🎯 **Siguientes pasos**: Intenta aplicar estas técnicas a otras tiendas en línea de productos comestibles.

------

📢 **¡Felicidades! Ahora tienes una práctica completa para monitorear los precios de la competencia en línea 🚀**



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)