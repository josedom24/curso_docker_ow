# Gestión de Imágenes

Para crear un contenedor es necesario usar una imagen que tengamos descargada en nuestro registro local. Por lo tanto al ejecutar `docker run` se comprueba si tenemos la versión indicada de la imagen y si no es así, se procede a su descarga desde **Docker Hub**.

Otra manera de descargar una imagen a nuestro registro local, es usando la instrucción `docker image pull` o la siguiente instrucción:

```bash
$ docker pull nginx:stable
```

Para mostrar las imágenes que tenemos en nuestro registro local podemos usar `docker image ls` o la siguiente instrucción:

```bash
$ docker images
```

Si queremos borrar una imagen, usaremos la instrucción `docker image rm` o la siguiente instrucción:

```bash
$ docker rmi nginx:stable
```

**Nota**: No podemos eliminar una imagen si tenemos algún contenedor creado a partir de ella.

Si queremos buscar imágenes de **Docker Hub** desde la línea de comandos, podemos usar la instrucción:

```bash
$ docker search nginx
```

Por último es posible obtener información detallada sobre una imagen. Para ello usaremos la instrucción `docker image inspect` o de forma abreviada:

```bash
$ docker inspect nginx:stable
```

La información más destacable que podemos ver:

* El id y el checksum de la imagen.
* Los puertos que se exponen al crear un contenedor.
* La arquitectura y el sistema operativo de la imagen.
* El tamaño de la imagen.
* Las variables de entorno definidas en la imagen.
* El comando que ejecuta el contenedor que se cree a partir de la imagen.
* Las capas.
* Y muchas más cosas...

Podemos usar filtros, como en el caso de los contenedores. Por ejemplo, para mostrar el identificador de la imagen, ejecutamos

```bash
$ docker inspect --format='{{.Id}}' nginx:stable
```

Consultar los puertos que expone el contenedor que creemos a a partir de esta imagen:

```bash
$ docker inspect --format='{{range $port,$key := .Config.ExposedPorts}}{{$port}}{{end}}' nginx:stable
```

Consultar el sistema operativo y la arquitectura:

```bash
$ docker inspect --format='{{.Os}} {{.Architecture}}' nginx:stable
```

Consultar las variables de entorno definidas en la imagen:

```bash
$ docker inspect --format='{{range .Config.Env}}{{println .}}{{end}}' nginx:stable
```

Y por último, para consultar los identificadores de las capas que forman la imagen:

```bash
$ docker inspect --format='{{range .RootFS.Layers}}{{println .}}{{end}}' nginx:stable
```