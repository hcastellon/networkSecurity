## Objetivo

El objetivo de este laboratorio es aprender a utilizar tres herramientas de criptografía que permiten realizar diferentes operaciones con datos y comunicaciones. Estas herramientas son:

- GPG: Una herramienta que implementa el estándar OpenPGP, que permite cifrar, descifrar, firmar y verificar archivos y mensajes con criptografía de clave pública.
- OpenSSL: Una herramienta que implementa el protocolo SSL/TLS, que permite establecer conexiones seguras entre clientes y servidores, así como generar y gestionar certificados digitales.
- Hashcat: Una herramienta que implementa el ataque de fuerza bruta y el ataque de diccionario, que permiten recuperar contraseñas o claves a partir de sus hashes o cifrados.

## Ejercicio 1
Encriptación de un archivo con criptografía simétrica (con GPG)

### Pasos

1. Descarga el archivo `docker-compose.yml` que contiene la configuración de los servicios y dispositivos que se van a utilizar en el laboratorio. 
2. Ejecuta el comando `docker-compose up -d` desde un terminal (en el directorio actual deben existir los archivos `docker-compose.yaml` y `Dockerfile`)para levantar los servicios y dispositivos.
3. Crea un archivo de texto llamado mensaje1.txt con el siguiente contenido: "Hola, este es un mensaje secreto".
4. En el dispositivo `crypto_machine`, utiliza la herramienta GPG para cifrar el archivo mensaje.txt con una clave simétrica con el algoritmo AES-256. Utilice la ayuda del sistema (`man gpg`) para encontrar la sintaxis del comando.
5. Copie el archivo *encriptado* del dispositivo `crypto_machine` hacia el dispositivo `crypto_machine2` por medio del comando `docker cp`.
6. En el dispositivo `crypto_machine2`, muestre en pantalla el contenido del archivo para verificar que está encriptado. Luego, utilice la herramienta GPG para descifrar el archivo encriptado.

## Ejercicio 2
Encriptación con criptografía asimétrica (con GPG)

## Pasos

1. Utilizar la herramienta GPG para generar un par de claves pública y privada, utilizando el algoritmo RSA de 2048 bits. Utilice la ayuda del sistema (`man gpg`) para encontrar la sintaxis del comando. Se generará un par de claves que se almacenarán en el directorio .gnupg del equipo local.
2. Cree un duplicado del archivo `mensaje1.txt` con el nombre `mensaje2.txt`. 
3. Utilizar la herramienta GPG para cifrar el archivo mensaje2.txt con la clave pública generada, utilizando el algoritmo RSA. El comando a ejecutar es: `gpg --encrypt --recipient correo@electronico mensaje2.txt`. Se debe sustituir `correo@electronico` por el correo electrónico introducido al generar las claves. Se generará un archivo llamado mensaje2.txt.gpg que contiene el mensaje cifrado.
4. Copie el archivo *encriptado* del dispositivo `crypto_machine` hacia el dispositivo `crypto_machine2` por medio del comando `docker cp`.
5. Utilizar la herramienta GPG para descifrar el archivo mensaje.txt.gpg con la clave privada generada, utilizando el algoritmo RSA. El comando a ejecutar es: `gpg --decrypt mensaje.txt.gpg`. Se debe introducir la contraseña de la clave privada cuando se solicite. Se mostrará el mensaje original en la salida estándar.

## Ejercicio 3
Firmas digitales (con GPG)

## Pasos
1.  Cree un duplicado del archivo `mensaje1.txt` con el nombre `mensaje3.txt`.
2.  Utilizar la herramienta GPG para firmar el archivo mensaje3.txt con la clave privada generada, utilizando el algoritmo RSA. El comando a ejecutar es: `gpg --sign mensaje3.txt`. Se debe introducir la contraseña de la clave privada cuando se solicite. Se generará un archivo llamado mensaje3.txt.gpg que contiene el mensaje firmado.
3. Utilizar la herramienta GPG para verificar el archivo mensaje3.txt.gpg con la clave pública generada, utilizando el algoritmo RSA. El comando a ejecutar es: `gpg --verify mensaje3.txt.gpg`. Se mostrará el resultado de la verificación en la salida estándar, indicando si la firma es válida o no.

## Ejercicio 4
Generación de certificados (con OpenSSL)

## Pasos

1. Utilizar la herramienta OpenSSL para generar un certificado digital autofirmado, utilizando el algoritmo RSA de 2048 bits. El comando a ejecutar es: `openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365`. Se debe introducir la contraseña de la clave privada y la información del certificado cuando se soliciten. Se generarán dos archivos llamados key.pem y cert.pem que contienen la clave privada y el certificado digital, respectivamente.
2. Utilizar la herramienta OpenSSL para mostrar el contenido del certificado digital generado, utilizando el formato X.509. El comando a ejecutar es: `openssl x509 -in cert.pem -text -noout`. Se mostrará el contenido del certificado en la salida estándar, incluyendo la información del emisor, del sujeto, de la validez, del algoritmo, de la clave pública y de la firma.
3. Utilizar la herramienta OpenSSL para establecer una conexión segura con un servidor web que utiliza el protocolo HTTPS, utilizando el protocolo SSL/TLS. El comando a ejecutar es: `openssl s_client -connect www.example.com:443`. Se debe sustituir www.example.com por el nombre del servidor web al que se quiere conectar. Se mostrará el resultado de la conexión en la salida estándar, incluyendo el certificado del servidor, el algoritmo de cifrado, el intercambio de claves y el mensaje de bienvenida.

## Ejercicio 5
Recuperación de claves por medio de hashes (con Hashcat)

## Pasos

1. Utilizar la herramienta Hashcat para generar el hash MD5 del archivo mensaje.txt, utilizando el algoritmo MD5. El comando a ejecutar es: `hashcat --example-hashes | grep MD5 | cut -d " " -f 2 > hash.txt`. Se generará un archivo llamado hash.txt que contiene el hash MD5 del mensaje.
2. Utilizar la herramienta Hashcat para recuperar el mensaje original a partir del hash MD5 generado, utilizando el ataque de fuerza bruta, el algoritmo MD5 y el conjunto de caracteres alfanuméricos. El comando a ejecutar es: `hashcat -m 0 -a 3 hash.txt ?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a`. Se mostrará el resultado del ataque en la salida estándar, indicando el mensaje original y el tiempo empleado.

## Resultados esperados

Al realizar el laboratorio, se espera que los estudiantes puedan observar y explicar lo siguiente:

- La diferencia entre el cifrado simétrico y el cifrado asimétrico. 
- La diferencia entre la firma digital y el certificado digital. 
- La diferencia entre el protocolo SSL/TLS y el protocolo HTTPS. E
- La diferencia entre el hash y el cifrado. 
- La diferencia entre el ataque de fuerza bruta y el ataque de diccionario. 