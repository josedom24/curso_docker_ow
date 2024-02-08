# Uso de la red host en Docker

Si utilizamos el tipo de red **host** en un contenedor, la pila de red de ese contenedor no está aislada del host Docker (el contenedor comparte el espacio de nombres de red del host), y el contenedor no tiene asignada su propia dirección IP. Por ejemplo, si ejecutas un contenedor que se enlaza al puerto 80 y utilizas el modo de red del host, la aplicación del contenedor estará disponible en el puerto 80 de la dirección IP del host.

## Ventajas del uso de la red host

* Para optimizar el rendimiento
* En situaciones en las que un contenedor necesita manejar un gran rango de puertos.
    Esto se debe a que no requiere traducción de direcciones de red (DNAT), por lo tanto no usaremos la opción `-p` en la creación del contenedor con `docker run`.

El controlador de red **host** sólo funciona en hosts Linux, y no es compatible con Docker Desktop para Mac, Docker Desktop para Windows o Docker EE para Windows Server.

## Ejemplo de uso

Vamos a crear un contenedor desde la imagen `nginx` que nos proporciona un servidor web que espera peticiones en el puerto 80/tcp:

```bash
docker run --rm -d --network host --name my_nginx nginx
```
Lista el contenedor y comprobamos que efectivamente no hemos creado ninguna redirección de puertos:

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS     NAMES
d1764a33c096   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute             my_nginx
```

Podemos comprobar que no tiene asignada ninguna dirección IP:

```bash
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' my_nginx
```

Y en el host podemos comprobar quien está escuchando en el puerto 80/tcp:

```bash
$ sudo ss -tulpn | grep :80
```

Por último, desde un navegados acceder al puerto 80 dl host Docker para comprobar que funciona. Usando `curl` podemos hacer la prueba desde el host:

```bash
$ curl http://localhost
```

