# Utilizamos la imagen oficial de Ubuntu como base
FROM ubuntu:latest

# Actualizamos los paquetes del sistema operativo
RUN apt-get update && apt-get upgrade -y

# Instalamos los paquetes requeridos
RUN apt-get install -y gpg openssl hashcat man-db

# Comando por defecto al iniciar el contenedor
CMD ["bash"]
