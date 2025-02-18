![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **Práctica de Web Scraping en Google Colab** 📊🌐

## **Objetivo**

Aprender a extraer información de páginas web de manera automática utilizando Python en Google Colab. Se utilizarán librerías como `requests`, `BeautifulSoup`, y `pandas`, y se guardarán los datos en un archivo CSV.

## **📌 Paso 1: Configurar Google Colab**

1. Abre Google Colab y crea un nuevo cuaderno.
2. Cambia el nombre del archivo a **"Web_Scraping_Practica"**.

------

## **📌 Paso 2: Instalar y Importar Librerías**

Ejecuta el siguiente código en una celda de Google Colab para instalar y cargar las librerías necesarias:

```python
# Instalación de librerías (si no están instaladas por defecto)
!pip install requests beautifulsoup4 pandas lxml

# Importar las librerías necesarias
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time  # Para evitar ser bloqueados por la web al hacer múltiples solicitudes
```

🔹 **Explicación de las librerías:**

- `requests`: Permite hacer solicitudes HTTP para obtener el contenido de una página web.
- `BeautifulSoup`: Analiza el HTML y extrae la información deseada.
- `pandas`: Organiza y guarda los datos en archivos.
- `lxml`: Motor de análisis de HTML para mejorar la compatibilidad con `BeautifulSoup`.

------

## **📌 Paso 3: Elegir una Página Web para Scraping**

Para esta práctica, utilizaremos **https://books.toscrape.com/**, una página diseñada para pruebas de scraping, donde extraeremos información sobre libros.

------

## **📌 Paso 4: Hacer una Solicitud HTTP a la Web**

Ejecuta el siguiente código para obtener el contenido de la página:

```python
# URL de la página a analizar
url = "https://books.toscrape.com/"

# Hacer la solicitud HTTP
response = requests.get(url)

# Verificar si la solicitud fue exitosa
if response.status_code == 200:
    print("Conexión exitosa")
else:
    print("Error en la conexión", response.status_code)

# Mostrar los primeros 500 caracteres del HTML obtenido
print(response.text[:500])
```

✅ **Verificación**: Si el código devuelve "Conexión exitosa", la página está lista para el scraping.

------

## **📌 Paso 5: Analizar el HTML con BeautifulSoup**

Ejecuta el siguiente código para analizar la estructura HTML de la página:

```python
# Analizar el contenido HTML con BeautifulSoup
soup = BeautifulSoup(response.text, "lxml")

# Ver la estructura del HTML
print(soup.prettify()[:1000])  # Imprime los primeros 1000 caracteres formateados
```

🔍 **Explicación**: `prettify()` formatea el HTML para que sea más legible.

------

## **📌 Paso 6: Extraer Información Específica**

Buscaremos los títulos de los libros, sus precios y su disponibilidad.

```python
# Encontrar todos los contenedores de libros
books = soup.find_all("article", class_="product_pod")

# Lista para almacenar los datos
book_data = []

# Extraer datos de cada libro
for book in books:
    title = book.h3.a["title"]  # Obtener el título del libro
    price = book.find("p", class_="price_color").text  # Obtener el precio
    availability = book.find("p", class_="instock availability").text.strip()  # Obtener disponibilidad
    
    # Guardar en la lista
    book_data.append([title, price, availability])

# Mostrar los primeros 5 resultados
for i in range(5):
    print(book_data[i])
```

✅ **Verificación**: Deberías ver una lista con los títulos de los libros, precios y disponibilidad.

------

## **📌 Paso 7: Guardar los Datos en un DataFrame**

```python
# Convertir los datos en un DataFrame de pandas
df = pd.DataFrame(book_data, columns=["Título", "Precio", "Disponibilidad"])

# Mostrar los primeros registros
df.head()
```

📊 **Explicación**: `pandas` organiza la información en formato tabular para facilitar su análisis.

------

## **📌 Paso 8: Almacenar los Datos en un Archivo CSV**

```python
# Guardar el DataFrame en un archivo CSV
df.to_csv("libros_scraping.csv", index=False, encoding="utf-8")

print("Archivo CSV guardado exitosamente.")
```

✅ **Resultado esperado**: Se genera un archivo `libros_scraping.csv` con los datos extraídos.

------

## **📌 Paso 9: Descargar el Archivo CSV en Google Colab**

Para descargar el archivo desde Google Colab a tu computadora:

```python
from google.colab import files
files.download("libros_scraping.csv")
```

📥 **Explicación**: Esta función permite descargar el archivo de Colab a tu equipo.

------

## **📌 Paso 10: Mejorar el Scraping con Navegación entre Páginas**

Para extraer datos de varias páginas, podemos recorrerlas automáticamente:

```python
# URL base de la web
base_url = "https://books.toscrape.com/catalogue/page-{}.html"

# Lista para almacenar los datos
all_books = []

# Extraer datos de las primeras 5 páginas
for page in range(1, 6):  # Modifica este número según las páginas a extraer
    print(f"Scraping página {page}...")
    
    # Hacer la solicitud HTTP
    response = requests.get(base_url.format(page))
    
    # Verificar que la página existe
    if response.status_code != 200:
        break

    # Analizar el contenido
    soup = BeautifulSoup(response.text, "lxml")
    
    # Encontrar los libros en la página
    books = soup.find_all("article", class_="product_pod")

    # Extraer los datos
    for book in books:
        title = book.h3.a["title"]
        price = book.find("p", class_="price_color").text
        availability = book.find("p", class_="instock availability").text.strip()
        
        all_books.append([title, price, availability])

    # Esperar 2 segundos entre páginas para no sobrecargar el servidor
    time.sleep(2)

# Crear DataFrame con todos los datos
df_all = pd.DataFrame(all_books, columns=["Título", "Precio", "Disponibilidad"])

# Guardar en CSV
df_all.to_csv("libros_multiples_paginas.csv", index=False, encoding="utf-8")

print("Scraping completado y datos guardados.")
```

------

## **📌 Conclusión**

Has aprendido a: 

✅ Instalar y usar `requests` y `BeautifulSoup`.

✅ Extraer información específica de una página web.

✅ Guardar los datos en un archivo CSV.

✅ Recorrer múltiples páginas automáticamente.



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)