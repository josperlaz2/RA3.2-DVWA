# Guía de Explotación - Nivel Medio de DVWA

Esta guía proporciona una introducción práctica a las vulnerabilidades web comunes explotadas en el nivel de seguridad "Medio" de la aplicación web vulnerable Damn Vulnerable Web Application (DVWA). El objetivo es comprender los principios básicos de cada vulnerabilidad antes de avanzar a niveles de dificultad más altos.

## Vulnerabilidades Cubiertas (Nivel Medio):

* **Brute Force:** Demuestra cómo realizar ataques de fuerza bruta para adivinar contraseñas cuando no existen mecanismos de protección adecuados.
* **Command Injection:** Ilustra cómo inyectar comandos del sistema operativo a través de una aplicación web vulnerable, permitiendo la ejecución de comandos no autorizados en el servidor.
* **CSRF (Cross-Site Request Forgery):** Muestra cómo un atacante puede forzar a un usuario autenticado a realizar acciones no deseadas en una aplicación web.
* **File Inclusion:** Explica cómo un atacante puede incluir archivos arbitrarios (locales o remotos) dentro de la aplicación web, lo que puede llevar a la divulgación de información sensible o la ejecución de código malicioso.
* **SQL Injection:** Introduce las vulnerabilidades de inyección SQL, donde un atacante puede interferir con las consultas a la base de datos para obtener acceso no autorizado, modificar datos o incluso ejecutar comandos en el servidor de la base de datos.
* **Upload:** Demuestra cómo las restricciones de carga de archivos insuficientes pueden permitir a un atacante cargar archivos maliciosos (por ejemplo, webshells) en el servidor.
* **XSS (Cross-Site Scripting) - DOM:** Explora las vulnerabilidades XSS basadas en el DOM, donde el código JavaScript malicioso se inyecta en la página web y se ejecuta en el navegador del usuario.
* **XSS (Cross-Site Scripting) - Reflected:** Muestra cómo los scripts maliciosos pueden ser inyectados a través de parámetros de URL o formularios y reflejados en la página web, afectando a los usuarios que hacen clic en enlaces maliciosos.
* **XSS (Cross-Site Scripting) - Stored:** Ilustra cómo los scripts maliciosos pueden almacenarse en la base de datos de la aplicación y ejecutarse para todos los usuarios que visitan la página donde se muestra el contenido afectado.

## Prerrequisitos:

* Tener DVWA instalado y configurado en un entorno de pruebas (por ejemplo, con XAMPP, WAMP o Docker).
* Tener un navegador web.
* Conocimientos básicos sobre cómo interactuar con aplicaciones web.

## Uso:

Cada sección de la guía detallará los pasos para explotar la vulnerabilidad específica en el nivel de seguridad "Medio" de DVWA. Se proporcionarán ejemplos y explicaciones para facilitar la comprensión.

## Advertencia:

Esta guía se proporciona únicamente con fines educativos y de pruebas de seguridad. No intentes explotar estas vulnerabilidades en sistemas que no te pertenezcan o para los que no tengas permiso. El uso indebido de esta información es ilegal y puede tener consecuencias graves.

## Contribuciones:

Las contribuciones a esta guía son bienvenidas. Si encuentras errores o tienes sugerencias para mejorarla, no dudes en crear un pull request.
