![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Ejercicio 1: Fuerza Bruta contra Hashes**

### **Contexto**

La empresa "CyberSecure" ha detectado que una base de datos con hashes MD5 de contraseñas podría haber sido comprometida. Como analista de seguridad, necesitas auditar la base de datos para identificar contraseñas débiles y luego implementar soluciones para fortalecerlas.

------

### **Objetivos del ejercicio**

1. Instalar y configurar las herramientas necesarias (Hashcat).
2. Realizar un ataque de fuerza bruta para descifrar hashes MD5.
3. Proponer medidas de mitigación, como el uso de contraseñas salteadas y algoritmos más seguros.

------

### **Pasos Detallados**

#### **Paso 1: Instalar Hashcat**

1. **Actualizar el sistema:** Antes de instalar cualquier herramienta, asegúrate de que Kali Linux esté actualizado:

   ```css
   sudo apt update && sudo apt upgrade -y
   ```

2. **Instalar Hashcat:** En la mayoría de los casos, Hashcat viene preinstalado en Kali Linux. Si no es así, instala Hashcat usando:

   ```css
   sudo apt install hashcat
   ```

3. **Verificar la instalación:** Asegúrate de que Hashcat se instaló correctamente ejecutando:

   ```css
   hashcat --version
   ```

------

#### **Paso 2: Obtener los Hashes MD5**

1. Crea un archivo llamado 

   ```
   hashes.txt
   ```

    para simular la base de datos comprometida:

   ```
   nano hashes.txt
   ```

   Pega los siguientes hashes MD5 en el archivo (estas son contraseñas comunes encriptadas con MD5):

   ```tex
   5f4dcc3b5aa765d61d8327deb882cf99  # password
   098f6bcd4621d373cade4e832627b4f6  # test
   d41d8cd98f00b204e9800998ecf8427e  # empty
   ```

   Guarda el archivo presionando **`Ctrl + O`**, luego **`Enter`**, y sal con **`Ctrl + X`**.

------

#### **Paso 3: Realizar el Ataque**

1. Ejecuta Hashcat con el archivo de hashes y el diccionario de contraseñas `rockyou.txt` (viene incluido en Kali Linux):

   ```css
   hashcat -m 0 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt
   ```

   - `-m 0`: Indica que el tipo de hash es MD5.
   - `-a 0`: Especifica un ataque de diccionario.
   - `hashes.txt`: Archivo con los hashes comprometidos.
   - `/usr/share/wordlists/rockyou.txt`: Diccionario de contraseñas comunes.

2. **Resultados esperados:** Hashcat identificará las contraseñas correspondientes a los hashes:

   ```tex
   5f4dcc3b5aa765d61d8327deb882cf99:password
   098f6bcd4621d373cade4e832627b4f6:test
   d41d8cd98f00b204e9800998ecf8427e:
   ```

------

#### **Paso 4: Mitigación**

1. Implementa contraseñas salteadas:

   - Usa algoritmos modernos como bcrypt o Argon2 para añadir mas seguridad a las contraseñas.

   - Ejemplo de cómo saltear contraseñas en Python:

     ```python
     import bcrypt
     password = b"password"
     salt = bcrypt.gensalt()
     hashed = bcrypt.hashpw(password, salt)
     print(hashed)
     ```

2. Migra de MD5 a un algoritmo más seguro (SHA-256 o bcrypt).

