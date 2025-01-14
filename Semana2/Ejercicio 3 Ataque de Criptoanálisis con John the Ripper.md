![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Ejercicio 3: Ataque de Criptoanálisis con John the Ripper**

En una red interna de la empresa "CryptoTech", se detectan intentos de romper claves simétricas utilizadas en sistemas de cifrado mediante ataques de criptoanálisis. Como analista, necesitas evaluar la fortaleza de las claves.

------

### **Objetivos del ejercicio**

1. Instalar y configurar John the Ripper.
2. Realizar un ataque para evaluar la seguridad de las claves.
3. Recomendar cambios en la política de claves.

------

### **Pasos Detallados**

#### **Paso 1: Instalar John the Ripper**

1. **Actualizar el sistema:**

   ```css
   sudo apt update && sudo apt upgrade -y
   ```

2. **Instalar John the Ripper:**

   ```css
   sudo apt install john
   ```

3. **Verificar la instalación:**

   ```css
   john --version
   ```

------

#### **Paso 2: Realizar el Ataque**

1. **Crea un archivo de prueba con claves cifradas:**

   - Simula un archivo con contraseñas cifradas:

     ```css
     echo "user:$6$sal$RReOAEeQ2C/:0:0:User:/root:/bin/bash" > passwords.txt
     ```

2. **Ejecuta John the Ripper para descifrar las contraseñas:**

   ```css
   john --wordlist=/usr/share/wordlists/rockyou.txt passwords.txt
   ```

3. **Resultados esperados:** John intentará descifrar las contraseñas utilizando el diccionario especificado.

------

#### **Paso 3: Recomendaciones**

1. Aumentar el tamaño de las claves simétricas (por ejemplo, usar AES-256 en lugar de AES-128).
2. Rotar las claves regularmente y limitar su tiempo de uso.
3. Introducir autenticación multifactor para proteger el acceso al sistema.



# **Ejemplo Adicional: Ataque de Criptoanálisis con John the Ripper**



La empresa ficticia "SecureBank" detecta un archivo de contraseñas cifradas en un servidor comprometido. El archivo contiene hashes de contraseñas de usuarios que administran sistemas críticos. Estas contraseñas están cifradas usando el algoritmo **SHA-512**, pero sin aplicar sal (salt), lo que las hace más vulnerables a ataques.

Como analista de seguridad, tu tarea es:

1. Auditar el archivo de hashes utilizando **John the Ripper** para detectar contraseñas débiles.
2. Proponer medidas para reforzar la seguridad de las contraseñas almacenadas.

------

### **Objetivo del Ataque**

Usar **John the Ripper** para realizar un ataque de criptoanálisis que permita identificar contraseñas débiles almacenadas como hashes SHA-512 sin sal.

------

### **Pasos Detallados del Ejemplo**

#### **Paso 1: Preparar el Entorno y el Archivo de Hashes**

1. **Crear el archivo de hashes de prueba:** Simula un archivo con contraseñas cifradas usando el algoritmo SHA-512. Puedes usar la utilidad `openssl` para generar los hashes.

   Ejecuta el siguiente comando para crear el archivo:

   ```css
   echo "admin:$6$NoSaltHash$RReOAEeQ2CvXlNTy.kXHjW9jl5sv3zOBrLPQ9DKRcDRSbgYUSszqZV.o1gOjgpBXth3x/JKbXvPQYYGZFEkIj0:0:0:Admin:/root:/bin/bash" > securebank_hashes.txt
   ```

   En este ejemplo:

   - `"admin"` es el nombre del usuario.
   - `"$6$NoSaltHash$..."` es un hash SHA-512 sin sal.
   - `securebank_hashes.txt` es el archivo que contiene los hashes.

2. **Contenido del archivo de hashes:** Para este ejemplo, el archivo `securebank_hashes.txt` debería contener algo similar a:

   ```css
   admin:$6$NoSaltHash$RReOAEeQ2CvXlNTy.kXHjW9jl5sv3zOBrLPQ9DKRcDRSbgYUSszqZV.o1gOjgpBXth3x/JKbXvPQYYGZFEkIj0:0:0:Admin:/root:/bin/bash
   ```

------

#### **Paso 2: Instalar John the Ripper**

Si aún no tienes **John the Ripper** instalado en tu sistema Kali Linux, realiza los siguientes pasos:

1. **Actualizar el sistema:**

   ```css
   sudo apt update && sudo apt upgrade -y
   ```

2. **Instalar John the Ripper:**

   ```css
   sudo apt install john
   ```

3. **Verificar la instalación:** Asegúrate de que John se instaló correctamente ejecutando:

   ```css
   john --version
   ```

------

#### **Paso 3: Configurar el Diccionario de Contraseñas**

1. Usa el diccionario `rockyou.txt`, que viene incluido en Kali Linux. Este archivo contiene millones de contraseñas comunes y se encuentra en la ruta:

   ```css
   /usr/share/wordlists/rockyou.txt
   ```

2. Si el archivo está comprimido (con extensión `.gz`), descomprímelo:

   ```css
   sudo gunzip /usr/share/wordlists/rockyou.txt.gz
   ```

------

#### **Paso 4: Ejecutar el Ataque con John the Ripper**

1. **Cargar el archivo de hashes en John:** Usa el siguiente comando para iniciar el ataque:

   ```css
   john --wordlist=/usr/share/wordlists/rockyou.txt securebank_hashes.txt
   ```

   - `--wordlist=/usr/share/wordlists/rockyou.txt`: Especifica el archivo de diccionario que será usado para probar contraseñas.
   - `securebank_hashes.txt`: Es el archivo que contiene los hashes de las contraseñas a descifrar.

2. **Resultados esperados:** John intentará descifrar las contraseñas utilizando las palabras del diccionario. Si la contraseña del hash está en el diccionario, obtendrás un resultado similar al siguiente:

   ```css
   admin:password123
   ```

   Esto indica que la contraseña de "admin" es `password123`, que es extremadamente débil.

------

#### **Paso 5: Evaluar el Resultado**

1. Si se descifraron contraseñas, significa que el sistema de almacenamiento de contraseñas es inseguro debido a:
   - Uso de contraseñas débiles (como `password123`).
   - Falta de sal (salt) al generar los hashes.
   - Uso de un algoritmo rápido como SHA-512, que, aunque robusto, puede ser vulnerable si no se complementa con medidas adicionales como salteado.
2. Si las contraseñas no fueron descifradas, felicidades, las contraseñas almacenadas parecen ser fuertes. Sin embargo, aún puedes reforzar la seguridad con medidas adicionales.

------

#### **Paso 6: Proponer Medidas de Mitigación**

1. **Uso de salteado (salt):**

   - Aplica un valor aleatorio único (sal) antes de generar el hash de cada contraseña. Esto previene ataques basados en diccionarios precomputados (rainbow tables).

   - Ejemplo en Python:

     ```python
     import hashlib
     import os
     
     password = "password123"
     salt = os.urandom(16)  # Generar una sal aleatoria
     hashed_password = hashlib.sha512(salt + password.encode()).hexdigest()
     print("Sal:", salt)
     print("Hash:", hashed_password)
     ```

2. **Migrar a algoritmos de hash más seguros:**

   - Sustituir SHA-512 por algoritmos diseñados específicamente para contraseñas, como:
     - **bcrypt**: Genera hashes lentos, lo que dificulta ataques de fuerza bruta.
     - **Argon2**: Algoritmo moderno diseñado para resistencia a ataques de hardware.

3. **Políticas de contraseñas fuertes:**

   - Implementar políticas que requieran contraseñas largas y complejas (mínimo 12 caracteres, con letras, números y símbolos).
   - Reforzar la autenticación con mecanismos adicionales, como autenticación multifactor (MFA).

4. **Pruebas regulares de seguridad:**

   - Realizar auditorías frecuentes de contraseñas utilizando herramientas como John the Ripper para identificar contraseñas débiles antes de que lo hagan los atacantes.





