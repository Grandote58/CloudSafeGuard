![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# 🚀 **Práctica Avanzada de Web Scraping: Dashboard en Tiempo Real con Plotly para Monitoreo de Activos Financieros**

## **📌 Introducción**

En esta práctica, vamos a construir un **dashboard en tiempo real** para monitorear **acciones y criptomonedas** utilizando datos obtenidos mediante **web scraping**. Además, configuraremos **alertas** cuando los precios superen ciertos valores.

------

## **📌 Objetivos**

✔️ Extraer datos en tiempo real de **acciones y criptomonedas**.
✔️ Construir un **dashboard interactivo** con **Plotly Dash**.
✔️ Programar **actualizaciones automáticas** de los precios cada **2 minutos**.
✔️ **Enviar alertas** cuando un activo supere cierto umbral de precio.

------

## **📌 Paso 1: Configurar Google Colab**

1. Abre Google Colab y crea un nuevo cuaderno.
2. Cambia el nombre del archivo a **"Dashboard_Activos_Financieros"**.

------

## **📌 Paso 2: Instalar y Importar Librerías**

Ejecuta el siguiente código para instalar las librerías necesarias:

```python
# Instalación de librerías necesarias
!pip install requests beautifulsoup4 pandas plotly dash dash-bootstrap-components flask

# Importar librerías
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time
import plotly.graph_objects as go
from dash import Dash, dcc, html
import dash_bootstrap_components as dbc
from flask import Flask
import threading
```

🔹 **Explicación de librerías:**

- `requests`: Para hacer solicitudes HTTP y obtener datos de la web.
- `BeautifulSoup`: Para analizar el HTML y extraer datos.
- `pandas`: Para organizar los datos en tablas.
- `plotly`: Para generar gráficos interactivos.
- `dash`: Para construir el dashboard en tiempo real.
- `dash-bootstrap-components`: Para mejorar el diseño del dashboard.
- `flask`: Para manejar la ejecución del servidor en Google Colab.
- `threading`: Para manejar tareas en paralelo.

------

## **📌 Paso 3: Seleccionar una Fuente de Datos**

Usaremos **Investing.com** para obtener datos en tiempo real de **acciones y criptomonedas**.

### **🔍 Inspección de la Web**

En Investing Currencies y Investing Stocks, encontramos los siguientes selectores:

- **Nombre del activo:** `td.left.bold`
- **Precio actual:** `td[class^='pid-'][class$='-last']`
- **Cambio porcentual:** `td[class^='pid-'][class$='-pcp']`

------

## **📌 Paso 4: Extraer Datos en Tiempo Real**

Ejecutemos el código para obtener **precios de criptomonedas y acciones**.

```python
# URL de criptomonedas y acciones
url_cripto = "https://www.investing.com/currencies/"
url_acciones = "https://www.investing.com/equities/"

# Headers para evitar bloqueos
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"
}

# Función para extraer datos de la web
def obtener_datos(url):
    response = requests.get(url, headers=headers)
    if response.status_code != 200:
        print(f"Error en la conexión: {response.status_code}")
        return []

    soup = BeautifulSoup(response.text, "lxml")

    # Extraer nombres, precios y cambios porcentuales
    nombres = [n.text.strip() for n in soup.select("td.left.bold")[:5]]
    precios = [float(p.text.replace(',', '')) for p in soup.select("td[class^='pid-'][class$='-last']")[:5]]
    cambios = [p.text.strip() for p in soup.select("td[class^='pid-'][class$='-pcp']")[:5]]

    # Formatear los datos
    datos = [{"Nombre": nombres[i], "Precio": precios[i], "Cambio (%)": cambios[i]} for i in range(len(nombres))]
    return datos

# Obtener datos de criptomonedas y acciones
datos_cripto = obtener_datos(url_cripto)
datos_acciones = obtener_datos(url_acciones)

# Mostrar datos
print(pd.DataFrame(datos_cripto))
print(pd.DataFrame(datos_acciones))
```

✅ **Verificación**: Se imprimirá una tabla con los activos financieros y sus precios.

------

## **📌 Paso 5: Crear un Dashboard en Tiempo Real**

Ahora construiremos un **dashboard interactivo** con **Plotly Dash**.

```python
# Crear servidor Flask para Dash
server = Flask(__name__)

# Crear la aplicación Dash
app = Dash(__name__, server=server, external_stylesheets=[dbc.themes.BOOTSTRAP])

# Diseño del Dashboard
app.layout = html.Div([
    html.H1("📈 Dashboard de Activos Financieros", style={"textAlign": "center"}),

    dcc.Interval(id="intervalo", interval=120000, n_intervals=0),  # Actualiza cada 2 min

    html.Div([
        html.H3("Criptomonedas", style={"textAlign": "center"}),
        dcc.Graph(id="grafico_cripto")
    ]),

    html.Div([
        html.H3("Acciones", style={"textAlign": "center"}),
        dcc.Graph(id="grafico_acciones")
    ])
])

# Función para actualizar gráficos en tiempo real
@app.callback(
    [dcc.Output("grafico_cripto", "figure"), dcc.Output("grafico_acciones", "figure")],
    [dcc.Input("intervalo", "n_intervals")]
)
def actualizar_graficos(n):
    datos_cripto = obtener_datos(url_cripto)
    datos_acciones = obtener_datos(url_acciones)

    df_cripto = pd.DataFrame(datos_cripto)
    df_acciones = pd.DataFrame(datos_acciones)

    fig_cripto = go.Figure([go.Bar(x=df_cripto["Nombre"], y=df_cripto["Precio"], text=df_cripto["Cambio (%)"])])
    fig_cripto.update_layout(title="Precios de Criptomonedas", yaxis_title="Precio (USD)")

    fig_acciones = go.Figure([go.Bar(x=df_acciones["Nombre"], y=df_acciones["Precio"], text=df_acciones["Cambio (%)"])])
    fig_acciones.update_layout(title="Precios de Acciones", yaxis_title="Precio (USD)")

    return fig_cripto, fig_acciones
```

✅ **Explicación:**

- Se crea un dashboard con **dos gráficos de barras**.
- Se actualiza **cada 2 minutos** con `dcc.Interval`.
- Se extraen datos en tiempo real mediante `@app.callback`.

------

## **📌 Paso 6: Integrar Alertas de Precio**

Ahora configuraremos alertas cuando un **activo supere cierto umbral**.

```python
# Umbrales de alerta
UMBRAL_CRIPTO = 50000  # Ejemplo: Bitcoin supera 50,000 USD
UMBRAL_ACCION = 3000  # Ejemplo: Tesla supera 3000 USD

# Función para verificar alertas
def verificar_alertas():
    while True:
        datos_cripto = obtener_datos(url_cripto)
        datos_acciones = obtener_datos(url_acciones)

        for cripto in datos_cripto:
            if float(cripto["Precio"]) > UMBRAL_CRIPTO:
                print(f"🚨 ALERTA: {cripto['Nombre']} supera {UMBRAL_CRIPTO} USD!")

        for accion in datos_acciones:
            if float(accion["Precio"]) > UMBRAL_ACCION:
                print(f"🚨 ALERTA: {accion['Nombre']} supera {UMBRAL_ACCION} USD!")

        time.sleep(120)  # Revisar cada 2 minutos

# Ejecutar alertas en un hilo paralelo
hilo_alertas = threading.Thread(target=verificar_alertas)
hilo_alertas.start()
```

✅ **Resultado esperado**: Se imprimirá una alerta si un activo supera el umbral.

------

## **📌 Conclusión**

✔️ Creaste un **dashboard en tiempo real** con **Plotly Dash**.
✔️ Implementaste **scraping** de acciones y criptomonedas.
✔️ Configuraste **alertas automáticas** cuando los precios superan umbrales.

📢 **¡Felicidades! Ahora tienes un sistema de monitoreo financiero 🚀📊**.

![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)