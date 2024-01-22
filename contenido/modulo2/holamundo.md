# El "Hola Mundo" de docker

Vamos a crear nuestro primer contenedor, para comprobar que todo está funcionando y vamos a explicar el proceso que se va a realizar para la creación del contenedor.  

```bash
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete 
Digest: sha256:a13ec89cdf897b3e551bd9f89d499db6ff3a7f44c5b9eb8bca40da20eb4ea1fa
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

Pero, ¿qué es lo que está sucediendo al ejecutar esa orden?:

1. El cliente Docker se conecta al demonio Docker y le indica que debe crear un contenedor (`docker run`).
2. Al ser la primera vez que ejecuto un contenedor basado en la imagen `hello-word`, se descarga del registro público llamado dockerHub y se guarda en nuestro registro local.
3. Se crea el contenedor que ejecuta un comando que muestra el mensaje que hemos leído.
4. El mensaje se envía al cliente Docker que nos lo muestra en el terminal.

Si listamos los contenedores que se están ejecutando (`docker ps`):

```bash
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
Comprobamos que este contenedor no se está ejecutando. **Un contenedor ejecuta un proceso y cuando termina la ejecución, el contenedor se para.**

Para ver los contenedores que no se están ejecutando (observa que se ha asignado un nombre aleatorio al contenedor):

```bash
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
372ca4634d53        hello-world         "/hello"            8 minutes ago       Exited (0) 8 minutes ago                       elastic_johnson
```

Como podemos observar el script que ha ejecutado el contenedor se llama `/hello`.

Para eliminar el contenedor podemos identificarlo con su `id`:

```bash
$ docker rm 372ca4634d53
```

o con su nombre:

```bash
$ docker rm elastic_johnson
```

## Creación de contenedores sin ejecutarlos

De manera habitual vamos a usar `docker run` para crear y ejecutar un contenedor. Podríamos también crear un contenedor que no se jecute y posteriormente dar la orden de ejecución. Vamos a observar que la creación de este segundo contenedor será mucho más rápida ya que tenemos la imagen descargada en nuestro registro local. Para crear un contenedor y no iniciar su ejecución utilizaremos el comando `docker create`:

```bash
$ docker create hello-world
```

Podemos ver que el contenedor está creado pero no en ejecución:


```bash
$ docker ps -a
```

Podemos inicar la ejecución de este contenedor usando `docker start -i`. La opción `-a` nos permite conectar a la salida estándar del contenedor y poder ver en nuestro terminal la salida.

```bash
$ docker start -a ...
```
