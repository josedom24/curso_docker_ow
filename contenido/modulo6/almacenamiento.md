# Almacenamiento con Docker Compose

## Definiendo volúmenes Docker con Docker Compose

Además de definir los servicios (parámetro `services`) en el fichero `compose.yaml`, podemos definir los volúmenes que vamos a necesitar en nuestra infraestructura. Además, como hemos visto, podremos indicar que volumen va a utilizar cada contenedor.

Veamos un ejemplo, puedes encontrar el fichero en el [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_docker_ow).

El contenido del fichro `compose.yaml` es:

```yaml
version: '3.1'
services:
  db:
    container_name: contenedor_mariadb
    image: mariadb
    restart: always
    environment:
      MARIADB_ROOT_PASSWORD: asdasd
    volumes:
      - mariadb_data:/var/lib/mysql
volumes:
    mariadb_data:
```

Y podemos iniciar el escenario:

```bash
$ docker compose up -d
[+] Running 3/3
 ✔ Network mariadb_default        Created                                         0.1s 
 ✔ Volume "mariadb_mariadb_data"  Created                                         0.0s 
 ✔ Container contenedor_mariadb   Started                                         0.5s

$ docker compose ps
NAME                 IMAGE     COMMAND                           SERVICE   CREATED              STATUS              PORTS
contenedor_mariadb   mariadb   "docker-entrypoint.sh mariadbd"   db        About a minute ago   Up About a minute   3306/tcp
```

Y comprobamos que se ha creado un nuevo volumen:

```bash
$ docker volume ls
DRIVER    VOLUME NAME
local     mariadb_mariadb_data
...
```

En la definición del servicio `db` hemos indicado que el contenedor montará el volumen en un directorio determinado con el parámetro `volumes`. Podemos comprobar que efectivamente se ha realizado el montaje:

```bash
$ docker inspect -f '{{json .Mounts}}' contenedor_mariadb
[{"Type":"volume","Name":"mariadb_mariadb_data","Source":"/var/lib/docker/volumes/mariadb_mariadb_data/_data","Destination":"/var/lib/mysql","Driver":"local","Mode":"z","RW":true,"Propagation":""}]
```

Recuerda que si necesitas iniciar el escenario desde 0, debes eliminar el volumen:

```bash
$ docker compose down -v
[+] Running 3/3
 ✔ Container contenedor_mariadb  Removed                                          0.8s 
 ✔ Volume mariadb_mariadb_data   Removed                                          0.1s 
 ✔ Network mariadb_default       Removed                                          0.1s
```

## Utilización de bind mount con Docker Compose

De forma similar podemos indicar que un contenedor va a utilizar bind mount como almacenamiento. En este caso sería:

```yaml
version: '3.1'
services:
  db:
    container_name: contenedor_mariadb
    image: mariadb
    restart: always
    environment:
      MARIADB_ROOT_PASSWORD: asdasd
    volumes:
      - ./data:/var/lib/mysql
```

Y después de iniciar el escenario podemos ver cómo se ha creado el directorio `data`:

```bash
$ cd data/
/data$ ls
aria_log.00000001  aria_log_control  ibdata1  ib_logfile0  ibtmp1  mysql
```

Hay que tener en cuenta que si usamos bind mount, el comando `docker compose down -v` no eliminará el directorio donde se guardan los datos, en este caso `./data`.