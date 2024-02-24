# Ejemplo: Creando un contenedor con un servidor web

En este ejemplo vamos a crear un contenedor demonio que ejecuta un servidor web Apache, para ello vamos a usar la imagen `httpd:2.4` del registro **Docker Hub** (en este caso hemos indicado el nombre de la imagen y su etiqueta `2.4` que nos indica la versión del servidor web que vamos a usar):

```bash
$ docker run -d --name my-apache-app -p 8080:80 httpd:2.4
```

Hay que tener en cuenta que los contenedores que estamos creando se conectan a una red virtual privada y que toman direccionamiento dinámico. No solemos usar la dirección IP del contenedor para acceder al servicio que nos ofrece. Con la opción `-p` mapeamos un puerto del Host Docker, con el puerto del servicio ofrecido por el contenedor. Si accedemos a la dirección IP del ordenador que tiene instalado Docker al primer puerto indicado, se redireccionará la petición a la dirección IP del contenedor al segundo puerto indicado. **Nunca utilizamos directamente la dirección IP del contenedor para acceder a él**. 

Podemos ver los puertos que están mapeados en un contenedor de dos maneras distintas. Usando el comando `docker port`:

```bash
$ docker port my-apache-app
80/tcp -> 0.0.0.0:8080
80/tcp -> [::]:8080
```

O utilizando el comando `docker inspect` con un filtro:

```bash
docker inspect --format='{{range $p, $conf := .NetworkSettings.Ports}}  {{(index $conf 0).HostPort}} -> {{$p}} {{end}}' my-apache-app
```

Para probarlo accedemos desde un navegador web:

* Si estamos accediendo desde el Host Docker, accederemos a `http://localhost:8080`.
* Si estamos accediendo desde un ordenador remoto, accederemos a la dirección IP del Host Docker y al puerto, por ejemplo,si dirección IP del Host Docker es `192.168.121.54`, accederemos a `http://192.168.121.54:8080`:

![web](img/web.png)

Para acceder al log del contenedor podemos ejecutar:

```bash
$ docker logs my-apache-app
```

## Modificación del contenido servido por el servidor web

Si consultamos la documentación de la imagen [`httpd`](https://hub.docker.com/_/httpd) en el registro Docker Hub, podemos determinar que el servidor web que se ejecuta en el contenedor guarda los ficheros que sirve (directorio *DocumentRoot*) en `/usr/local/apache2/htdocs/`. Vamos a crear un nuevo fichero `index.html` en ese directorio.

Lo podemos hacer de varias formas:

* Accediendo de forma interactiva al contenedor y haciendo la modificación:

    ```bash
    $ docker exec -it my-apache-app bash

    root@cf3cd01a4993:/usr/local/apache2# cd /usr/local/apache2/htdocs/
    root@cf3cd01a4993:/usr/local/apache2/htdocs# echo "<h1>Curso Docker</h1>" > index.html
    root@cf3cd01a4993:/usr/local/apache2/htdocs# exit
    ```

* Ejecutando directamente el comando de creación del fichero `index.html` en el contenedor:

    ```bash
    $ docker exec my-apache-app bash -c 'echo "<h1>Curso Docker</h1>" > /usr/local/apache2/htdocs/index.html'
    ```

* Usando el comando `docker cp` y copiando el fichero `index.html` al contenedor:

    ```bash
    $ echo "<h1>Curso Docker</h1>" > index.html
    $ docker cp index.html  my-apache-app:/usr/local/apache2/htdocs/
    ```
    
Independientemente de cómo hayamos creado el fichero, podemos volver a acceder al servidor web y comprobar que efectivamente hemos cambiado el contenido del `index.html`:

![web](img/web2.png)