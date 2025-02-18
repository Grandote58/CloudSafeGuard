![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# 📈 **Práctica Avanzada de Web Scraping: Monitoreo en Tiempo Real de Datos de la Bolsa de Valores**

## **📌 Introducción**

En esta práctica avanzada, simularemos un sistema de monitoreo de **precios de monedas representativas** en la bolsa de valores en **tiempo real**. Para esto, realizaremos **web scraping** a una página que provea información actualizada sobre el mercado financiero. Luego, representaremos los datos en **gráficos de barras dinámicos** que se actualicen **cada 2 minutos**.

## **📌 Objetivos**

🔹 Extraer los precios de las principales monedas en la bolsa de valores.
🔹 Identificar los selectores CSS o XPath adecuados.
🔹 Automatizar la captura de datos cada 2 minutos.
🔹 Visualizar la evolución de los precios en gráficos de barras.

------

## **📌 Paso 1: Configurar Google Colab**

1. Abre Google Colab y crea un nuevo cuaderno.
2. Cambia el nombre del archivo a **"Scraping_Bolsa_Valores"**.

------

## **📌 Paso 2: Instalar y Importar Librerías**

Ejecuta la siguiente celda para instalar y cargar las librerías necesarias:

```python
# Instalación de librerías necesarias
!pip install requests beautifulsoup4 pandas matplotlib

# Importar librerías
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time  # Para automatizar las solicitudes cada 2 minutos
import matplotlib.pyplot as plt
import datetime  # Para registrar el tiempo de cada medición
```

🔹 **Explicación de librerías:**

- `requests`: Para hacer solicitudes HTTP y obtener datos de la web.
- `BeautifulSoup`: Para analizar y extraer información del HTML.
- `pandas`: Para organizar los datos en una estructura tabular.
- `matplotlib`: Para generar gráficos dinámicos.
- `datetime`: Para registrar el tiempo en cada medición.

------

## **📌 Paso 3: Inspeccionar la Web y Obtener la URL**

Para obtener datos en tiempo real, utilizaremos la web **Investing.com** (o cualquier otra web financiera que permita el scraping).

**🔍 Exploración del sitio:**

1. Abre Investing Currencies en tu navegador.
2. Haz clic derecho sobre la tabla de monedas y selecciona **"Inspeccionar"**.
3. Observa la estructura HTML y busca los elementos clave:
   - **Nombre de la moneda**
   - **Precio actual**
   - **Cambio porcentual**

🔹 **Identificamos los siguientes selectores CSS/XPath:**

- **Nombre de la moneda**: `td.left.bold`
- **Precio**: `td.pid-XXX-last` (Donde `XXX` es un número único para cada moneda)
- **Cambio porcentual**: `td.pid-XXX-pcp`

💡 *Si los selectores cambian con el tiempo, inspecciona nuevamente la página para encontrar los correctos.*

------

## **📌 Paso 4: Extraer los Datos de la Bolsa de Valores**

Ejecuta el siguiente código para obtener información en tiempo real:

```python
# URL de la bolsa de valores (Investing Currencies)
url = "https://www.investing.com/currencies/"

# Configurar los headers para evitar bloqueos por parte de la web
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"
}

# Hacer la solicitud HTTP
response = requests.get(url, headers=headers)

# Verificar si la solicitud fue exitosa
if response.status_code == 200:
    print("Conexión exitosa")
else:
    print("Error en la conexión", response.status_code)

# Analizar el contenido HTML con BeautifulSoup
soup = BeautifulSoup(response.text, "lxml")

# Encontrar los nombres de las monedas
monedas = [moneda.text.strip() for moneda in soup.select("td.left.bold")[:10]]

# Encontrar los precios de las monedas
precios = [precio.text.strip() for precio in soup.select("td[class^='pid-'][class$='-last']")[:10]]

# Encontrar el cambio porcentual de cada moneda
cambios = [cambio.text.strip() for cambio in soup.select("td[class^='pid-'][class$='-pcp']")[:10]]

# Crear un DataFrame con los datos obtenidos
df = pd.DataFrame({"Moneda": monedas, "Precio": precios, "Cambio (%)": cambios})

# Mostrar los primeros registros
df.head()
```

✅ **Verificación**: Se imprimirá una tabla con el **nombre de la moneda, su precio y su cambio porcentual**.

------

## **📌 Paso 5: Monitorear los Cambios Cada 2 Minutos**

Ahora configuraremos el scraping para ejecutarse cada **2 minutos**, almacenando los datos en una lista.

```python
# Lista para almacenar registros en tiempo real
historial_precios = []

# Número de iteraciones (ejemplo: 5 mediciones)
n_iteraciones = 5

for i in range(n_iteraciones):
    print(f"📊 Monitoreo {i+1}/{n_iteraciones} - {datetime.datetime.now().strftime('%H:%M:%S')}")

    # Hacer la solicitud HTTP
    response = requests.get(url, headers=headers)
    
    if response.status_code != 200:
        print("Error en la conexión")
        break

    # Analizar el contenido
    soup = BeautifulSoup(response.text, "lxml")

    # Extraer nombres, precios y cambios
    monedas = [moneda.text.strip() for moneda in soup.select("td.left.bold")[:10]]
    precios = [float(precio.text.replace(',', '')) for precio in soup.select("td[class^='pid-'][class$='-last']")[:10]]
    cambios = [cambio.text.strip() for cambio in soup.select("td[class^='pid-'][class$='-pcp']")[:10]]

    # Guardar los datos con marca de tiempo
    for j in range(len(monedas)):
        historial_precios.append([datetime.datetime.now(), monedas[j], precios[j], cambios[j]])
    
    # Esperar 2 minutos antes de la siguiente solicitud
    time.sleep(120)

# Convertir la lista a DataFrame
df_historial = pd.DataFrame(historial_precios, columns=["Tiempo", "Moneda", "Precio", "Cambio (%)"])

# Guardar los datos en CSV
df_historial.to_csv("historial_precios.csv", index=False)

# Mostrar los primeros datos
df_historial.head()
```

✅ **Resultado esperado**: Un CSV con **datos en tiempo real** de las monedas seleccionadas.

------

## **📌 Paso 6: Crear un Gráfico de Barras Dinámico**

Ahora visualizaremos los cambios de precio de las monedas.

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Última medición de precios
ultima_medicion = df_historial[df_historial["Tiempo"] == df_historial["Tiempo"].max()]

# Graficar los precios actuales
plt.figure(figsize=(10, 5))
plt.barh(ultima_medicion["Moneda"], ultima_medicion["Precio"])
plt.xlabel("Precio en USD")
plt.ylabel("Moneda")
plt.title("Precios de Monedas - Última Actualización")
plt.grid(True)
plt.show()
```

✅ **Resultado esperado**: Un gráfico de barras con los precios de las monedas en la última medición.

------

## **📌 Conclusión**

Has aprendido a:

✅ Extraer precios de monedas representativas en la bolsa de valores.

✅ Identificar selectores CSS o XPath para scraping.

✅ Automatizar la extracción de datos cada 2 minutos.

✅ Guardar datos históricos en un CSV.

✅ Visualizar la evolución de los precios en gráficos de barras.



📢 **¡Felicidades! Ahora tienes un sistema funcional de monitoreo de datos de la bolsa de valores 🚀📊**



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)