# Ataques más comunes en Redes TCP/IP


A continuación, se mencionan los ataques más comunes a los que son vulnerables las redes TCP/IP. Existen muchos más, pero en esta lista se resumen los más destacados junto con una breve explicación general de lo que el ataque es.

1. **Sniffing o interceptación**: Un atacante puede utilizar la herramienta de monitoreo de red para capturar paquetes y tratar de “leer” la información contenida en tales paquetes. Información sensible puede ser comprometida en caso de que no se utilicen mecanismos de confidencialidad para enviar la información por las comunicaciones de red.

2. **Ataques de autenticación**: La autenticación es el proceso de confirmar la identidad de un usuario, es decir, de verificar que es quien dice ser. Los ataques de autenticación están dirigidos a descubrir las contraseñas (u otra forma de autenticación) para cuentas de usuarios. Los procedimientos más comunes para este tipo de ataques son el ataque por diccionario y el ataque por fuerza bruta.


3. **Hijacking de conexión**: El atacante abre una conexión (TCP) legitima a un servicio y recibe un número de secuencia. Luego personifica a otro cliente, enviando un paquete con la dirección en el campo de dirección de origen y el número de secuencia consistente (que el servicio espera que sea). Si el servicio acepta la comunicación, entonces el servicio cree que está ‘hablando’ con el cliente cuando en realidad lo está haciendo con el atacante.


4. **Falsificación (IP, ARP, MAC, DNS, etc.)**: Con las falsificaciones, el atacante modifica un paquete o la información en sí para personificar sea un recurso o una persona. Esta falsificación se puede dar con diferentes protocolos, tal como puede ser con el protocolo de Internet, el protocolo de resolución de direcciones, con la dirección de control de acceso al medio, con el sistema de nombres de dominio, etc.

5. **Hombre en el medio (MitM)**: El atacante modifica los datos cuando estos se encuentran entre el origen y el destino. Así, este se posiciona donde pueda capturar la información de origen y destino, la modifica y les hace creer que la información modificada proviene de la persona indicada.

6. **Denegación de servicio, denegación de servicio distribuido**: Estos ataques se llevan a cabo porque el atacante envía una gran cantidad de paquetes hacia un servidor o servicio o también puede explotar una vulnerabilidad que no permite que usuarios legítimos (de un servicio) puedan accederlo.
 
7. **Inyección SQL**: Es una de las técnicas de hacking web más común. Se lleva a cabo cuando el atacante introduce código malicioso en sentencias que el motor de base de datos puede entender, mediante la entrada de datos en la web. Normalmente sucede cuando páginas web solicitan información de ingreso (ID usuario y contraseña) y esta se ejecuta en una base de datos.

8. **Explotación de contraseñas predeterminadas**: Este tipo de ataque tiene que ver con que muchos dispositivos, plataformas, productos, entre otros, se pre configuran con credenciales predeterminadas. Algunos no cambian estas credenciales una vez que compran o implementan las soluciones, entonces los atacantes pueden.

9. **Explotación de encriptación débil**: Cada algoritmo de encriptación que se puede utilizar para cifrar información tiene un grado de complejidad específico. Entre más complejo sea más difícil de ‘craquear’ será. Este ataque se encarga de explotar algoritmos de complejidad baja o que pueden ser quebrantados rápidamente. Este ataque puede tener lugar porque la persona que aplicó el algoritmo desconoce la ‘debilidad’ que lo caracteriza.
