# Docker

### Imágenes

- Templates/Blueprints para contenedores
- Contiene el código + herramientas requeridas (node, php, etc.)

### Contenedores

- La "unidad de software" en ejecución
- Muchos contenedores pueden estar basados en una sola imágen

# Comandos

- `docker run <image>:<tag>`: 
    * Crea un contenedor basado en la imágen. 
    * Si la imagen no incluye el tag, por defecto crea el contenedor utilizando la última.
    * Solamente crea el contenedor, no lo ejecuta.
    * Por defecto, un contenedor se crea de manera aislada de su entorno.
    * El contenedor no está expuesto para un usuario u otros contenedores

- `docker ps -a`:
    * ps: processes
    * -a: all the processes for all the containers

- `docker run -it <image>`:
    * -it: inicia una sesión interactiva con la consola de la imagen
    * Crea un nuevo contenedor

# Imágenes Personalizadas

Se utilizan para adaptar los contenedores al entorno de ejecución.

### Dockerfile

- Archivo en donde se definen las instrucciones para la construccion de nuestras propias imágenes
- Ejemplo

- Traemos una copia física de la imagen a nuestro entorno local
`FROM <image>:<tag>`
- Establece el directorio de trabajo *dentro del contenedor* 
- Todos los comandos posteriores serán ejecutados en la ruta definida
`WORKDIR /app`
- Qué archivos que viven en nuestra máquina local deben estar en la imagen
- Primer path (.): donde están los archivos
- Segundo path(/app por el workdir): en qué lugar del contenedor serán alojados
`COPY . /app`
- Construiremos todas las dependencias de la imágen
`RUN npm install`
- Indicamos a docker, qué puerto será del contenedor será expuesto a nuestra máquina local
`EXPOSE 80`
- CMD: Commando
- Diferencia con RU, no se ejecuta cuando la imágen se está creando, sino que cuando se arranca el contenedor
`CMD ["node", "server.js"]`

### Construir una imágen a partir de un Dockerfile
##### Cada instrucción en el dockerfile representa una capa para docker

- `docker build <path>`
- `docker build .`: Construye una imagen con todas las dependencias en *este directorio*
- `docker run <image_id>`: Levanta un nuevo contenedor a partir de una imagen
- `docker run -p <local_port>:<container_port> -d --name <container_name> <image_id>`
    * Levanta un contenedor y mapea el puerto local con el puerto del contenedor
    * -p: port
    * -d: modo detached
    * -it: interactive mode
    * --name: Establece el nombre de un contenedor
    * --rm: Crea la imágen y la elimina al momento de su detención
- `docker stop <container_id>|<container_name>`
    * Detiene un contenedor
- `docker rm -f <container_id>|<container_name>`
    * Elimina un contenedor

*Importante* Una imágen es cerrada, por lo tanto, una vez creada, no es posible editar el contenido interno desde afuera.

#### Docker trabaja con una arquitectura basada en capas (layer based architecture).

# Administración de imágenes y contenedores

- `docker --help`: Ver todas las posibles opciones con las que nos puede ayudar docker
- `docker ps -a`: Lista todos los contenedores, incluso los detenidos
- `docker start <container_name>`: Inicia un contenedor
    * -d: detached mode
    * -a: attached mode
- `docker attach <container_name>`: Establece el contenedor en modo escucha/atachado.
- `docker logs`: Permite acceder al historial de logs del contenedor
    * -f: follow, modo escucha
- `docker rm <container_name>`: Elimina un contenedor
    * -f: force, fuerza la eliminación
    
- `docker images`: Lista las imágenes creadas

- `docker rmi <image_name|image_id>`: Remueve la imagen y todos los contenedores creados a partir de esta

- `docker image prune -a`: Elimina todas las imágenes que no están en uso

- `docker run --rm <image_id>`: Crea la imágen y la elimina al momento de su detención

# Inspecting images

- `docker image inspect <image_id>`: Inspecciona las propiedades de una imagen

- `docker cp <source_path> <destination_path>`: Copia archivos en el filesystem local, o dentro del contenedor

Ejemplo

- `docker cp dummy/. exciting_swartz:/test`: Copia una directorio local hacia un contenedor

# Naming images and containers

- `docker build -t <image_name>:<image_tag>`
    * -t: tag

# Login and logout from Dockerhub

- `docker login`: Ingresar
- `docker logout`: Salir

# Upload & pull images to & from Dockerhub

- `docker push <username>/<image_name>:<image_tag>` (el nombre de la imagen debe ser identico a <username>/<image_name>)
- `docker push joserivascoloma/node-app-test:tagname`
- `docker tag <old_image_name>:<old_image_tag> <new_image_name>:<new_image_tag>`: Crea una copia de una imagen y renombra el tag

- `docker pull <image_name>:<tag_name>` : Descarga una imágen desde Dockerhub