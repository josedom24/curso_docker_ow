# Ejecución simple de contenedores

En este apartado vamos a crear un contenedor especificando el comando que debe ejecutar a partir de la imagen `ubuntu`.
En este caso vamos a descargar primero la imagen del registro público Docker Hub, y a continuación crearemos el contenedor.

Para descargar una imagen de Docker Hub, ejecutamos el comando `docker pull`:

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

Los contenedores que hemos creado se nombran de manera aleatoria. Podemos cambiar el nombre de cualquier contenedor usando el comando `docker rename`:

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
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
ubuntu        latest    e34e831650c1   11 days ago    77.9MB
hello-world   latest    d2c94e258dcb   8 months ago   13.3kB
```

## ¿Qué ocurre cuando creamos un contenedor?

Para terminar podemos ver las distintas etapas por las que pasa la creación de un contenedor ejecutando `docker events`. Para ello en una terminal ejecutamos el comando:

```bash
$ docker events
```

Y en otro terminal ejecutamos un contenedor:

```bash
$ docker run ubuntu echo 'Hello world' 
```

En el primer terminal veremos las operaciones que se han dio produciendo:

```
2024-01-22T21:08:37.947946863+01:00 container create 6167a0dbc036143fcd9d3b2783f6d507fbd72900622f8929a5424dee9264e9f5 (image=ubuntu, name=loving_jennings, org.opencontainers.image.ref.name=ubuntu, org.opencontainers.image.version=22.04)
2024-01-22T21:08:37.953203114+01:00 container attach 6167a0dbc036143fcd9d3b2783f6d507fbd72900622f8929a5424dee9264e9f5 (image=ubuntu, name=loving_jennings, org.opencontainers.image.ref.name=ubuntu, org.opencontainers.image.version=22.04)
2024-01-22T21:08:38.310303971+01:00 network connect 7e6404027e1ec38230c4dc35f40079c6f6366cd61d73a472517e39f598cebab1 (container=6167a0dbc036143fcd9d3b2783f6d507fbd72900622f8929a5424dee9264e9f5, name=bridge, type=bridge)
2024-01-22T21:08:38.966836361+01:00 container start 6167a0dbc036143fcd9d3b2783f6d507fbd72900622f8929a5424dee9264e9f5 (image=ubuntu, name=loving_jennings, org.opencontainers.image.ref.name=ubuntu, org.opencontainers.image.version=22.04)
2024-01-22T21:08:39.392255443+01:00 network disconnect 7e6404027e1ec38230c4dc35f40079c6f6366cd61d73a472517e39f598cebab1 (container=6167a0dbc036143fcd9d3b2783f6d507fbd72900622f8929a5424dee9264e9f5, name=bridge, type=bridge)
2024-01-22T21:08:40.227050098+01:00 container die 6167a0dbc036143fcd9d3b2783f6d507fbd72900622f8929a5424dee9264e9f5 (execDuration=0, exitCode=0, image=ubuntu, name=loving_jennings, org.opencontainers.image.ref.name=ubuntu, org.opencontainers.image.version=22.04)
```