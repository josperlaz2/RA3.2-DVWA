# Explotación Nivel Medio DVWA

## 1. SQL Injection Bloqueada

La función `mysql_real_escape_string` implementada en este nivel previene inyecciones SQL directas al escapar caracteres especiales en la entrada del usuario. Los payloads del nivel bajo ya no son efectivos.

## 2. Fuerza Bruta con Hydra

Aunque la inyección SQL directa está bloqueada, aún podemos intentar un ataque de fuerza bruta para adivinar la contraseña.

**Comando:**
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt <IP_SERVIDOR> http-get-form "/dvwa/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:H=Cookie:PHPSESSID=<TU_SESION>;security=medium:F=Username and/or password incorrect."
```

Nota: Reemplaza <IP_SERVIDOR> con la dirección IP de tu servidor DVWA y <TU_SESION> con el valor actual de tu cookie PHPSESSID.

Resultado: Hydra intentará cada contraseña en la lista rockyou.txt para el usuario "admin". Eventualmente, si la contraseña se encuentra en la lista, Hydra la revelará. El proceso puede ser más lento en comparación con el nivel bajo, ya que cada intento requiere una petición HTTP completa.

<img src="../../BruteForceMedium01.png">
