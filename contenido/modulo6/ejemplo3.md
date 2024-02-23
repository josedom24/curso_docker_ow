# Ejemplo 3: Despliegue de WordPress + MariaDB

En este ejemplo vamos a desplegar con Docker Compose la aplicación WordPress + MariaDB, que estudiamos en un módulo anterior.

Puedes encontrar el fichero `compose.yaml` en el [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_docker_ow).

## Utilizando volúmenes Docker

Por ejemplo para la ejecución de WordPress persistente con volúmenes Docker podríamos tener un fichero `compose.yaml` con el siguiente contenido:

```yaml
version: '3.1'
services:
  wordpress:
    container_name: servidor_wp
    image: wordpress
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user_wp
      WORDPRESS_DB_PASSWORD: asdasd
      WORDPRESS_DB_NAME: bd_wp
    ports:
      - 80:80
    volumes:
      - wordpress_data:/var/www/html
  db:
    container_name: servidor_mysql
    image: mariadb
    restart: always
    environment:
      MARIADB_DATABASE: bd_wp
      MARIADB_USER: user_wp
      MARIADB_PASSWORD: asdasd
      MARIADB_ROOT_PASSWORD: asdasd
    volumes:
      - mariadb_data:/var/lib/mysql
volumes:
    wordpress_data:
    mariadb_data:
```

Para crear el escenario:

```bash
$ docker compose up -d
[+] Running 5/5
 ✔ Network wordpress_default          Created                                                    0.2s 
 ✔ Volume "wordpress_wordpress_data"  Created                                                    0.0s 
 ✔ Volume "wordpress_mariadb_data"    Created                                                    0.0s 
 ✔ Container servidor_mysql           Started                                                    0.5s 
 ✔ Container servidor_wp              Started                                                    0.5s 
```

Para listar los contenedores:

```bash
$ docker compose ps
NAME             IMAGE       COMMAND                                     SERVICE     CREATED          STATUS          PORTS
servidor_mysql   mariadb     "docker-entrypoint.sh mariadbd"             db          21 seconds ago   Up 19 seconds   3306/tcp
servidor_wp      wordpress   "docker-entrypoint.sh apache2-foreground"   wordpress   21 seconds ago   Up 19 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp
```

Para parar los contenedores:

```bash
$ docker compose stop
[+] Stopping 2/2
 ✔ Container servidor_mysql  Stopped                                                             0.9s 
 ✔ Container servidor_wp     Stopped                                                             1.8s 
```

Para borrar los contenedores:

```bash
$ docker compose rm
? Going to remove servidor_wp, servidor_mysql Yes
[+] Removing 2/0
 ✔ Container servidor_mysql  Removed                                                             0.0s 
 ✔ Container servidor_wp     Removed                                                             0.0s 
```

Para eliminar el escenario (contenedores, red y volúmenes):

```bash
$ docker compose down -v
...
```

## Utilizando bind-mount

Por ejemplo para la ejecución de WordPress persistente con bind mount podríamos tener un fichero `compose.yaml` con el siguiente contenido:

```yaml
version: '3.1'
services:
  wordpress:
    container_name: servidor_wp
    image: wordpress
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user_wp
      WORDPRESS_DB_PASSWORD: asdasd
      WORDPRESS_DB_NAME: bd_wp
    ports:
      - 80:80
    volumes:
      - ./wordpress:/var/www/html
  db:
    container_name: servidor_mysql
    image: mariadb
    restart: always
    environment:
      MARIADB_DATABASE: bd_wp
      MARIADB_USER: user_wp
      MARIADB_PASSWORD: asdasd
      MARIADB_ROOT_PASSWORD: asdasd
    volumes:
      - ./mysql:/var/lib/mysql
```