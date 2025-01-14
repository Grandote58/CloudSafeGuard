![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Ejercicio 3: Configurar Medidas Preventivas para Proteger el DNS**

TechSecure desea prevenir futuros ataques al DNS implementando configuraciones seguras y herramientas de monitoreo.

#### **Pasos para Resolver el Problema:**

1. **Configurar DNSSEC:**

   - Generar claves DNSSEC para el servidor autoritativo:

     ```css
     dnssec-keygen -a RSASHA256 -b 2048 -n ZONE ejemplo.com
     ```

2. **Habilitar registros de auditoría:**

   - Configurar BIND para registrar consultas sospechosas:

     ```css
     logging {
         channel default_debug {
             file "data/named.run";
             severity dynamic;
         };
     };
     ```

3. **Monitorear el tráfico DNS:**

   - Usar **dnsenum** para mapear configuraciones DNS:

     ```css
     dnsenum ejemplo.com
     ```

#### **Herramientas de Kali Linux utilizadas:**

- **dnsenum** para auditoría.
- **dnssec-keygen** para habilitar DNSSEC.



# **Instalación y Uso de `dnsenum` y `dnssec-keygen` en Kali Linux**

------

## **1. Instalación de `dnsenum`**

`dnsenum` es una herramienta de enumeración DNS que permite realizar auditorías de seguridad al sistema de nombres de dominio. Es ideal para descubrir registros DNS, subdominios, servidores de correo, y configuraciones relacionadas.

### **Pasos para Instalar `dnsenum` en Kali Linux:**

1. **Actualizar los repositorios:** Antes de instalar cualquier herramienta, asegúrate de que los repositorios estén actualizados:

   ```css
   sudo apt update
   ```

2. **Instalar `dnsenum`:** En la mayoría de los casos, `dnsenum` ya está preinstalado en Kali Linux. Si no lo está, puedes instalarlo con el siguiente comando:

   ```css
   sudo apt install dnsenum
   ```

3. **Verificar la instalación:** Asegúrate de que `dnsenum` esté correctamente instalado ejecutando:

   ```css
   dnsenum --version
   ```

   Si el comando se ejecuta correctamente, significa que `dnsenum` está listo para usar.

------

## **2. Instalación de `dnssec-keygen`**

`dnssec-keygen` es una herramienta utilizada para generar claves criptográficas necesarias para implementar **DNSSEC (DNS Security Extensions)**. DNSSEC protege las consultas y respuestas DNS mediante firmas digitales.

### **Pasos para Instalar `dnssec-keygen` en Kali Linux:**

1. **Actualizar los repositorios:** Como siempre, asegúrate de que tu sistema esté actualizado:

   ```css
   sudo apt update
   ```

2. **Instalar BIND Utilities:** `dnssec-keygen` es parte del paquete **BIND9 Utilities**. Instálalo con este comando:

   ```css
   sudo apt install bind9-utils
   ```

3. **Verificar la instalación:** Confirma que `dnssec-keygen` está disponible ejecutando:

   ```css
   dnssec-keygen -h
   ```

   Esto mostrará la ayuda de la herramienta y confirmará su correcta instalación.

------

## **3. Tres Formas de Uso de `dnsenum`**

#### **Uso 1: Enumerar todos los registros DNS de un dominio**

Este comando realiza una consulta exhaustiva sobre un dominio y devuelve información como registros `A`, `MX`, `NS`, subdominios, etc.

**Comando:**

```css
dnsenum ejemplo.com
```

**Resultados esperados:**

- Muestra servidores DNS autoritativos.
- Identifica servidores de correo (MX).
- Detecta subdominios asociados al dominio.

------

#### **Uso 2: Enumerar subdominios utilizando diccionarios personalizados**

Puedes usar `dnsenum` para buscar subdominios específicos utilizando un archivo de diccionario.

**Comando:**

```css
dnsenum --dnsserver 8.8.8.8 --enum -f /usr/share/wordlists/dnsmap.txt ejemplo.com
```

**Explicación:**

- `--dnsserver 8.8.8.8`: Especifica el servidor DNS a consultar (en este caso, el de Google).
- `-f /usr/share/wordlists/dnsmap.txt`: Especifica el archivo de diccionario que contiene posibles nombres de subdominios.
- Esto es útil para identificar subdominios adicionales que podrían no estar publicados.

------

#### **Uso 3: Realizar transferencia de zona DNS (Zone Transfer)**

Un ataque de transferencia de zona intenta descargar todos los registros DNS desde un servidor autoritativo. Esto es posible si el servidor DNS está mal configurado.

**Comando:**

```css
dnsenum --enum --dnsserver <IP_del_servidor_DNS> ejemplo.com
```

**Resultados esperados:**

- Si el servidor permite la transferencia de zona, obtendrás una lista completa de los registros DNS almacenados.
- Esto incluye registros A, CNAME, MX, TXT y otros.

**Nota:** Si no tienes permiso para realizar pruebas en el dominio objetivo, realizar una transferencia de zona puede considerarse ilegal. Asegúrate de tener autorización para probar.

------

## **4. Tres Formas de Uso de `dnssec-keygen`**

#### **Uso 1: Generar una clave privada y pública para firmar zonas DNS**

Este comando genera una clave RSA con una longitud de 2048 bits para proteger una zona DNS.

**Comando:**

```css
dnssec-keygen -a RSASHA256 -b 2048 -n ZONE ejemplo.com
```

**Explicación:**

- `-a RSASHA256`: Especifica el algoritmo de cifrado.
- `-b 2048`: Indica el tamaño de la clave en bits.
- `-n ZONE`: Genera claves para firmar una zona completa.
- Este comando generará dos archivos:
  - Un archivo con la clave privada (`Kejemplo.com.+008+XXXXX.private`).
  - Un archivo con la clave pública (`Kejemplo.com.+008+XXXXX.key`).

------

#### **Uso 2: Generar una clave de firma para un registro DNS específico**

Puedes usar `dnssec-keygen` para proteger registros individuales en lugar de una zona completa.

**Comando:**

```css
dnssec-keygen -a RSASHA1 -b 1024 -n HOST subdominio.ejemplo.com
```

**Explicación:**

- `-a RSASHA1`: Utiliza el algoritmo RSASHA1.
- `-b 1024`: Genera una clave de 1024 bits (más rápida, pero menos segura que RSASHA256).
- `-n HOST`: Genera claves específicas para un subdominio.

**Resultados esperados:** Esto generará claves privadas y públicas para firmar digitalmente un subdominio.

------

#### **Uso 3: Verificar la clave generada para DNSSEC**

Una vez que hayas generado claves con `dnssec-keygen`, puedes usarlas para firmar zonas y verificar su autenticidad.

**Pasos:**

1. Genera una clave para tu zona:

   ```css
   dnssec-keygen -a RSASHA256 -b 2048 -n ZONE ejemplo.com
   ```

2. Verifica la clave generada:

   - Verifica el archivo **`.key`** generado:

     ```css
     cat Kejemplo.com.+008+XXXXX.key
     ```

   - Asegúrate de que contiene los datos esperados.

3. Firma la zona DNS: Utiliza la herramienta `dnssec-signzone` para firmar tu archivo de zona con la clave generada:

   ```css
   dnssec-signzone -o ejemplo.com -k Kejemplo.com.+008+XXXXX ejemplo.com.zone
   ```

   Esto crea un archivo firmado (`ejemplo.com.signed`) que puedes cargar en tu servidor DNS.

------

## **5. Conclusión**

- **`dnsenum`:** Es ideal para realizar enumeración DNS, descubrir registros, identificar vulnerabilidades (como transferencia de zona) y obtener un mapeo completo de dominios/subdominios.
- **`dnssec-keygen`:** Es esencial para proteger zonas DNS implementando firmas digitales con DNSSEC.

Ambas herramientas son potentes para pentesters y administradores de sistemas, y al combinarlas puedes evaluar la seguridad DNS de un sistema y protegerlo con estándares modernos como DNSSEC. ¡Ponte manos a la obra y experimenta con estas herramientas en un entorno controlado! 🚀

