# Demostración: Almacenamiento de imágenes y contenedores

## Ejemplo de almacenamiento de imágenes

Se ha creado una imagen que hemos subido a **Docker Hub** llamada `josedom24/servidorweb`. Esta imagen ofrece un servidor web sobre un sistema operativo Debian Linux, con una sencilla página web.

Durante el proceso de generación de la imagen:

* Se ha partido de la imagen `debian:stable-slim` (**Pimera capa**).
* Se ha instalado el servidor web Apache (**Segunda capa**).
* La imagen tiene dos versiones. Las versiones están etiquetadas con las etiquetas **v1** y **v2**. Cada versión tiene un fichero `index.html` diferente. (**Tercera capa**).

Vamos a descargar la primera versión de la imagen (suponemos que no tenemos descargada ninguna imagen en nuestro registro local):

```bash
$ docker pull josedom24/servidorweb:v1
v1: Pulling from josedom24/servidorweb
9532dfcb62dd: Pull complete 
209210f58112: Pull complete 
4f6c4ab344d9: Pull complete 
Digest: sha256:5707a7005d440bf32619e27e37800419b2c52644205da3c6a9edb9d55b8c51de
Status: Downloaded newer image for josedom24/servidorweb:v1
docker.io/josedom24/servidorweb:v1
```
Vemos como se han descargado las 3 capas, y que finalmente tenemos una nueva imagen en nuestro registro local:

```bash
$ docker images
REPOSITORY              TAG       IMAGE ID       CREATED      SIZE
josedom24/servidorweb   v1        d0d75af6b8ec   3 days ago   187MB
```

A continuación descargamos la segunda versión de la imagen:

```bash
$ docker pull josedom24/servidorweb:v2
v2: Pulling from josedom24/servidorweb
9532dfcb62dd: Already exists 
209210f58112: Already exists 
274f48ad3e93: Pull complete 
Digest: sha256:9acd8efeb1f0be80c466a7ebb3772e045ffcb6b12cfbfadaa8b7c4b110bd83ea
Status: Downloaded newer image for josedom24/servidorweb:v2
docker.io/josedom24/servidorweb:v2
```

Podemos observar que las dos primeras capas "ya existen", es decir, ya la tenemos almacenadas en nuestro registro, porque son iguales a las capas de la primera versión de la imagen.

Si visualizamos las imágenes:

```bash
$ docker images
REPOSITORY              TAG       IMAGE ID       CREATED      SIZE
josedom24/servidorweb   v2        f558b3613d2c   3 days ago   187MB
josedom24/servidorweb   v1        d0d75af6b8ec   3 days ago   187MB
```

Podemos pensar que se ha ocupado en el disco duro 187Mb + 187 Mb, pero en realidad el espacio ocupado por las dos primeras capas sólo se guarda en el disco una vez, esas capas se comparten entre las dos versiones de la imagen. Esto lo podemos ver de manera más clara ejecutando el siguiente comando:

```bash
docker system df -v
Images space usage:

REPOSITORY              TAG       IMAGE ID       CREATED      SIZE      SHARED SIZE   UNIQUE SIZE   CONTAINERS
josedom24/servidorweb   v2        f558b3613d2c   3 days ago   187MB     186.8MB       25B           0
josedom24/servidorweb   v1        d0d75af6b8ec   3 days ago   187MB     186.8MB       22B           0
```

De los 187 MB que tienen de tamaño las imágenes, 186,8 MB están compartido (este es el tamaño de las dos primeras capas), por lo tanto el espacio ocupado por cada una de las imágenes corresponde a la tercera capa (el fichero `index.html`) que en este caso es 25B  y 22B.

Por lo tanto, ¿cuánto han ocupado en total estas dos imágenes en el disco duro? Pues sería 186,8MB + 25B + 22B.
El mecanismo de compartir capas entre imágenes hace que se ocupe el menor espacio posible en disco duro, el almacenamiento es muy eficiente.

## Ejemplo de almacenamiento de contenedores

Hemos descargado la imagen `ubuntu` y vemos su tamaño:

```bash
$ docker images
REPOSITORY              TAG       IMAGE ID       CREATED       SIZE
ubuntu                  latest    e34e831650c1   2 weeks ago   77.9MB
```

Si creamos un contenedor interactivo:

```bash
$ docker run -it --name contenedor1 ubuntu
```

Nos salimos, y a continuación visualizamos el tamaño ocupado por el contenedor con la opción `-s` (size) del comando `docker ps`:

```bash
$ docker ps -a -s
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                     PORTS     NAMES         SIZE
679822aff71a   ubuntu    "/bin/bash"   25 seconds ago   Exited (0) 9 seconds ago             contenedor1   5B (virtual 77.9MB)
```

Vemos que el tamaño real del contenedor es 5B, aunque el sistema de archivos del contenedor (tamaño virtual) es de 77,9MB. Este tamaño es el de la imagen `ubuntu` cuyo sistema de ficheros se comparte con el contenedor.

Si a continuación volvemos a acceder al contenedor y creamos un fichero:

```bash
$ docker start contenedor1
contenedor1
$ docker attach contenedor1
root@a2d1ce6990d8:/# echo "00000000000000000">file.txt
```

Y volvemos a ver el tamaño, vemos que ha crecido con la creación del fichero:

```bash
$ docker ps -a -s
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS                     PORTS     NAMES         SIZE
679822aff71a   ubuntu    "/bin/bash"   2 minutes ago   Exited (0) 5 seconds ago             contenedor1   62B (virtual 77.9MB)
```

El tamaño del contenedor ha aumentado, en realidad la **Capa del Contenedor** de lectura y escritura ha aumentado ya que hemos creado un nuevo fichero.

Por todo lo que hemos explicado, ahora se entiende  que **no podemos eliminar una imagen cuando tenemos contenedores creados a a partir de ella**.

```bash
$ docker rmi ubuntu
Error response from daemon: conflict: unable to remove repository reference "ubuntu" (must force) - container 679822aff71a is using its referenced image e34e831650c1
```

