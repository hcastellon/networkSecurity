# Laboratorio 4: Implementación de SSH y VPN

En este laboratorio vamos a configurar los servidores web ([del laboratorio 3](https://github.com/hcastellon/networkSecurity/blob/main/week4/Lab3.md)) para que sean accedidos por medio del protocolo SSH con llave asimétrica. El objetivo de acceder estos servidores por medio de SSH es que estos, por lo general, requieren tareas de administración como reconfiguración, resolución de problemas, monitoreo, etc. Para hacer el escenario mas completo, en la segunda parte del laboratorio vamos a agregar una VPN. Esta nos permitirá acceder a los servidores desde Internet (simulado) porque no siempre nos encontraremos en la red interna.


## Parte 1: Instalación y configuración de SSH para el acceso remoto 

Para configurar los servidores web de manera que un cliente  pueda acceder por medio de SSH utilizando autenticación por clave pública, se deben seguir los pasos a continuación:

### 1. Configuración de los servidores Web

#### Paso 1: Instalación de SSH en los servidores

Primero, es necesario asegurarnos de instalar SSH en los servidores. Instalamos el paquete con el siguiente comando:

```
apt update
apt install -y openssh-server
```

Luego de haber instalado el paquete, podemos verificar si el servicio esta ejecutándose con el comando `service ssh status`. Si nos muestra que el servicio no esta corriendo entonces podemos ejecutar el comando `service ssh start`.

#### Paso 2: Configurar las Claves SSH

1. **Generar un par de claves SSH en el cliente:**

   Lo siguiente que debemos hacer es crear un cliente y generar las llaves. 
   Crea un contenedor cliente con el comando `docker run -it --name nombre_contenedor --network red_servidores ubuntu "sh"`, donde `red_servidores` debe ser la red donde se encuentran conectados los servidores web.
   Cuando se conecte la consola del cliente, instala los siguientes paquetes: 
   `apt update && apt install -y openssh-client`.
   Una vez finalizada la instalación, es momento de generar las claves SSH en el cliente:

   ```
   ssh-keygen -t rsa -b 4096 -C "cuenta_de_correo@ejemplo.com"
   ```
   Cuando solicite el directorio en el cual guardar las llaves, presiona Enter (dejando en blanco) para guardar en el directorio por defecto. De igual manera no especifiques `passphrase` cuando lo solicite.

   Esto generará un par de claves (privada y pública) en `/root/.ssh/id_rsa` y `/root/.ssh/id_rsa.pub`, respectivamente.

2. **Crear usuario para SSH:**

    Ahora necesitamos crear un usuario que nos permita conectarnos por medio de SSH al servidor. Para ello, vamos a ejecutar el comando `useradd` de la siguiente manera:
    `useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1000 usuario_admin` donde

    `useradd`: El comando utilizado para agregar un nuevo usuario.
    `-rm`: El flag `-r` le dice al sistema que se debe crear una cuenta de sistema, y la opcióm `-m` crea un directorio de inicio para el usuario.
    `-d /home/ubuntu`: Configura el directorio inicial a `/home/ubuntu`.
    `-s /bin/bash`: Esto establece el shell por defecto del usuario a bash.
    `-g root`: Esto establece el grupo primario del usuario en el grupo 'root'.
    `-G sudo`: Esto añade el usuario al grupo 'sudo', dándole privilegios administrativos.
    `-u 1000`: Esto establece el ID del usuario en 1000.
    
    Ahora que hemos creado el usuario necesitamos configurar la clave de acceso con el comando `echo 'usuario_admin:contraseña_segura' | chpasswd`.

3. **Copiar la clave pública al servidor remoto:**

   Utiliza `ssh-copy-id` (recomendable) para copiar la clave pública al servidor remoto. Sustituye `usuario` y `direccion_IP` con el nombre de usuario y la dirección IP interna del servidor remoto.

   ```
   ssh-copy-id usuario@servidor_remoto
   ```
    Si todo bien, debería solicitarte la contraseña (que hemos definido en el paso anterior) para el usuario.
   Alternativamente, puedes copiar la clave pública manualmente:

   ```
   cat ~/.ssh/id_rsa.pub | ssh user@backend_server_ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
   ```

   Una vez copiada la llave, nos debería enviar un mensaje que la llave ha sido agregada y nos invita a que ingresemos al servidor. Puedes verificar el ingreso correcto con el comando `ssh usuario_admin@direccion_IP`. Deberías poder ingresar sin ningún inconveniente.


#### Paso 3: Configurar el Servidor SSH para Autenticación por Clave Pública

1. **Editar la configuración del servidor SSH:**

   Abre el archivo de configuración del servidor SSH:

   ```
   nano /etc/ssh/sshd_config
   ```
   Puedes utilizar cualquier otro editor de texto que tengas disponible.

   Asegúrate de que las siguientes líneas estén configuradas correctamente:

   ```conf
   PubkeyAuthentication yes
   PasswordAuthentication no
   ChallengeResponseAuthentication no
   ```
   Si se encuentran comentadas, puedes quitar el comentario. 

2. **Reiniciar el servicio SSH:**

   Aplica los cambios reiniciando el servicio SSH:

   ```
   service ssh restart
   ```

## Parte 2: Implementación de una VPN

### Pasos para creación de la VPN

1. **Crear el archivo Docker Compose:**

   Crea un archivo llamado `docker-compose.yaml` en tu directorio de trabajo con el siguiente contenido:

   ```yaml
   version: '3.8'

   services:
     wireguard:
       image: linuxserver/wireguard
       container_name: wireguard
       environment:
         - PUID=1000
         - PGID=1000
         - TZ=Etc/UTC
         - SERVERURL=direccion_IP
         - SERVERPORT=51820
         - PEERS=cliente1
         - PEERDNS=auto
         - INTERNAL_SUBNET=10.0.0.0/24
       volumes:
         - ./config:/config
       ports:
         - "51820:51820/udp"
       networks:
         red-proxy:
           ipv4_address: direccion_IP
       cap_add:
         - NET_ADMIN
         - SYS_MODULE
       sysctls:
         - net.ipv4.conf.all.src_valid_mark=1
         - net.ipv4.ip_forward=1
       restart: unless-stopped

     
   ```

3. **Configurar el servidor VPN:**

   - **SERVERURL:** Reemplaza `direccion_IP` con la dirección IP del servidor.
   - **SERVERPORT:** Puedes cambiar el puerto si es necesario, pero lo mejor es dejarlo por defecto.
   - **PEERS:** Especifica el nombre de los clientes. En este caso, se llama `cliente1`.

4. **Construir y ejecutar el entorno Docker:**

   En el directorio donde has creado los archivos, ejecuta:

   `docker-compose up`

6. **Configurar el cliente WireGuard:**

   Después de que el contenedor de WireGuard esté en funcionamiento, debes obtener la configuración del cliente. La configuración estará en el directorio `./config/peer_cliente1`.

   - Copia el archivo de configuración del cliente desde el directorio `./config/peer_cliente1/cliente1.conf` a tu máquina cliente.
   - Instala WireGuard en una máquina cliente. Para ello, lanza un contenedor con la imagen de Ubuntu y ejecuta los siguientes comandos:

     ```
     apt update && apt install -y wireguard iputils-ping resolvcong
     ```

   - Copia el archivo de configuración del servidor VPN en la ruta `/etc/wireguard/wg0.conf`y utilízalo para conectar el cliente a la VPN:

     ```
     wg-quick up cliente1.conf
     ```

7. **Probar la conexión:**

   - Con la VPN activa en el cliente, intenta acceder al servidor web a través de la IP interna del contenedor (por ejemplo, `ping 10.0.0.2` si `10.0.0.2` es la IP asignada al contenedor  dentro de la red VPN).
   - Visualiza tanto en el servidor como en el cliente el estado de conexión con el comando `wg`.

### Detalles Adicionales

- **Configuración del Servidor WireGuard:**
  El archivo de configuración del servidor WireGuard se encuentra en `./config/wg0.conf` y puede personalizarse según sea necesario.






