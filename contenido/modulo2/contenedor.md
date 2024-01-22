# Ejecución simple de contenedores

En esta apartado vamos a crear un contenedor donde vamos a especificar el comando que debe ejecutar a partir de la imagen `ubuntu`.
En este caso vasmo a descargar priemro la imagen del registro público Docker Hub, y a continuación crearemos el contenedor.

Para descargar una imagen de Docker Huv, ejecutamos el comando `docker pull`:

```bash
$ docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
29202e855b20: Pull complete 
Digest: sha256:e6173d4dc55e76b87c4af8db8821b1feae4146dd47341e4d431118c7dd060a74
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
```

A continuación, creamos y ejecutamos un nuevo contenedor indicando el comando que va a ejecutar:

```bash
$ docker run ubuntu echo 'Hello world' 
Hello world
```

Comprobamos que el contenedor ha ejecutado el comando que hemos indicado y se ha parado:

```bash
$ docker ps -a
CONTAINER ID        IMAGE              COMMAND                  CREATED               STATUS                      PORTS               NAMES
3bbf39d0ec26        ubuntu             "echo 'Hello wo…"        31 seconds ago        Exited (0) 29 seconds ago                       wizardly_edison
```

Aunque lo veremos en el próximo apartado, los contenedores que hemos creado se nombran de manera aleatoria. Podemos cambiar el nombre de cualquier contenedor usando el comando `docker rename`:

```bash
$ docker rename wizardly_edison contenedor_ubuntu
```

Y comprobamos que el nombre ha cambiado:

```bash
$ docker ps -a
CONTAINER ID   IMAGE     COMMAND                CREATED         STATUS                     PORTS     NAMES
5bd9366588ef   ubuntu    "echo 'Hello world'"   6 minutes ago   Exited (0) 5 minutes ago             contenedor_ubuntu
```

## Visualizando las imágenes de nuestro registro local

Con el comando `docker images` (también se puede usar `docker image ls`, `docker image list`) podemos visualizar las imágenes que ya tenemos descargadas en nuestro registro local:

```bash
$ docker images
REPOSITORY          TAG                 IMAGE ID           CREATED             SIZE
ubuntu                     latest                  99284ca6cea0   3 weeks ago    77.8MB
hello-world                latest                  9c7a54a9a43c   7 weeks ago    13.3kB
```