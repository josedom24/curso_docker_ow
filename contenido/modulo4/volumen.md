# Asociando almacenamiento a los contenedores: volúmenes Docker

Antes de usar volúmenes Docker en contenedores para hacer persistir sus datos, vamos a estudiar como podemos gestionar los volúmenes.

## Gestionando volúmenes

Algunos comando útiles para trabajar con volúmenes Docker, son los siguientes.

Podemos crear un volumen indicando su nombre:

```bash
$ docker volume create my-vol
```

Para listar los volúmenes que tenemos creados:

```bash
$ docker volume ls
...
local               my-vol
```

Para obtener información de un volumen:

```bash
$ docker volume inspect my-vol
[
    {
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/my-vol/_data",
        "Name": "my-vol",
        "Options": {},
        "Scope": "local"
    }
]
```

Y para eliminar un volumen, ejecutamos:

```bash
$ docker volume rm my-vol
```


## Creación de contenedores con volúmenes

Lo primero que vamos a hacer es crear un volumen Docker:

```bash
$ docker volume create miweb
miweb
```

A continuación, creamos un contenedor con el volumen asociado, usando el parámetro `--mount`. En este ejemplo vamos a montar nuestro volumen en el directorio *DocumentRoot* del servidor Apache que nos ofrece la imagen `httpd:2.4` (en la documentación de la imagen se nos indica que el directorio *DocumentRoot* es `usr/local/apache2/htdocs`).

```bash
$ docker run -d --name my-apache-app --mount type=volume,src=miweb,dst=/usr/local/apache2/htdocs -p 8080:80 httpd:2.4
```

Podemos comprobar en la información del contenedor los puntos de montajes que tiene configurado:

```bash
$ docker inspect --format='{{json .Mounts}}' my-apache-app 
[{"Type":"volume","Name":"miweb","Source":"/var/lib/docker/volumes/miweb/_data","Destination":"/usr/local/apache2/htdocs","Driver":"local","Mode":"z","RW":true,"Propagation":""}]
```

A continuación, creamos un fichero `index.html` en el directorio donde hemos montado el volumen, por lo tanto esta información no se perderá:

```bash
$ docker exec my-apache-app bash -c 'echo "<h1>Hola</h1>" > /usr/local/apache2/htdocs/index.html'
```

Podemos comprobar el acceso al servidor web usando un navegado web o en este caso usando el comando `curl`:

```bash
$ curl http://localhost:8080
<h1>Hola</h1>
```

A continuación borramos el contenedor:

```bash
$ docker rm -f my-apache-app 
my-apache-app
```

Podemos comprobar que el volumen no se ha borrado, ejecutando `docker volume ls`.

Después de borrar el contenedor, volvemos a crear otro contenedor con el mismo volumen asociado, en esta ocasión vamos a usar el parámetro `-v`:

```bash
$ docker run -d --name my-apache-app -v miweb:/usr/local/apache2/htdocs -p 8080:80 httpd:2.4
```

Y podemos comprobar que no no se ha perdido la información (el fichero `index.html`):

```bash
$ curl http://localhost:8080
<h1>Hola</h1>
```

