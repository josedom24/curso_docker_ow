# Ejemplo 1: Despliegue de la aplicación Guestbook

En este ejemplo vamos a desplegar una aplicación web que requiere de dos servicios (servicio web y servicio de base de datos) para su ejecución. La aplicación se llama Guestbook y necesita los dos siguientes servicios:

* La **aplicación Guestbook** es una aplicación web desarrollada en Python que es servida en el **puerto 5000/tcp**. Utilizaremos la imagen `iesgn/guestbook` para crear el contenedor.
* Esta aplicación guarda la información en una **base de datos no relacional redis**, que utiliza el **puerto 6379/tcp** para conectarnos. Usaremos la imagen `redis` para la creación del contenedor.

La aplicación Guestbook por defecto utiliza el nombre `redis` para conectarse a la base de datos, por lo tanto debemos nombrar al contenedor redis con ese nombre para que tengamos una resolución de nombres adecuada. A continuación veremos cómo podemos configurar la aplicación Guestbook (usando una variable de entorno)  para que se conecte a un contenedor redis con otro nombre.

Los dos contenedores tienen que estar conectados en la misma red bridge definida por el usuario y deben tener acceso por nombres (resolución DNS) ya que de principio no sabemos que dirección IP va a tomar cada contenedor. Por lo tanto vamos a crear los contenedores en la misma red:

```bash
$ docker network create red_guestbook
```

Para ejecutar los contenedores:

```bash
$ docker run -d --name redis --network red_guestbook -v /opt/redis:/data redis redis-server --appendonly yes

$ docker run -d -p 80:5000 --name guestbook --network red_guestbook iesgn/guestbook
```

Algunas observaciones:

* No es necesario mapear el puerto del contenedor de la base de datos redis, ya que no vamos a acceder desde el exterior. Sin embargo la aplicación Guestbook va a poder acceder a la base de datos porque están conectado a la misma red.
* Al nombrar al contenedor de la base de datos con `redis` se crea una entrada en el DNS que resuelve ese nombre con la dirección IP del contenedor. Como hemos indicado, por defecto, la aplicación Guestbook usa ese nombre para acceder.
* Para conseguir la persistencia de datos en el contenedor de la base de datos redis, montamos un bind mount (también podríamos haber usado un volumen Docker) en el directorio `/data` del contenedor. Además ejecutamos el comando `redis-server --appendonly yes` para que se guarden los datos de la base de datos en ese directorio.

Suponiendo que la dirección IP del Host Docker es la `192.168.121.54` podríamos acceder a la aplicación mediante un navegado web:

![ ](img/guestbook.png)

## Configuración de la aplicación Guestbook

Como hemos indicado anteriormente, en la creación de la imagen `iesgn/guestbook` se ha creado una variable de entorno (llamada `REDIS_SERVER`) donde se configura el nombre del servidor de base de datos redis al que se accede, por defecto el valor de esta variable es `redis`. Por lo tanto, es necesario que el contenedor de la base de datos tenga el nombre `redis` para que el contenedor de Guestbook pueda conectar a la base de datos.

Si creamos un contenedor redis con otro nombre, por ejemplo:

```bash
$ docker run -d --name contenedor_redis --network red_guestbook -v /opt/redis:/data redis redis-server --appendonly yes
```

Tendremos que configurar la aplicación Guestbook para que acceda a la base de datos redis usando como nombre `contenedor_redis`, por lo tanto en la creación tendremos que definir la variable de entorno `REDIS_SERVER`, para ello ejecutamos:

```bash
$ docker run -d -p 80:5000 --name guestbook -e REDIS_SERVER=contenedor_redis --network red_guestbook iesgn/guestbook
```

