# Asociando almacenamiento a los contenedores: bind mount

## Creación de contenedores con bind mount

En este caso, vamos a crear un directorio en el sistema de archivos del Host Docker, donde vamos a crear un fichero `index.html`:

```bash
$ mkdir web
$ cd web
/web$ echo "<h1>Hola</h1>" > index.html
```

Y podemos montar ese directorio en un contenedor, en este caso usamos la opción `-v`:

```bash
$ docker run -d --name my-apache-app -v /home/usuario/web:/usr/local/apache2/htdocs -p 8080:80 httpd:2.4
```

Podemos comprobar en la información del contenedor los puntos de montaje que tiene configurado:

```bash
$ docker inspect --format='{{json .Mounts}}' my-apache-app 
[{"Type":"bind","Source":"/home/usuario/web","Destination":"/usr/local/apache2/htdocs","Mode":"","RW":true,"Propagation":"rprivate"}]
```

Y comprobamos que realmente estamos sirviendo el fichero que tenemos en el directorio que hemos creado.

```bash
$ curl http://localhost:8080
<h1>Hola</h1>
```

Eliminamos el contenedor y volvemos a crear otro con el directorio montado, ahora usando la opción `--mount`:

```bash
$ docker rm -f my-apache-app 
my-apache-app

$ docker run -d --name my-apache-app --mount type=bind,src=/home/usuario/web,dst=/usr/local/apache2/htdocs -p 8080:80 httpd:2.4

$ curl http://localhost:8080
<h1>Hola</h1>
```

Además, podemos comprobar que al modificar el contenido del fichero se modificará en el contenedor:

```bash
$ echo "<h1>Adios</h1>" > web/index.html 
$ curl http://localhost:8080
<h1>Adios</h1>
```

Por último, indicar que si nuestro directorio origen no existe y hacemos un bind mount con `-v`, se creará, pero lo que tendremos en el contenedor será un directorio vacío. Sin embargo, si hacemos un bind mount con la opción `--mount` nos dará un error.

