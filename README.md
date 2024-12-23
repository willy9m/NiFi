# Como instalar y desplegar NiFi

- Linux/Unix/macOS
  - Descargar el archive de instalación del siguiente enlace:

    <https://archive.apache.org/dist/nifi/1.27.0/nifi-1.27.0-bin.zip>
  - Descomprimir en el Directorio de instalación deseado:
    ```
    unzip nifi-1.27.0-bin.zip
    cd nifi-1.27.0/
    ```

  - Desde la ruta <installdir>/bin, ejecutar el siguiente comando ./nifi.sh <command>:
    - start: iniciar NiFi en segundo plano
    - stop: parar NiFi que está iniciado en segundo plano
    - status: informa del estado actual de NiFi
    - run: incia NiFi en primer plano y espera el lacombinacion Ctrl-C para ejecutar la parada de NiFi
    - install: instalar NiFi como un servicio que puede ser controlado por los siguientes comandos
      - service nifi start
      - service nifi stop
      - service nifi status
     
- Windows
  - Descomprimir en el Directorio de instalación deseado
  - Navegar hasta el siguiente directorio <installdir>/bin
  - Hacer doble click sobre run-nifi.bat. Esto iniciará NiFi en Segundo plano es esperará a la combinación Ctrl-C para ejecutar la parada de NiFi.
    Otra opcion es abrir una consola cmd, establecerse en el directorio anterior y ejecutar nifi.cmd start
  - Para ver el estado actual de NiFi, hacer doble click sobre status-nifi.bat.
    Otra opcion es abrir una consola cmd, establecerse en el directorio anterior y ejecutar nifi.cmd status

**Puesta en marcha**

Para instalar Nifi, sólo hemos de descargar la última versión desde [https://nifi.apache.org](https://nifi.apache.org/) (en nuestro caso hemos descargado la version 1.27) y tras descomprimirla, debemos crear unas credenciales de acceso. Para ello, ejecutaremos el comando ./nifi.sh set-single-user-credentials <username> <password> indicando el usuario y contraseña que queramos.

Por ejemplo, nosotros hemos creado el usuario nifi/nifinifinifi:
```
./bin/nifi.sh set-single-user-credentials nifi nifinifinifi
```
A continuación, ya podemos arrancar Nifi ejecutando el comando ./nifi.sh start (se ejecutará en *brackgound*):
```
./bin/nifi.sh start
```
Si queremos detenerlo ejecutaremos ./bin/nifi.sh stop.

**Instalación con AWS**

Si quieres trabajar con una máquina en AWS has de seguir los siguientes pasos:

1. Crear una instancia EC2 (se recomienda elegir el tipo t3.large para tener 8GB RAM), con un grupo de seguridad que permita tanto las conexiones SSH como el puerto 8443.
1. Conectarnos via SSH a la *ipPublicaAWS* y descargar Nifi:
1. wget https://downloads.apache.org/nifi/1.15.0/nifi-1.15.0-bin.zip
1. unzip nifi-1.15.0-bin.zip
1. Tras meternos dentro de la carpeta recién creada, configurar el archivo conf/nifi.properties para permitir el acceso remoto. Para ello, modificaremos las siguientes propiedades:
1. nifi.web.https.host = 0.0.0.0
1. nifi.web.proxy.host = <ipPublicaAWS>:8443
1. Configurar el usuario con las credenciales de acceso
1. ./bin/nifi.sh set-single-user-credentials nifi nifinifinifi
1. Acceder desde un navegador a la dirección [https://ipPublicaAWS:8443/nifi](https://ippublicaaws:8443/nifi)

**Instalación con Docker**

Si no queremos instalarlo y hacer uso de un contenedor mediante Docker, ejecutaremos el siguiente comando:

docker run --name nifi -p 8443:8443 -d -e SINGLE\_USER\_CREDENTIALS\_USERNAME=nifi -e  SINGLE\_USER\_CREDENTIALS\_PASSWORD=nifinifinifi -e NIFI\_JVM\_HEAP\_MAX=2g apache/nifi:latest



