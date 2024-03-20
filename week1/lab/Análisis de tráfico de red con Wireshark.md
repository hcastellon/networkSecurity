# Análisis de tráfico con Wireshark de un ataque ARP spoofing

## Objetivo
El objetivo de este laboratorio es aprender a utilizar la herramienta Wireshark para analizar el tráfico de red entre diferentes servicios y dispositivos. Wireshark es un analizador de protocolos de red que permite ver el contenido y las características de los paquetes que circulan por una red. Esto es útil para comprender el funcionamiento de los protocolos de red, detectar problemas de comunicación, identificar anomalías y ataques, y realizar auditorías de seguridad.

## Arpspoofing y su utilización en escenarios de ataque

El arpspoofing es una técnica de ataque en la que un atacante engaña a los dispositivos de una red para que envíen su tráfico a través de él. Esto se logra enviando mensajes ARP falsos que asocian la dirección IP de la víctima con la dirección MAC del atacante. Como resultado, el atacante puede interceptar y modificar el tráfico de red entre los dispositivos.

El arpspoofing se utiliza en escenarios de ataque para realizar diversas actividades maliciosas, como el robo de información confidencial, la suplantación de identidad, el espionaje de comunicaciones y la manipulación de datos. Por ejemplo, un atacante puede utilizar arpspoofing para interceptar las credenciales de inicio de sesión de los usuarios o redirigir su tráfico a sitios web falsos.


## Pasos
1. Descargar e instalar la herramienta Wireshark en el equipo local. Se puede obtener desde esta [URL](https://www.docker.com/products/docker-desktop/).
2. Descargar el archivo `docker-compose.yml` que contiene la configuración de los servicios y dispositivos que se van a utilizar en el laboratorio. 
3. Ejecutar el comando `docker-compose up -d` desde un terminal (en el directorio actual deben existir los archivos `docker-compose.yaml` y `Dockerfile`)para levantar los servicios y dispositivos. Estos son:
   - Un servidor web con la página por default de Tomcat. El servidor web está expuesto en el puerto 8888 y tiene la dirección IP 172.20.0.6.
   - Un cliente web que accede al servidor web desde la consola utilizando la herramienta CURL. El cliente web tiene la dirección IP 172.20.0.4.
   - Un atacante que intenta interceptar y modificar el tráfico de red entre el cliente web y el servidor web. El atacante tiene la dirección IP 172.20.0.5.


4.  En el servidor web, abrir una terminal, por medio del comando `docker exec -it nombre_contenedor "bash"` y ejecutar el comando `tcpdump -i eth0 -w server.pcap` para guardar el tráfico de red que llega al servidor web en un archivo llamado server.pcap. Si la aplicación tcpdump no existe, es necesario actualizar la paquetería e instalar el paquete(`apt update && apt install tcpdump -y`).
5.  En el atacante, abrir otra terminal por medio del comando `docker exec -it nombre_contenedor "bash"` y ejecutar el comando `tcpdump -i eth0 -w attacker.pcap` para guardar el tráfico de red que llega al atacante en un archivo llamado attacker.pcap.
6.  En el atacante, abrir una segunda terminal (siempre usando el comando `docker exec -it nombre_contenedor "bash"`) y ejecutar el comando `arpspoof -i eth0 -t 172.20.0.4 172.20.0.1` para realizar un ataque de ARP spoofing que engaña al cliente web para que envíe el tráfico destinado para el servidor web al atacante, en lugar de enviarlo al servidor web. Este ataque se basa en enviar mensajes ARP falsos que indican que la dirección MAC del servidor web es la misma que la del atacante.
7.  En el cliente web, ejecutar el comando `curl 172.20.0.6:8888`. Como resultado, debería ver un error 404 de que el recurso que se esta solicitando no existe. 
8.  En el servidor web, detener el comando tcpdump con `Ctrl+C` y copiar el archivo `server.pcap` al equipo local con el comando `docker cp [nombre_contenedor]:server.pcap ..`.
9.  En el atacante, detener los comandos arpspoof y tcpdump con `Ctrl+C` y copiar el archivo attacker.pcap al equipo local con el comando `docker cp [nombre_contenedor]:attacker.pcap ..`
10. Detener los servicios y dispositivos con el comando `docker compose down`.
11. Abre los archivos `server.pcap` y `attacker.pcap` (o como los hayas nombrado) con la herramienta Wireshark y analizar el contenido de los paquetes capturados.

## Resultados esperados
Al analizar el contenido de los paquetes capturados, se espera que puedas observar y explicar lo siguiente:

- La diferencia entre el tráfico de red que llega al servidor web y al atacante. 
- La forma en que el atacante realiza el ataque de ARP spoofing. 
- La información que el atacante puede obtener del tráfico de red. 
- La forma en que el atacante puede modificar el tráfico de red. 
- Las medidas que se podrían tomar para evitar o mitigar el ataque de *ARP spoofing*. 
