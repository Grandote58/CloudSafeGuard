![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Ejercicio 2: Análisis Diferencial de Tráfico Cifrado**

### **Contexto**

Un desarrollador de "NetSecure Corp." sospecha que un atacante intenta descifrar las comunicaciones cifradas de la empresa mediante análisis diferencial. El tráfico sospechoso está siendo interceptado en un servidor con SSL/TLS.

------

### **Objetivos del ejercicio**

1. Instalar y configurar Wireshark para capturar tráfico.
2. Analizar patrones sospechosos en las comunicaciones cifradas.
3. Implementar cifrados robustos y medidas adicionales de protección.

------

### **Pasos Detallados**

#### **Paso 1: Instalar Wireshark**

1. **Actualizar el sistema:**

   ```css
   sudo apt update && sudo apt upgrade -y
   ```

2. **Instalar Wireshark:**

   ```css
   sudo apt install wireshark
   ```

3. **Permitir a usuarios no root capturar tráfico:** Durante la instalación, selecciona "Yes" cuando se te pregunte si los usuarios no root pueden capturar tráfico.

4. **Verificar la instalación:**

   ```css
   wireshark --version
   ```

------

#### **Paso 2: Capturar Tráfico Sospechoso**

1. Inicia Wireshark:

   ```css
   sudo wireshark
   ```

2. Selecciona la interfaz de red activa (por ejemplo, `eth0` o `wlan0`).

3. Usa un filtro para capturar solo el tráfico HTTPS cifrado (puerto 443):

   ```css
   tcp.port == 443
   ```

4. Captura tráfico por unos minutos y analiza patrones sospechosos.

------

#### **Paso 3: Identificar Patrones y Mitigar**

1. Busca patrones como paquetes repetitivos o diferencias mínimas en tamaños de paquetes (indicadores de análisis diferencial).
2. Implementa las siguientes soluciones:
   - Cambia a **TLS 1.3**, que incluye cifrados robustos y elimina vulnerabilidades en versiones anteriores.
   - Introduce **padding aleatorio** para dificultar el análisis diferencial.



