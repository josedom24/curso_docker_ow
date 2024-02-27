# Ejemplo 2: Contenedor MariaDB con almacenamiento persistente

Si estudiamos la documentación de la imagen [`mariadb`](https://hub.docker.com/_/mariadb) en Docker Hub, nos indica que la información que hay que guardar de la base de datos se encuentra en el directorio `/var/lib/mysql`. Por lo tanto si queremos crear una contenedor de MariaDB donde no se pierda los datos de la base de datos, debemos ejecutar:

```bash
$ docker run --name some-mariadb -v /opt/mariadb:/var/lib/mysql -e MARIADB_ROOT_PASSWORD=my-secret-pw -d mariadb:10.5
```
Es decir se va a crear un directorio `/opt/mariadb` en el Host Docker, donde se va a guardar la información de la base de datos. Si tenemos que crear de nuevo el contenedor indicaremos ese directorio como bind mount y volveremos a tener accesible la información.

```bash
$ cd /opt/mariadb
opt/mariadb$ ls
aria_log.00000001  aria_log_control  ib_buffer_pool  ib_logfile0  ibdata1  ibtmp1  multi-master.info  mysql  performance_schema

$ docker exec -it some-mariadb bash -c 'mysql -u root -p$MARIADB_ROOT_PASSWORD'
...
MariaDB [(none)]> create database prueba;
MariaDB [(none)]> quit

$ docker rm -f some-mariadb 
some-mariadb

$ docker run --name some-mariadb --mount type=bind,src=/opt/mariadb,dst=/var/lib/mysql -e MARIADB_ROOT_PASSWORD=my-secret-pw -d mariadb:10.5

$ docker exec -it some-mariadb bash -c 'mysql -u root -p$MARIADB_ROOT_PASSWORD'
...
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| prueba             |
+--------------------+
4 rows in set (0.003 sec)
```

Para terminar indicar que aunque este ejercicio lo hemos realizado usando bind mount, también podríamos usar volúmenes Docker. En este caso la instrucción de creación del contenedor sería la siguiente:

```bash
$ docker run --name some-mariadb --mount type=volume,src=vol_mariadb,dst=/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mariadb:10.5
```

Recuerda que con el parámetro `--mount` se crea el volumen indicado si no estaba creado, sin embargo nos da un error si al usar bind mount no existe el directorio que indicamos en el parámetro `src`.

