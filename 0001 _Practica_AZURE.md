
![](../2x/Mesa%20de%20trabajo%201@2xParaMD.png)
# **Practica**

La Empresa TechNova, que está iniciando su migración a Azure usando recursos gratuitos. Esta empresa desea implementar una infraestructura de red básica con políticas de seguridad robustas, incluyendo un Firewall de Aplicaciones Web (WAF) y Azure Bastion para asegurar el acceso remoto.

# Objetivo
* Implementar una infraestructura de red para alojar aplicaciones web.
* Configurar un Firewall de Aplicaciones Web (WAF) para proteger aplicaciones frente a amenazas comunes.
* Mejorar la seguridad del acceso remoto mediante Azure Bastion.
* Aplicar políticas de seguridad en Azure para reforzar el cumplimiento.


# Diagrama de Arquitectura

**Infraestructura propuesta:**

```css
                   Internet
                      |
      ---------------------------------------
      |                                     |
Azure Firewall (WAF)                  Azure Bastion
      |                                     |
   Load Balancer                            |
      |                                     |
   Subred-Web (10.0.1.0/24)          Subred-Bastion (10.0.2.0/24)
      |                                     |
    Maquina Virtual (VM-App)         Maquina Virtual Administrativa
```
# Práctica Paso a Paso
### **Paso 1: Crear una red virtual (VNet)**

1. Inicia sesión en el portal de Azure: https://portal.azure.com.
2. En el buscador, escribe Redes virtuales y selecciona la opción.
3. Haz clic en Crear.
4. Configura los parámetros:

* Nombre: VNet-TechNova.
* Rango de direcciones IPv4: 10.0.0.0/16.
* Subred 1:

    * Nombre: Subred-Web.
    * Rango: 10.0.1.0/24.

* Subred 2:

    * Nombre: Subred-Bastion.
    * Rango: 10.0.2.0/24.
    
5. Haz clic en Revisar + Crear y luego en Crear.

### Paso 2: Crear una Máquina Virtual (VM-App)
1. Ve al buscador y escribe Máquinas virtuales.
2. Haz clic en Crear > Máquina virtual.
3. Configura los parámetros:

    * Nombre: VM-App.
    * Región: East US.
    * Imagen: Ubuntu Server 20.04 LTS.
    * Tamaño: B1s.
    * Red: Selecciona VNet-TechNova y la subred Subred-Web.
    * Grupo de seguridad de red: Selecciona la opción básica.

4. Haz clic en Revisar + Crear y luego en Crear.

### Paso 3: Configurar un Firewall de Aplicaciones Web (WAF)
1. En el buscador, escribe Firewall de aplicaciones web y selecciona la opción Front Door and CDN.
2. Haz clic en Crear.
3. Configura los parámetros:

    * Nombre: WAF-TechNova.
    * Grupo de recursos: Usa RG-TechNova.
    * Región: East US.

4. Configura las reglas:
    * Selecciona el modo de prevención para bloquear tráfico malicioso automáticamente.
    * Activa las siguientes reglas de OWASP:
        * Bloquear inyecciones SQL.
        * Bloquear inyecciones de script (XSS).

5. Haz clic en Revisar + Crear y luego en Crear.

### Paso 4: Configurar Azure Bastion
1. En el buscador, escribe Azure Bastion.
2. Haz clic en Crear.
3. Configura los parámetros:

    * Nombre: Bastion-TechNova.
    * Grupo de recursos: Usa RG-TechNova.
    * Red virtual: Selecciona VNet-TechNova.
    * Subred: Selecciona Subred-Bastion.

4. Haz clic en Revisar + Crear y luego en Crear.

### Paso 5: Configurar Políticas de Seguridad
1. Ve al buscador y escribe Azure Policy.
2. Haz clic en Asignaciones > Asignar política.
3. Configura los siguientes parámetros:

    * Definición de la política: Selecciona Permitir ubicaciones específicas.
    * Alcance: Aplica al grupo de recursos RG-TechNova.
    * Parámetros: Restringe las ubicaciones a East US.

4. Crea una nueva política:
    * Definición de la política: Selecciona Requerir que los recursos estén protegidos con un WAF.
    * Alcance: Selecciona RG-TechNova.

5. Haz clic en Revisar + Crear.

### Paso 6: Probar la Infraestructura
1. Conexión a la VM-App usando Bastion:
    * Ve a VM-App > Conexión > Bastion.
    * Introduce las credenciales para acceder.
2. Simular ataques con WAF:
    * Intenta realizar una inyección SQL en el puerto 80 usando una herramienta como OWASP ZAP o Burp Suite.
    * Verifica que el WAF bloquea estos intentos.
3. Verificar políticas:
    * Intenta crear un recurso fuera de East US y verifica que Azure muestra un error.

### Conclusión
Este ejercicio simuló el caso de TechNova, una empresa que implementó su primera infraestructura en Azure con seguridad avanzada. La infraestructura incluye:

* Un WAF para proteger aplicaciones frente a amenazas como inyección SQL y XSS.
* Azure Bastion para acceso remoto seguro a las máquinas virtuales.
* Políticas de seguridad para limitar la ubicación de los recursos y reforzar la protección.


## Cómo crear una cuenta gratuita en Microsoft Azure

Este video, publicado hace aproximadamente un mes, te muestra paso a paso cómo crear una cuenta gratuita en Azure, aprovechando los servicios gratuitos y el crédito de $200 que ofrece Microsoft. 

[![¿Cómo crear una cuenta gratuita en Microsoft Azure?](https://img.youtube.com/vi/tTGyZAAcjtY/maxresdefault.jpg)](https://www.youtube.com/watch?v=tTGyZAAcjtY)  
<small>*click image to [open video](https://www.youtube.com/watch?v=tTGyZAAcjtY)*</small>


## Bibliografia

Infraestructura de Seguridad en la Nube de Azure
Este documento académico detalla cómo mantener y securizar una infraestructura en la nube de Azure, analizando puntos clave para maximizar la seguridad y las responsabilidades compartidas entre el proveedor y el cliente. 

[Infraestructura de Seguridad en la Nube de Azure](https://openaccess.uoc.edu/bitstream/10609/147879/4/sergiolopezlopezTFG0123memoria.pdf?utm_source=chatgpt.com)








