# Configuración de Servidores con Docker y SSH

Este repositorio contiene los archivos y configuraciones necesarios para crear y administrar un entorno con 4 servidores en Linux utilizando Docker y SSH. 

- **server1**: Configuración y Dockerfile para el primer servidor.
- **server2**: Configuración y Dockerfile para el segundo servidor.
- **server3**: Configuración y Dockerfile para el tercer servidor.
- **bastion**: Configuración y Dockerfile para el servidor bastión.
- **docker-compose.yml**: Archivo de Docker Compose para orquestar los contenedores.

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



