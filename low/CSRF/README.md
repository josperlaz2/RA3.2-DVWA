# Explotación de la Vulnerabilidad CSRF en DVWA - Nivel Bajo

Este documento describe los pasos para explotar la vulnerabilidad Cross-Site Request Forgery (CSRF) en el nivel de seguridad "Bajo" de la aplicación web vulnerable Damn Vulnerable Web Application (DVWA).

## Resumen de la Vulnerabilidad CSRF

CSRF ocurre cuando un atacante engaña a un usuario autenticado para que realice acciones no deseadas en un sitio web en el que ya ha iniciado sesión. Esto se logra enviando una petición maliciosa al servidor en nombre del usuario sin su conocimiento o consentimiento. En el contexto de DVWA (Nivel Bajo), la función vulnerable es el cambio de contraseña, donde los parámetros se envían a través del método GET en la URL.

## Pasos para la Explotación

1.  **Identificar la acción vulnerable:** La función vulnerable en este nivel es el cambio de contraseña. El código fuente revela que la nueva contraseña y su confirmación se envían a través del método GET en la URL.

2.  **Realizar la acción legítimamente:**
    * Inicia sesión en DVWA como un usuario (por ejemplo, `admin` con la contraseña predeterminada `password`).
    * Navega a la página de cambio de contraseña.
    * Introduce una nueva contraseña (por ejemplo, `admin`) en los campos "New Password" y "Confirm New Password".
    * Haz clic en el botón "Change".

3.  **Analizar la petición:**
    * Observa la URL resultante en la barra de direcciones de tu navegador. Debería ser similar a la siguiente:

        ```
        http://<IP_DVWA>/vulnerabilities/csrf/?password_new=admin&password_conf=admin&Change=Change#
        ```

        Reemplaza `<IP_DVWA>` con la dirección IP o el nombre de dominio donde está alojado DVWA.

4.  **Construir la URL maliciosa:**
    * La URL observada en el paso anterior es la base de nuestro ataque CSRF. Si una víctima autenticada hace clic en esta URL mientras está logueada en DVWA, su contraseña se cambiará automáticamente a la contraseña especificada en la URL (`admin` en este ejemplo) sin su conocimiento o consentimiento.

    ```
    http://<IP_DVWA>/vulnerabilities/csrf/?password_new=<nueva_contraseña>&password_conf=<nueva_contraseña>&Change=Change#
    ```

    Reemplaza `<nueva_contraseña>` con la contraseña que deseas establecer para la víctima.
    
    ![imagen vulnerabilidad CSRF facil](../../assests/CSRFLow01.png)

5.  **Opcional: Codificar la URL (Ofuscación):**
    * Para hacer la URL menos sospechosa, puedes codificarla utilizando un codificador URL. Esto reemplaza los caracteres especiales con su representación porcentual. Por ejemplo, utilizando una herramienta como la función "Decoder" en Burp Suite, la URL del paso 3 podría codificarse como:

        ```
        %68%74%74%70%3a%2f%2f%3cIP_DVWA%3e%2f%76%75%6c%6e%65%72%61%62%69%6c%69%74%69%65%73%2f%63%73%72%66%2f%3f%70%61%73%73%77%6f%72%64%5f%6e%65%77%3d%61%64%6d%69%6e%26%70%61%73%73%77%6f%72%64%5f%63%6f%6e%66%3d%61%64%6d%69%6e%26%43%68%61%6e%67%65%3d%43%68%61%6e%67%65%23
        ```

        Reemplaza `%3cIP_DVWA%3e` con la representación codificada de tu dirección IP de DVWA.

	![imagen vulnerabilidad CSRF facil](../../assests/CSRFLow02.png)

6.  **Enviar la URL a la víctima:**
    * El atacante puede enviar esta URL maliciosa a la víctima a través de diversos métodos:
        * **Correo electrónico:** Insertando la URL en un correo electrónico engañoso.
        * **Sitio web malicioso:** Incluyendo la URL como un enlace en un sitio web controlado por el atacante.
        * **Mensajería instantánea:** Enviando la URL a través de plataformas de mensajería.

    Cuando la víctima (que ya está autenticada en DVWA) haga clic en este enlace, el navegador enviará una petición GET al servidor de DVWA con los parámetros de cambio de contraseña, ejecutando la acción no deseada sin su consentimiento.

**Precaución:** Esta demostración se realiza en un entorno controlado para fines educativos. La explotación de vulnerabilidades en sistemas reales sin permiso es ilegal y puede tener graves consecuencias.
