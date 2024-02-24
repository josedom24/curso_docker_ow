# Ejemplo: Configuración de un contenedor con la imagen MariaDB

En ocasiones es obligatorio el inicializar alguna variable de entorno para que el contenedor pueda ser ejecutado. Si miramos la [documentación](https://hub.docker.com/_/mariadb) en Docker Hub de la imagen `mariadb`, observamos que podemos definir algunas variables de entorno para la creación y configuración del contenedor (por ejemplo: `MARIADB_DATABASE`,`MARIADB_USER`, `MARIADB_PASSWORD`,...). Pero hay una que la tenemos que indicar de forma obligatoria, la contraseña del usuario `root` (`MARIADB_ROOT_PASSWORD`), por lo tanto:

```bash
$ docker run -d --name mimariadb -e MARIADB_ROOT_PASSWORD=my-secret-pw mariadb:10.5
$ $ docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS         PORTS             NAMES
09425481aee6   mariadb     "docker-entrypoint.s…"   22 seconds ago   Up 1 second    3306/tcp          mimariadb
```

Podemos ver que se ha creado una variable de entorno:

```bash
$ docker exec -it mimariadb env
...
MARIADB_ROOT_PASSWORD=my-secret-pw
...
```

Y para acceder podemos ejecutar:

```bash
$ docker exec -it mimariadb bash
root@9c3effd891e3:/# mysql -u root -p"$MARIADB_ROOT_PASSWORD" 
...

MariaDB [(none)]> 
```
Otra forma de hacerlo sería:

```bash
$ docker exec -it mimariadb mysql -u root -p -h 127.0.0.1
Enter password: 
...
MariaDB [(none)]> 
```

## Accediendo a servidor de base de datos desde el exterior

En el ejemplo anterior hemos accedido a la base de datos de dos formas: 

1. Ejecutado un comando `bash` para acceder al contenedor y desde dentro hemos utilizado el cliente de MariaDB para acceder a la base de datos.
2. Ejecutando directamente en el contenedor el cliente de MariaDB.

En esta ocasión vamos a mapear los puertos para acceder desde el exterior a la base de datos:

Lo primero que vamos a hacer es eliminar el contenedor anterior:

```bash 
$ docker rm -f mimariadb
```

Y a continuación vamos a crear otro contenedor, pero en esta ocasión vamos a mapear el puerto 3306/tcp del Host Docker con el puerto 3306/tcp del contenedor:

```bash 
$ docker run -d -p 3306:3306 --name mimariadb -e MARIADB_ROOT_PASSWORD=my-secret-pw mariadb:10.5
```

Comprobamos que los puertos se han mapeado y que el contenedor está ejecutándose:

```bash
$ docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                                       NAMES
8d1857ff1d2f   mariadb     "docker-entrypoint.s…"   14 seconds ago   Up 11 seconds   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp   mimariadb
```

Ahora desde nuestro equipo, donde hemos instalado un cliente de MariaDB (`sudo apt install mariadb-client`), nos conectamos al Host Docker:

```bash
$ mysql -u root -p -h 127.0.0.1
Enter password: 
...
MariaDB [(none)]> 
```