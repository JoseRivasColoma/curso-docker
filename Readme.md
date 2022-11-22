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

- `docker build <path>`
- `docker build .`: Construye una imagen con todas las dependencias en *este directorio*
- `docker run <image_id>`: Levanta un nuevo contenedor a partir de una imagen
- `docker run -p <local_port>:<container_port> -d <image_id>`
    * Levanta un contenedor y mapea el puerto local con el puerto del contenedor
    * -p: port
    * -d: modo detached
- `docker stop <container_id>|<container_name>`
    * Detiene un contenedor
- `docker rm -f <container_id>|<container_name>`
    * Elimina un contenedor