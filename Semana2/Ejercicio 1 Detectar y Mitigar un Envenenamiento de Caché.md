![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Ejercicio 1: Detectar y Mitigar un Envenenamiento de Caché**

La empresa ficticia "TechSecure" nota que los usuarios internos son redirigidos a sitios maliciosos al intentar acceder a sus servicios en línea. Sospechan de un envenenamiento de caché en su servidor DNS interno.

#### **Pasos para Resolver el Ataque:**

1. **Detectar la presencia de entradas maliciosas:**

   - Usar la herramienta `dig`  para consultar registros DNS:

   - 

     ```css
     dig @<IP_del_servidor_DNS> <dominio_a_consultar>
     ```

   - Verificar si las respuestas DNS coinciden con las direcciones IP esperadas.

2. **Capturar tráfico DNS para analizar posibles inyecciones maliciosas:**

   - Utilizar Wireshark :

     ```css
     sudo wireshark
     ```

   - Filtrar tráfico DNS con el filtro: `dns`.

3. **Limpiar la caché del servidor DNS comprometido:**

   - Para BIND:

     ```css
     sudo rndc flush
     ```

4. **Implementar DNSSEC para proteger las respuestas DNS.**

#### **Herramientas de Kali Linux utilizadas:**

- **`dig`** para pruebas de resolución.
- **`Wireshark`** para capturar y analizar tráfico DNS.



## **Instalación y Uso de `dig` y `Wireshark` en Kali Linux**

------

### **1. Instalación de `dig` (Domain Information Groper)**

`dig` es una herramienta de línea de comandos que se utiliza para realizar consultas DNS y depurar configuraciones de DNS.

#### **Pasos de instalación en Kali Linux:**

1. **Actualizar los repositorios:** Asegúrate de que tu sistema esté actualizado antes de instalar cualquier paquete:

   ```css
   sudo apt update
   ```

2. **Instalar el paquete `dnsutils`:** `dig` forma parte del paquete `dnsutils`, por lo que necesitas instalarlo:

   ```css
   sudo apt install dnsutils
   ```

3. **Verificar la instalación:** Una vez instalado, confirma que `dig` está disponible ejecutando:

   ```css
   dig -v
   ```

   Esto mostrará la versión instalada de `dig`.

------

### **2. Instalación de Wireshark**

Wireshark es una herramienta gráfica que permite capturar, analizar y depurar tráfico de red en tiempo real.

#### **Pasos de instalación en Kali Linux:**

1. **Actualizar los repositorios:** Al igual que con `dig`, comienza actualizando los repositorios:

   ```css
   sudo apt update
   ```

2. **Instalar Wireshark:** Usa el siguiente comando para instalar Wireshark:

   ```css
   sudo apt install wireshark
   ```

3. **Configurar permisos para capturar tráfico como usuario no root:** Durante la instalación, el sistema preguntará si deseas permitir que los usuarios no root capturen tráfico. Selecciona "Sí". Si no realizaste esta configuración durante la instalación, puedes configurarla manualmente:

   - Agrega tu usuario al grupo :

     ```css
     sudo usermod -aG wireshark $(whoami)
     ```

   - Actualiza los permisos:

     ```css
     sudo dpkg-reconfigure wireshark-common
     ```

4. **Verificar la instalación:** Confirma que Wireshark está instalado ejecutando:

   ```css
   wireshark --version
   ```

5. **Iniciar Wireshark:** Puedes iniciarlo desde el terminal escribiendo:

   ```css
   wireshark
   ```

------

### **3. Tres Formas de Uso de `dig`**

#### **1. Consultar un registro A (dirección IPv4):**

- Ejemplo:

  ```css
  dig google.com A
  ```

- Esto devolverá la dirección IPv4 asociada con `google.com`.

#### **2. Consultar registros específicos (MX, NS, TXT, etc.):**

- Para consultar registros MX (Mail Exchange) de un dominio:

  ```css
  dig ejemplo.com MX
  ```

- Esto mostrará los servidores de correo configurados para el dominio.

#### **3. Consultar un servidor DNS específico:**

- Si deseas consultar un dominio utilizando un servidor DNS específico:

  ```css
  dig @8.8.8.8 ejemplo.com
  ```

- En este caso, se utiliza el servidor DNS de Google (8.8.8.8) para realizar la consulta.

------

### **4. Tres Formas de Uso de Wireshark**

#### **1. Capturar tráfico en tiempo real:**

- Inicia Wireshark y selecciona la interfaz de red que deseas monitorear (por ejemplo, `eth0` o `wlan0`).
- Haz clic en el botón **Start** para comenzar a capturar paquetes.

#### **2. Filtrar tráfico DNS:**

- Una vez que estés capturando paquetes, puedes filtrar el tráfico DNS escribiendo en la barra de filtros:

  ```css
  dns
  ```

- Esto mostrará solo los paquetes relacionados con consultas y respuestas DNS.

#### **3. Analizar un paquete específico:**

- Haz clic en un paquete capturado para ver sus detalles.
- Expande las secciones para analizar el contenido, como la dirección IP de origen y destino, los números de puerto, y los detalles del protocolo DNS.

------

### **Ejemplo Práctico Combinado**

Supongamos que estás investigando un problema en el servidor DNS de tu red.

1. **Usa `dig` para verificar si el dominio se está resolviendo correctamente:**

   ```css
   dig @192.168.1.1 ejemplo.com
   ```

2. **Abre Wireshark para capturar el tráfico DNS:**

   - Filtra por el dominio específico:

     ```css
     dns.qry.name == "ejemplo.com"
     ```

3. **Analiza los paquetes en Wireshark para identificar anomalías, como respuestas no autorizadas o direcciones IP sospechosas.**

Con estas herramientas y casos de uso, puedes resolver y analizar problemas DNS de manera efectiva. ¡Listo para empezar! 🚀











