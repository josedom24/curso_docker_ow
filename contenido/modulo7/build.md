# Creación de imágenes a partir de un Dockerfile

Veamos como podemos automatizar la creación de imágenes Docker, usando un fichero `Dockerfile` y el comando `docker build`. Puedes encontrar los ficheros necesarios en el [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_docker_ow).

1. Vamos a crear un directorio (a este directorio se le llama **contexto**) donde vamos a crear un fichero `Dockerfile` y un fichero `index.html`:

    ```bash
    cd build
    ~/build$ ls
    Dockerfile  index.html
    ```
    El contenido de `Dockerfile` es:

    ```Dockerfile
    # syntax=docker/dockerfile:1
    FROM debian:stable-slim
    RUN apt-get update  && apt-get install -y  apache2 
    WORKDIR /var/www/html
    COPY index.html .
    CMD apache2ctl -D FOREGROUND
    ```

2. Para crear la imagen uso el comando `docker build`, indicando el nombre de la nueva imagen (opción `-t`) y el directorio **contexto**.

    ```bash
    $ docker build -t josedom24/myapache2:v2 .
    ...
    ```
    **Nota:** Pongo como directorio el `.` porque estoy ejecutando esta instrucción dentro del directorio donde está el fichero `Dockerfile`.

    Una vez terminado, podremos comprobar que hemos generado una nueva imagen:

    ```bash
    $ docker images
    REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
    josedom24/myapache2       v2                  3bd28de7ae88        43 seconds ago      195MB
    ...
    ```
3. En este caso al crear el contenedor a partir de esta imagen no hay que indicar el proceso que se va a ejecutar, porque ya se ha indicando en el fichero `Dockerfile`, con el parámetro `CMD`:

```bash
$ docker run -d -p 8080:80 --name servidor_web josedom24/myapache2:v2 
```            

Si queremos ver los distintos pasos que hemos ejecutado para construir la imagen, podemos ejecutar la siguiente instrucción:

```bash
$ docker history josedom24/myapache2:v2
IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
b4836c1e7b7f   41 seconds ago   CMD ["/bin/sh" "-c" "apache2ctl -D FOREGROUN…   0B        buildkit.dockerfile.v0
<missing>      41 seconds ago   COPY index.html . # buildkit                    22B       buildkit.dockerfile.v0
<missing>      42 seconds ago   WORKDIR /var/www/html                           0B        buildkit.dockerfile.v0
<missing>      44 seconds ago   RUN /bin/sh -c apt-get update  && apt-get in…   131MB     buildkit.dockerfile.v0
<missing>      9 days ago       /bin/sh -c #(nop)  CMD ["bash"]                 0B        
<missing>      9 days ago       /bin/sh -c #(nop) ADD file:17e64d3a682fd256f…   74.8MB
```

Donde vemos los pasos que hemos ejecutado en la construcción de la imagen.

## Uso de la caché en la construcción de imágenes Docker

Como hemos indicado anteriormente, durante la construcción de una imagen Docker, se van guardando en caché las capas intermedias que se van generando. Vamos a ver qué ocurre si volvemos a construir la imagen después de alguna modificación:

1. Si modificamos el fichero `index.html` y volvemos a construir la imagen:

    ```bash
    $ docker build -t josedom24/myapache2:v3 .
    ...
    ```
    
    La construcción será muy rápida, ya que las imágenes intermedias que se van generando no se vuelven a generar porque están guardadas en caché. Sólo se ejecuta la instrucción donde copiamos el fichero que hemos modificado.

2. Si modificamos por ejemplo la instrucción donde se ejecuta la instalación del servidor web y ponemos por ejemplo:

    ```
    ...
    RUN apt-get update  && apt-get install -y  apache2 git
    ...
    ```

    Ahora esa primera instrucción ha cambiado, por lo que se generará una nueva capa distinta a la guardada en caché y todas las demás instrucciones se tendrán que volver a ejecutar.

Si usamos el parámetro `--no-cache` en `docker build` haríamos la construcción de una imagen sin usar las capas cacheadas generadas en construcciones anteriores.



