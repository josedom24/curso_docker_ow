# Gestión de contenedores Docker

## Ciclo de vida de los contenedores

Para ver un ejemplo de los comandos que gestionan el ciclo de vida de un contenedor vamos a ejecutar un contenedor demonio que va escribiendo la hora cada segundo, para ver la salida visualizamos sus logs:

```bash
$ docker run -d --name hora-container ubuntu bash -c 'while true; do echo $(date +"%T"); sleep 1; done'
$ docker logs -f hora-container
```
En otra terminal vamos ejecutando los comandos que nos permiten controlar su ciclo de vida:

* `docker start`: Inicia la ejecución de un contenedor que está parado.
* `docker stop`: Detiene la ejecución de un contenedor en ejecución.
* `docker restart`: Para y vuelve a iniciar la ejecución de un contenedor.
* `docker pause`: Pausa la ejecución de un contenedor.
* `docker unpause`: Continúa la ejecución de un contenedor que estaba pausado..


## Ejecución de comandos en contenedores

Si tenemos un contenedor que está iniciado, podemos ejecutar comandos en él con `docker exec`. En esta ocasión vamos a crear un contenedor que hace algo parecido al anterior, pero en este caso guarda la hora en un fichero cada segundo:

```bash
$ docker run -d --name hora-container2 ubuntu bash -c 'while true; do date +"%T" >> hora.txt; sleep 1; done'
$ docker exec hora-container2 ls
...
hora.txt
...
$  docker exec hora-container2 cat hora.txt
```

## Copiar ficheros en contenedores

Con el comando `docker cp` podemos copiar ficheros a o desde un contenedor. Por ejemplo, si tengo un fichero en mi equipo lo puedo copiar al contenedor:

```bash
$ echo "Curso Docker">docker.txt
$ docker cp docker.txt hora-container2:/tmp
Successfully copied 2.05kB to hora-container2:/tmp
```

Podemos comprobar que el fichero existe en el contenedor:

```bash
$ docker exec hora-container2 cat /tmp/docker.txt
Curso Docker
```

Evidentemente, también podemos copiar ficheros desde el contenedor a nuestro equipo:

```bash
docker cp hora-container2:hora.txt .
Successfully copied 5.63kB to /home/usuario/.
```

## Visualizar procesos que se ejecutan en un contenedor

Podemos visualizar los procesos que se están ejecutando en un contenedor con el comando `docker top`:

```bash
$ docker top hora-container2
```

## Obtener información de los contenedores

Para obtener información de cualquier objeto Docker vamos a usar el subcomando `inspect`. En el caso de los contenedores, ejecutamos:

```bash
$ docker inspect hora-container2
```

Nos muestra mucha información en formato JSON (JavaScript Object Notation) y nos da datos sobre aspectos como:

* El id del contenedor.
* Los puertos abiertos y sus redirecciones.
* Los bind mounts y volúmenes usados.
* El tamaño del contenedor (si ejecutamos `docker inspect -s`).
* La configuración de red del contenedor.
* El comando que se esta ejecutando en el contenedor.
* El valor de las variables de entorno.
* Y muchas más cosas....

Como nos devuelve mucha información podemos filtrar los campos que nos interesan, por ejemplo:

El identificado del contenedor:

```bash
$ docker inspect --format='{{.Id}}' hora-container2
```

El nombre de la imagen que hemos usado para crear el contenedor:

```bash
$ docker inspect --format='{{.Config.Image}}' hora-container2
```

El valor de las variables de entorno definidas en el contenedor:

```bash
$ docker container inspect -f '{{range .Config.Env}}{{println .}}{{end}}' hora-container2
```

El comando que hemos ejecutado en el contenedor:

```bash
$ docker inspect --format='{{range .Config.Cmd}}{{println .}}{{end}}' hora-container2
```

La dirección IP que tiene el contenedor:

```bash
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' hora-container2
```







