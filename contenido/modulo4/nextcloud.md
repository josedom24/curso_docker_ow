# Ejemplo 1: Contenedor NextCloud con almacenamiento persistente

[NextCloud](https://nextcloud.com/es/) en una aplicación escrita en PHP que nos posibilita construir una nube privada para guardar nuestros archivos, además de tener otros servicios como agenda, calendario, ...

Vamos a desplegar un contenedor con NextCloud, para simplificar la instalación vamos hacer uso de una base de datos SQLite. Si estudiamos la documentación de la imagen [`nextcloud`](https://hub.docker.com/_/nextcloud) en Docker Hub, la forma más sencilla de no perder la información es crear un volumen para guardar el directorio `/var/www/html` del contenedor. Vamos a realizar el ejercicio usando volúmenes Docker y bind mount.

## Ejemplo con volúmenes

Creamos un volumen:

```bash
$ docker volume create nextcloud
nextcloud
```

Y creamos el contenedor, guardando el directorio `/var/www/html` del contenedor en el volumen creado, con el parámetro `-v`:

```bash
$ docker run -d -p 80:80  -v nextcloud:/var/www/html --name contenedor_nextcloud nextcloud:28.0.1
```

Comprobamos que podemos acceder, terminamos de configurar la aplicación y una vez operativa subimos un ficheros a la aplicación.

A continuación eliminamos el contenedor y creamos uno nuevo con el mismo volumen, ahora usando el parámetro `--mount`::

```bash
$ docker rm -f contenedor_nextcloud

$ docker run -d -p 80:80  --mount type=volume,src=nextcloud,dst=/var/www/html --name contenedor_nextcloud nextcloud:28.0.1
```
Accediendo de nuevo a la aplicación podemos comprobar que la aplicación sigue configurada y que los ficheros subidos no se han perdido.

## Ejemplo con bind mount

En este caso, vamos a crear un directorio en nuestro ordenador, que es el que vamos a montar en el contenedor:

```bash
mkdir /opt/datos_nextcloud
```

Y creamos el contenedor usando el parámetro `-v`:

```bash
$ docker run -d -p 80:80 -v /opt/datos_nextcloud:/var/www/html --name contenedor_nextcloud nextcloud:28.0.1
```

También podríamos usar el parámetro `--mount`:

```bash
$ docker run -d -p 80:80 --mount type=bind,src=/opt/datos_nextcloud,dst=/var/www/html --name contenedor_nextcloud nextcloud:28.0.1
```

Volvemos a acceder, configuramos la aplicación y subimos algún fichero.

Usando bind mount tenemos acceso al directorio:

```bash
$ cd datos_nextcloud/
~/datos_nextcloud$ ls
3rdparty  COPYING  config       core      custom_apps  index.html  lib  ocm-provider  ocs-provider  remote.php  robots.txt  themes
AUTHORS   apps     console.php  cron.php  data         index.php   occ  ocs           public.php    resources   status.php  version.php
```

Podemos comprobar que al eliminar el contenedor y crearlo de nuevo usando el mismo directorio bind mount, toda la configuración y los ficheros subidos no se han perdido.