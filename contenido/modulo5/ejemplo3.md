# Ejemplo 3: Despliegue de WordPress + MariaDB

Para la instalación de WordPress necesitamos dos contenedores: uno para ejecutar la base de datos MariaDB (imagen `mariadb`) y el servidor web con la aplicación (imagen `wordpress`). Los dos contenedores tienen que estar en la misma red y deben tener acceso por nombre (resolución DNS) ya que de principio no sabemos que dirección IP va a coger cada contenedor. Por lo tanto vamos a crear los contenedores en la misma red:

```bash
$ docker network create red_wp
```

Siguiendo la documentación de la imagen [`mariadb`](https://hub.docker.com/_/mariadb) y la imagen [`wordpress`](https://hub.docker.com/_/wordpress) podemos ejecutar los siguientes comandos para crear los dos contenedores:

```bash
$ docker run -d --name servidor_mariadb \
                --network red_wp \
                -v vol_mariadb:/var/lib/mysql \
                -e MARIADB_DATABASE=bd_wp \
                -e MARIADB_USER=user_wp \
                -e MARIADB_PASSWORD=asdasd \
                -e MARIADB_ROOT_PASSWORD=asdasd \
                mariadb
                
$ docker run -d --name servidor_wp \
                --network red_wp \
                -v vol_wordpress:/var/www/html/ \
                -e WORDPRESS_DB_HOST=servidor_mariadb \
                -e WORDPRESS_DB_USER=user_wp \
                -e WORDPRESS_DB_PASSWORD=asdasd \
                -e WORDPRESS_DB_NAME=bd_wp \
                -p 80:80 \
                wordpress

$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
5b2c5a82a524        wordpress           "docker-entrypoint.s…"   9 minutes ago       Up 9 minutes        0.0.0.0:80->80/tcp   servidor_wp
f70f22aed3d1        mariadb             "docker-entrypoint.s…"   9 minutes ago       Up 9 minutes        3306/tcp             servidor_mariadb
```

Algunas observaciones:

* El contenedor `servidor_mariadb` **ejecuta un script `docker-entrypoint.sh`** que es el encargado, a partir de las variables de entorno, de configurar la base de datos: crea usuario, crea base de datos, cambia la contraseña del usuario root,... y termina ejecutando el servidor MariaDB.
* Del mismo modo el contenedor `servidor_wp` **ejecuta un script `docker-entrypoint.sh`**, que entre otras cosas, a partir de las variables de entorno, ha creado el fichero `wp-config.php` de WordPress, por lo que durante la instalación no te ha pedido las credenciales de la base de datos.
* Si te das cuenta la **variable de entorno** `WORDPRESS_DB_HOST` la hemos inicializado al nombre del servidor de base de datos. Como están conectada a la misma red bridge definida por el usuario, el contenedor WordPress al intentar acceder al nombre `servidor_mariadb` estará accediendo al contenedor de la base de datos.
* Vamos a **acceder desde el exterior** al servidor web, por lo que hemos mapeado los puertos con la opción `-p`. Sin embargo, en el contenedor de la base de datos no es necesario mapear los puertos porque no vamos a acceder desde el exterior a ese contenedor. Sin embargo, el contenedor `servidor_wp` puede acceder al puerto 3306/tcp del `servidor_mariadb` sin problemas ya que están conectados a la misma red.
* Hemos **usado un volumen** `vol_mariadb` para hacer persistente el contenedor de MariaDB, montándolo en el directorio `/var/lib/mysql.`. Del mismo modo, hemos usado el volumen `vol_wordpress` para guardar la información de WordPress, montándolo en el directorio *DocumentRoot* `var/www/html`.

![wordpress](img/wp.png)