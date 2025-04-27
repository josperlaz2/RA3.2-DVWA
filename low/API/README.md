# Explotación de Vulnerabilidad de Seguridad en API (Nivel Bajo) - DVWA

Este README describe brevemente la explotación de una vulnerabilidad de seguridad en la API del nivel de seguridad Bajo de DVWA (Damn Vulnerable Web Application). La vulnerabilidad reside en la exposición de una versión antigua y menos segura de la API (v1), que revela información sensible.

## Pasos para la Explotación:

1.  **Iniciar sesión:** Accede a DVWA con cualquier cuenta de usuario válida.
2.  **Acceder al módulo API:** Navega al módulo "API" dentro de la interfaz de DVWA.
3.  **Inspeccionar el tráfico:** Abre las herramientas de desarrollo de tu navegador (generalmente presionando la tecla F12) y dirígete a la pestaña "Red" (o "Network").
4.  **Identificar la petición a la API (v2):** Recarga la página para observar las peticiones HTTP realizadas. Localiza la petición dirigida al endpoint de la API en su versión 2, que tendrá una URL similar a `/vulnerabilities/api/v2/user/`.
5.  **Modificar y reenviar la petición:** Haz clic derecho sobre la petición identificada en el paso anterior y selecciona la opción "Editar y reenviar" (el nombre exacto puede variar según tu navegador). Esto te permitirá manipular la petición antes de enviarla al servidor.
6.  **Cambiar la versión de la API en la URL:** En la herramienta de edición de la petición, localiza la URL de la petición y reemplaza la parte `v2` con `v1`. La URL modificada debería ser similar a `/vulnerabilities/api/v1/user/`.
7.  **Reenviar la petición modificada:** Envía la petición con la URL modificada al servidor.
8.  **Analizar la respuesta:** Examina la respuesta del servidor a esta nueva petición dirigida a la versión 1 de la API. Esta respuesta contendrá información sensible.
9.  **Obtener contraseñas hasheadas:** La respuesta de la versión v1 de la API mostrará las contraseñas de los usuarios en formato hash.
10. **Decodificar los hashes (Opcional):** Copia los hashes obtenidos en el paso anterior y utiliza herramientas de cracking de hashes como `hashcat` o servicios online como [CrackStation](https://crackstation.net/) para intentar descifrar las contraseñas originales.

## Resumen de la Vulnerabilidad

La vulnerabilidad se explota al evitar la versión presumiblemente más segura de la API (v2) y acceder a una versión anterior y menos protegida (v1). Esta versión vulnerable expone información confidencial, como las contraseñas de los usuarios en formato hash, lo que permite a un atacante obtener acceso a credenciales que podrían ser descifradas.