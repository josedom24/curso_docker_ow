# Más opciones en la ejecución de contenedores

Hemos usado el comando `docker run` para crear y ejecutar contenedores. Este comando tiene muchas opciones veamos algunas de ellas:

## Nombrar los contenedores

A la hora de la creación del contenedor podemos ponerle un nombre (usando ela opción `--name`) y también , aunque no se suele usar podemos indicar el hostname (con al opción `-h` o `--hostname`). Veamos un ejemplo:

```bash
$ docker run --name contenedor1 -h contendor_ubuntu ubuntu hostname
contendor_ubuntu
```

Hemos comprobado que el hostname lo hemos configurado, y veamos que el nombre del contenedor también lo hemos configurado:

```bash
$ docker ps -a
CONTAINER ID   IMAGE     COMMAND      CREATED          STATUS                      PORTS     NAMES
cea2a22ac6aa   ubuntu    "hostname"   56 seconds ago   Exited (0) 54 seconds ago             contenedor1
```

## Ejecutando un contenedor interactivo

En este caso usamos la opción `-i` para abrir una sesión interactiva, `-t` nos permite crear un pseudo-terminal que nos va a permitir interaccionar con el contenedor. El comando que vamos a ejecutar es `bash` para que podamos acceder al terminal:

```bash
$ docker run -it --name contenedor2 -h cont1 ubuntu bash 
root@cont1:/#
```

El contenedor se para cuando salimos de él. Para volver a conectarnos a él:

```bash
$ docker start contendor2
contendor1
$ docker attach contendor2
root@cont1:/#
```

Con `docker attach` nos conectamos a la entrada estándar y a la salida estándar y de error de un contenedor en ejecución, conectándonos a su terminal.

En realidad, todas las imágenes tienen definidas un proceso que se ejecuta, en concreto la imagen `ubuntu` ( y en general todas las imágenes que corresponden al sistemas operativo) tiene definida por defecto el proceso `bash`, por lo que podríamos haber ejecutado:

```bash
$ docker run -it --name contenedor2 ubuntu
```

## Eliminación automática de un contenedor 

Como hemos visto hasta ahora cuando un contenedor termina de ejecutar el comando indicado, se para. Si queremos que cuando finalice de ejecutar el comando el contenedor se borre usamos la opción `--rm`. Por ejemplo:

```bash
docker run -it --rm --name contenedor3 ubuntu top
```

Cuando salgamos de ejecutar el comando `top` se borrará el contenedor. Puedes ejecutar un `docker ps -a` para comprobarlo.

## Ejecutando un contenedor demonio

En esta ocasión hemos utilizado la opción `-d` del comando `docker run`, para que la ejecución del comando en el contenedor se haga en segundo plano. DEmanera desatendida, sin estar conectada a la entrada y salida estándar.

```bash
$ docker run -d --name contenedor4 ubuntu bash -c "while true; do echo hello world; sleep 1; done"
7b6c3b1c0d650445b35a1107ac54610b65a03eda7e4b730ae33bf240982bba08
```

> NOTA: En la instrucción `docker run` hemos ejecutado el comando con `bash -c` que nos permite ejecutar uno o mas comandos en el contenedor de forma más compleja (por ejemplo, indicando ficheros dentro del contenedor).

Comprobamos que el contenedor se está ejecutando:

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
c6b1761c8831   ubuntu    "bash -c 'while true…"   5 seconds ago   Up 2 seconds             contenedor4
```

Podemos comprobar lo que está ejecutando el contenedor, ejecutando el siguiente comando:

```bash
$ docker logs contenedor4
```

Con la opción `logs -f` seguimos visualizando los logs en tiempo real.

Por último podemos parar el contenedor y borrarlo con las siguientes instrucciones:

```bash
$ docker stop contenedor4
$ docker rm contenedor4
```

Hay que tener en cuenta que un contenedor que esta ejecutándose no puede ser eliminado. Tendríamos que parar el contenedor y posteriormente borrarlo. Otra opción es borrarlo a la fuerza:

```bash
$ docker rm -f contenedor4
```