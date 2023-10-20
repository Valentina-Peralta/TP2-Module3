# Configuración de Servidores con Docker y SSH

Este repositorio contiene los archivos y configuraciones necesarios para crear y administrar un entorno con 4 servidores en Linux utilizando Docker y SSH. 

- **server1**: Configuración y Dockerfile para el primer servidor.

# Configuración de cada servidor

Este Dockerfile se utiliza para construir una imagen de contenedor que actúa como el servidor 1 en tu entorno Docker. A continuación, se explican las etapas y los comandos utilizados en el Dockerfile:

1. `FROM ubuntu:latest`: Esta línea indica que se basará en la imagen oficial de Ubuntu con la etiqueta "latest". Especifica la imagen base que se utilizará como punto de partida para construir la imagen del contenedor.

2. `RUN apt-get update && apt-get install -y openssh-server`: Estas líneas ejecutan dos comandos durante la construcción del contenedor. Primero, se actualiza la lista de paquetes disponibles en Ubuntu con `apt-get update`, y luego se instala el servidor SSH (`openssh-server`) con `apt-get install -y openssh-server`. Esto asegura que el servidor SSH esté instalado en el contenedor.

3. `RUN useradd -m user1 && echo "user1:password1" | chpasswd`: Estas líneas se utilizan para crear un usuario llamado "user1" en el contenedor. El usuario se crea con un directorio personal (`-m`) y se le asigna una contraseña "password1" utilizando `echo` y `chpasswd`. Ten en cuenta que para fines de seguridad, se recomienda utilizar una autenticación más segura, como la autenticación por clave pública, en lugar de contraseñas en entornos de producción.

4. `RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config`: Esta línea modifica el archivo de configuración del servidor SSH (`sshd_config`) para permitir el acceso como usuario "root". Cambia la configuración de `PermitRootLogin` de "prohibit-password" a "yes". Esta es una configuración que se debe tratar con precaución en entornos de producción, ya que permitir el acceso directo como "root" puede ser un riesgo de seguridad.

5. `RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config`: Esta línea modifica el archivo de configuración del servidor SSH para habilitar la autenticación por contraseña (`PasswordAuthentication`). Elimina el carácter "#" al principio de la línea para descomentarla.

6. `RUN service ssh start`: Inicia el servicio SSH en el contenedor para que esté listo para aceptar conexiones SSH.

7. `EXPOSE 22`: Esta línea expone el puerto 22 del contenedor, que es el puerto por defecto para SSH. Indica que el puerto 22 del contenedor estará disponible para ser mapeado a un puerto del host cuando se ejecute el contenedor.

8. `CMD ["/usr/sbin/sshd", "-D"]`: Define el comando que se ejecutará cuando se inicie el contenedor. En este caso, se ejecuta el servidor SSH (`sshd`) en modo demonio (`-D`), lo que permite que el contenedor escuche las conexiones SSH.


# Archivo de Docker Compose

Este archivo de Docker Compose se utiliza para definir y orquestar los servicios de los contenedores que componen tu entorno de servidores Docker. A continuación, se explican las secciones y los servicios definidos en el archivo:

## Versión de Docker Compose

- `version: '3'`: Indica la versión del formato de Docker Compose que se está utilizando. En este caso, se utiliza la versión 3.

## Definición de Servicios

Dentro de la sección `services`, se definen los diferentes servicios o contenedores que se crearán. Cada servicio tiene su propia configuración. En este caso, se definen cuatro servicios: `server1`, `server2`, `server3` y `bastion`.

### Servicio server1

- `build`: Indica que la imagen del contenedor para `server1` se construirá a partir de los archivos en el directorio `./server1`.

- `container_name`: Define un nombre específico para el contenedor de `server1`.

- `ports`: Mapea el puerto 22 del contenedor al puerto 2222 del host, permitiendo que el contenedor acepte conexiones SSH en el puerto 2222 del host.

- `networks`: Asocia el contenedor `server1` a la red `my_network` para que pueda comunicarse con otros contenedores en la misma red.

### Servicio server2

- `build`: Indica que la imagen del contenedor para `server2` se construirá a partir de los archivos en el directorio `./server2`.

- `container_name`: Define un nombre específico para el contenedor de `server2`.

- `ports`: Mapea el puerto 22 del contenedor al puerto 2223 del host.

- `networks`: Asocia el contenedor `server2` a la red `my_network`.

### Servicio server3

- `build`: Indica que la imagen del contenedor para `server3` se construirá a partir de los archivos en el directorio `./server3`.

- `container_name`: Define un nombre específico para el contenedor de `server3`.

- `ports`: Mapea el puerto 22 del contenedor al puerto 2224 del host.

- `networks`: Asocia el contenedor `server3` a la red `my_network`.

### Servicio bastion

- `build`: Indica que la imagen del contenedor para `bastion` se construirá a partir de los archivos en el directorio `./bastion`.

- `container_name`: Define un nombre específico para el contenedor de `bastion`.

- `ports`: Mapea el puerto 22 del contenedor al puerto 2221 del host.

- `networks`: Asocia el contenedor `bastion` a la red `my_network`.

## Definición de Red

- `networks`: Esta sección define una red llamada `my_network`, que se utiliza para conectar todos los contenedores entre sí. Los contenedores que están en la misma red pueden comunicarse utilizando los nombres de host de los contenedores como direcciones.




## Uso

Sigue estos pasos para levantar los servidores en tu entorno local:

1. Asegúrate de tener Docker instalado en tu sistema.
2. Clona este repositorio en tu máquina local.

   ```bash
   git clone https://github.com/tuusuario/turepositorio.git

### Levantar los contenedores

Dentro de la carpeta del repositorio, ejecutar:
 
   ```bash
     docker-compose up -d
   ```

Ahora los servidores estarán disponibles para su acceso a través de SSH.

### Desde el servidor bastión, puedes conectarte a server1, server2, o server2.

Ingresar al servidor bastión:

   ```bash
    docker exec -it bastion /bin/bash
   ```
  

Conectarse desde el servidor bastión al servidor 1:

   ```bash
    ssh user1@server1
   ```
## Acceso a los Servidores

**Servidor Bastión:**

Usuario: bastionuser

Contraseña: bastionpassword



**Server1:**

Usuario: user1

Contraseña: password1



**Server2:**

Usuario: user2

Contraseña: password2



**Server3:**

Usuario: user3

Contraseña: password3



