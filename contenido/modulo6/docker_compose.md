# El fichero compose.yaml

En el fichero `compose.yaml` vamos a definir un escenario multicontenedor usando el formato YAML. **La instrucción `docker compose` se debe ejecutar en el directorio donde se encuentra ese fichero**. Por lo tanto tenderemos un directorio con un fichero `compose.yaml` para cada una de las aplicaciones que queremos desplegar. 

Puedes encontrar los ficheros necesarios para este ejemplo en el [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_docker_ow).

Veamos un fichero de ejemplo `compose.yaml` donde se define el escenario para desplegar la aplicación [Let's Chat](https://github.com/sdelements/lets-chat) que es un chat escrito en NodeJS, que guarda su información en una base de datos MongoDB:

```yaml
version: '3.1'
name: 'escenario_letschat'
services:
  app:
    container_name: letschat
    image: sdelements/lets-chat
    restart: always
    environment:
      LCB_DATABASE_URI: mongodb://mongo/letschat
    ports:
      - 80:8080
    depends_on:
      - db
  db:
    container_name: mongo
    image: mongo:4
    restart: always
    volumes:
      - mongo:/data/db
volumes:
  mongo:
```

## Estructura del fichero compose.yaml

* El parámetro `version` indica el formato de fichero Docker Compose que se está utilizando. Se usa sólo a nivel informativo, no es necesario indicarlo.
* Podemos indicar un nombra al escenario con el parámetro `name`. Si no se indica el nombre del proyecto será el nombre del directorio donde se encuentra el fichero `compose.yaml`.
* Es escenario está formado por `services`. Cada uno ello representa un contenedor.
* Podemos declarar volúmenes con el parámetros `volumes` y declarar redes con el parámetro `network`.

## Definición de los servicios

Como hemos comentado con el parámetro `services` se define la lista de contenedores y su configuración que formarán parte del escenario. Veamos algunos aspectos de la configuración de un servicio:

* Cada servicio tiene un nombre, en nuestro ejemplo se llaman `app` y `db`.
* En la definición del contenedor indicamos un nombre (parámetro `container_name`), la imagen (con el parámetro `image`), las variables de entorno (parámetro `environment`), el mapeo de puertos (parámetro `ports`), los volúmenes (parámetro `volume`),... Iremos estudiando los distintos parámetros cuando veamos más ejemplos.
* `restart: always`: Indicamos la política de reinicio del contenedor si por cualquier circunstancia se para. [Más información](https://docs.docker.com/compose/compose-file/compose-file-v3/#restart).
* `depend on`: Indica la dependencia entre contenedores. No se va a iniciar un contenedor hasta que otro este funcionando. [Más información](https://docs.docker.com/compose/compose-file/compose-file-v3/#depends_on).
* Puedes encontrar todos los parámetros que podemos definir en la [documentación oficial](https://docs.docker.com/compose/compose-file/compose-file-v3/).

## Introducción a las redes con Docker Compose

Cuando creamos un escenario con `docker compose` se crea una **nueva red bridge definida por el usuario** donde se conectan los contenedores, por lo tanto, obtenemos resolución por DNS que resuelve tanto el nombre del contenedor (por ejemplo, `mongo`) como el nombre del servicio (por ejemplo, `db`).

