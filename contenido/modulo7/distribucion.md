# Distribución de imágenes

Como hemos comentado, tenemos dos maneas de distribuir nuestras imágenes Docker:

1. Distribuir nuestras imágenes a través de ficheros, utilizando los comandos `docker save` / `docker load`. 
2. Distribuir nuestras imágenes usando un registro de imágenes, por ejemplo Docker Hub, para ello utilizamos los comandos `docker push` / `docker pull`.

## Distribución a partir de un fichero

1. Guardamos la imagen que queremos distribuir en un archivo `.tar` usando el comando `docker save`:

    ```bash    
    $ docker save josedom24/myapache2:v1 > myapache2.tar
    ```

2. Distribuimos el fichero `.tar`.

3. Si me llega un fichero `.tar` puedo añadir la imagen a mi repositorio local:

    ```bash
    $ docker load -i myapache2.tar          
    Loaded image: josedom24/myapache2:v1
    ```

## Distribución usando Docker Hub

Necesitamos estar registrados en Docker Hub. Durante el registro indicaremos un nombre de usuario y una contraseña con las que podremos acceder al registro.

Los pasos para distribuir nuestra imagen usando Docker Hub, serían:

1. Accedemos a Docker Hub usando el comando `docker login`.

    ```bash
    $ docker login 
    Login with your Docker ID to push and pull images from Docker Hub...
    Username: ...
    Password: ...
    ...
    Login Succeeded
    ```

2. Subimos la nueva imagen a Docker Hub mediante `docker push`. Recuerda que el nombre de la imagen tiene que tener como primera parte el nombre del usuario de Docker Hub que estamos usando.

    ```bash
    $ docker push josedom24/myapache2:v2
    The push refers to repository [docker.io/josedom24/myapache2:v2]
    ...
    ```

3. Podemos bajar la imagen en otro servidor usando `docker pull`.