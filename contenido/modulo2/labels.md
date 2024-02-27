# Etiquetando los contenedores con Labels

Las etiquetas son un mecanismo para guardar metadatos en los objetos Docker: en contenedores, imágenes, volúmenes, redes, etc. Podemos utilizar etiquetas para organizar los distintos objetos Docker que estemos utilizando.

Una etiqueta es una información del tipo clave-valor, almacenado como una cadena. Puede especificar varias etiquetas para un objeto, pero cada clave debe ser única dentro de un objeto. 

Para etiquetar un contenedor en su creación utilizaremos el parámetro `-l` (`--label`). Vamos a crear varios contenedores con distintas etiquetas:

```bash
$ docker run -l servicio=web -l entorno=desarrollo -l aplicacion=apache --name prueba_web ubuntu
$ docker run -l servicio=bd -l entorno=desarrollo -l aplicacion=mysql --name prueba_bd ubuntu
$ docker run -l servicio=web -l entorno=produccion --name web ubuntu
$ docker run -l servicio=bd -l entorno=produccion --name bd ubuntu

$ docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                      PORTS     NAMES
6b5742b82a07   ubuntu    "/bin/bash"   7 seconds ago    Exited (0) 5 seconds ago              bd
30d6f1a83749   ubuntu    "/bin/bash"   19 seconds ago   Exited (0) 17 seconds ago             web
3d2ebb7ecfde   ubuntu    "/bin/bash"   39 seconds ago   Exited (0) 36 seconds ago             prueba_bd
553675a1457e   ubuntu    "/bin/bash"   52 seconds ago   Exited (0) 50 seconds ago             prueba_web
```

Hay que tener en cuenta que estos contenedores tendrán además de las etiquetas indicadas en su creación, las etiquetas que estén definidas en la imagen que hemos utilizado para su creación.

A la hora de listar los contenedores podemos filtrar por varios criterios, entre ellos podemos usar las etiquetas para hacer el filtro. 

Por ejemplo, mostrar los contenedores que tienen una etiqueta `aplicacion`:

```bash
$ docker ps -a --filter="label=aplicacion"
```

Mostrar los contenedores que tienen la etiqueta `entorno` con el valor `produccion`:

```bash
$ docker ps -a --filter="label=entorno=produccion"
```

Por último, mostrar los contenedores cuyo servicio es web en el entorno de producción:

```bash
$ docker ps -a --filter="label=entorno=produccion" --filter="label=servicio=web"
```

Para terminar este apartado veamos un filtro que nos devuelve las etiquetas usando el comando `docker inspect`, por ejemplo:

```bash
$ docker inspect --format '{{range $key, $value := .Config.Labels}}{{$key}}: {{$value}}{{"\n"}}{{end}}' prueba_web
aplicacion: apache
entorno: desarrollo
org.opencontainers.image.ref.name: ubuntu
org.opencontainers.image.version: 22.04
servicio: web
```

Como observamos, este contenedor tiene 5 etiquetas, las tres que hemos indicado en su creación, y dos definidas en la imagen `ubuntu`.