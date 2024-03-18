# Más opciones en la ejecución de contenedores (1ª parte)

Hemos usado el comando `docker run` para crear y ejecutar contenedores. Este comando tiene muchas opciones, veamos algunas de ellas:

## Nombrar los contenedores

A la hora de la creación del contenedor podemos ponerle un nombre (usando el parámetro `--name`) y también, podemos indicar el hostname (con al opción `-h` o `--hostname`). Veamos un ejemplo:

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

En este caso usamos la opción `-i` para abrir una sesión interactiva, `-t` nos permite crear un pseudo-terminal que nos va a permitir interaccionar con el contenedor. El comando que vamos a ejecutar en el contenedor es `bash` para que podamos acceder al terminal:

```bash
$ docker run -it --name contenedor2 -h cont1 ubuntu bash 
root@cont1:/#
```

El contenedor se para cuando salimos de él. Para volver a conectarnos a él:

```bash
$ docker start contenedor2
contendor2
$ docker attach contenedor2
root@cont1:/#
```

Con `docker attach` nos conectamos a la entrada estándar y a la salida estándar y de error de un contenedor en ejecución, conectándonos a su terminal.

En realidad, todas las imágenes tienen definidas un proceso que se ejecuta por defecto si no se indica de manera explicita cuando creamos el contenedor. En concreto, la imagen `ubuntu` ( y en general todas las imágenes que corresponden al sistemas operativos) tiene definida por defecto el proceso `bash`, por lo que podríamos haber ejecutado:

```bash
$ docker run -it --name contenedor2 ubuntu
```

## Eliminación automática de un contenedor 

Como hemos visto hasta ahora cuando un contenedor termina de ejecutar el comando indicado, se para. Si queremos que cuando finalice la ejecución del contenedor se borre, usaremos la opción `--rm`. Por ejemplo:

```bash
docker run -it --rm --name contenedor3 ubuntu top
```

Cuando salgamos de ejecutar el comando `top` se borrará el contenedor. Puedes ejecutar un `docker ps -a` para comprobarlo.

