![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Ejercicio 2: Responder a un Ataque DDoS al Servidor DNS**

**TechSecure** detecta un aumento inusual en el tráfico hacia su servidor DNS autoritativo, lo que ralentiza sus servicios.

#### **Pasos para Resolver el Ataque:**

1. **Identificar el tráfico anómalo:**

   - Utilizar Wireshark:

     ```css
     sudo wireshark
     ```

   - Filtrar por tráfico UDP en el puerto 53: `udp.port == 53`.

2. **Bloquear direcciones IP sospechosas:**

   - Utilizar reglas de iptables:

     ```css
     sudo iptables -A INPUT -s <IP_maliciosa> -j DROP
     ```

3. **Configurar límites en consultas DNS para prevenir abuso:**

   - Modificar configuraciones de BIND:

     ```css
     rate-limit {
         responses-per-second 10;
     };
     ```

4. **Implementar soluciones de mitigación DDoS como Cloudflare.**

#### **Herramientas de Kali Linux utilizadas:**

- **Wireshark** para identificar tráfico malicioso.
- **iptables** para bloquear IPs maliciosas.

# **Instalación y Uso de iptables Combinado con Wireshark en Kali Linux**

### **1. Instalación de iptables en Kali Linux**

`iptables` es una herramienta de línea de comandos que permite gestionar reglas de firewall en Linux. Aunque generalmente ya está preinstalada en Kali Linux, te muestro cómo verificar su instalación y, si es necesario, instalarla.

#### **Pasos para Instalar iptables:**

1. **Verificar si iptables está instalado:** Ejecuta el siguiente comando para comprobar si iptables está disponible en tu sistema:

   ```css
   iptables --version
   ```

   Si ves una salida como `iptables v1.8.x`, entonces iptables ya está instalado.

2. **Actualizar los repositorios (si no está instalado):** Antes de instalar cualquier paquete, asegúrate de que los repositorios estén actualizados:

   ```css
   sudo apt update
   ```

3. **Instalar iptables:** Si no tienes iptables instalado, utiliza este comando para instalarlo:

   ```css
   sudo apt install iptables
   ```

4. **Verificar que iptables esté activo:** Una vez instalado, puedes verificar que el servicio está activo ejecutando:

   ```css
   sudo iptables -L
   ```

   Esto listará las reglas actuales del firewall (si las hay).

------

### **2. Tres Formas de Uso de iptables Combinado con Wireshark**

#### **Antes de Empezar:**

- **Wireshark** será utilizado para capturar y analizar el tráfico de red relacionado con las reglas configuradas en iptables.
- Estas técnicas te permiten combinar el análisis del tráfico con la implementación de reglas de firewall para bloquear, redirigir o analizar conexiones sospechosas.

------

#### **Caso 1: Bloquear el Tráfico desde una Dirección IP Específica**

**Escenario:** Detectaste que una dirección IP sospechosa está enviando tráfico malicioso hacia tu sistema. Quieres bloquear todo el tráfico proveniente de esa IP.

**Pasos:**

1. **Identificar la IP sospechosa con Wireshark:**

   - Abre Wireshark y captura el tráfico en tu interfaz de red (por ejemplo, `eth0` o `wlan0`).

   - Usa un filtro para observar solo las conexiones de la IP sospechosa:

     ```css
     ip.src == <IP_sospechosa>
     ```

   - Anota la IP que necesitas bloquear.

2. **Bloquear la IP con iptables:**

   - Usa iptables para bloquear todo el tráfico entrante desde esa IP:

     ```css
     sudo iptables -A INPUT -s <IP_sospechosa> -j DROP
     ```

3. **Confirmar que la IP está bloqueada:**

   - Captura tráfico nuevamente en Wireshark y verifica que no hay más paquetes provenientes de la IP bloqueada.

4. **Guardar las reglas de iptables:**

   - Para evitar perder las reglas después de un reinicio, guárdalas usando:

     ```css
     sudo iptables-save > /etc/iptables/rules.v4
     ```

------

#### **Caso 2: Limitar el Número de Conexiones Concurrentes desde una IP**

**Escenario:** Una IP parece estar realizando un ataque de tipo DoS enviando múltiples solicitudes en poco tiempo. Quieres limitar la cantidad de conexiones permitidas desde una sola IP.

**Pasos:**

1. **Identificar el tráfico de alta frecuencia con Wireshark:**

   - Abre Wireshark y captura el tráfico.

   - Filtra por paquetes con muchas conexiones desde una misma IP:

     ```css
     ip.src == <IP_sospechosa>
     ```

2. **Configurar una regla de iptables para limitar conexiones:**

   - Establece un límite de conexiones simultáneas permitidas:

     ```css
     sudo iptables -A INPUT -p tcp --syn -s <IP_sospechosa> -m connlimit --connlimit-above 10 -j REJECT
     ```

   - Esto bloquea cualquier intento de realizar más de 10 conexiones simultáneas desde la misma IP.

3. **Monitorear el tráfico con Wireshark:**

   - Captura tráfico nuevamente en Wireshark y verifica que la IP ya no puede superar las 10 conexiones activas.

------

#### **Caso 3: Registrar y Analizar Intentos de Conexión Bloqueados**

**Escenario:** Deseas registrar todos los intentos de conexión bloqueados para analizarlos posteriormente con Wireshark.

**Pasos:**

1. **Configurar iptables para registrar los intentos bloqueados:**

   - Añade una regla para registrar todo el tráfico bloqueado:

     ```css
     sudo iptables -A INPUT -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
     ```

2. **Verificar los registros en tiempo real:**

   - Usa el comando 

     ```
     dmesg
     ```

      para ver los registros generados por iptables:

     ```css
     dmesg | grep "IPTables-Dropped"
     ```

3. **Capturar el tráfico bloqueado con Wireshark:**

   - Filtra por paquetes que coincidan con la regla de iptables.

   - En Wireshark, utiliza un filtro para buscar paquetes específicos bloqueados:

     ```css
     ip.dst == <tu_dirección_IP> && tcp.flags == 0x0004
     ```

4. **Analizar los paquetes bloqueados:**

   - Examina los detalles de los paquetes bloqueados para identificar patrones, posibles vectores de ataque o direcciones IP sospechosas.

------

### **Comandos Adicionales y Notas**

#### **Listar las reglas actuales de iptables:**

```css
sudo iptables -L -v -n
```

#### **Eliminar una regla específica:**

Si necesitas eliminar una regla, utiliza:

```css
sudo iptables -D INPUT <número_de_regla>
```

Puedes encontrar el número de la regla listándolas primero.

#### **Restablecer iptables a su configuración predeterminada:**

```css
sudo iptables -F
```

#### **Guardar y restaurar reglas de iptables:**

- Guardar reglas:

  ```css
  sudo iptables-save > /etc/iptables/rules.v4
  ```

- Restaurar reglas:

  ```css
  sudo iptables-restore < /etc/iptables/rules.v4
  ```

------

### **Conclusión**

La combinación de **iptables** y **Wireshark** es extremadamente poderosa para proteger y analizar el tráfico de red. Mientras que iptables actúa como un firewall para bloquear, limitar o registrar conexiones, Wireshark te permite capturar y analizar los paquetes involucrados para tomar decisiones informadas. Con estos pasos, puedes configurar reglas de seguridad dinámicas y monitorear el impacto en tiempo real. ¡Manos a la obra! 🚀





