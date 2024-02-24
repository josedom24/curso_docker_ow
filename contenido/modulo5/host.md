# Uso de la red host en Docker

Si utilizamos el tipo de red **host** en un contenedor, la pila de red de ese contenedor no está aislada del Host Docker (el contenedor comparte el espacio de nombres de red del Host Docker), y el contenedor no tiene asignada su propia dirección IP. Por ejemplo, si ejecutas un contenedor que ofrece su servicio en al puerto 80/tcp y utilizas el modo de red del host, la aplicación del contenedor estará disponible en el puerto 80/tcp de la dirección IP del Host Docker.

## Ventajas del uso de la red host

* Para optimizar el rendimiento.
* En situaciones en las que un contenedor necesita manejar un gran rango de puertos.
    Esto se debe a que no requiere traducción de direcciones de red (DNAT), por lo tanto no usaremos la opción `-p` en la creación del contenedor con `docker run`.

El controlador de red **host** sólo funciona en máquinas Linux, y no es compatible con Docker Desktop para Windows o Mac.

## Ejemplo de uso

Vamos a crear un contenedor desde la imagen `nginx` que nos proporciona un servidor web que espera peticiones en el puerto 80/tcp conectado a la red **host**:

```bash
docker run --rm -d --network host --name my_nginx nginx
```
Al listar el contenedor, comprobamos que efectivamente no hemos creado ninguna redirección de puertos:

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS     NAMES
d1764a33c096   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute             my_nginx
```

Podemos comprobar que no tiene asignada ninguna dirección IP:

```bash
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' my_nginx
```

Y en el Host Docker podemos comprobar quien está escuchando en el puerto 80/tcp:

```bash
$ sudo ss -tulpn | grep :80
```

Por último, desde un navegador podemos acceder al puerto 80/tcp del Host Docker para comprobar que funciona. Usando `curl` podemos hacer la prueba desde el mismo Host Docker:

```bash
$ curl http://localhost
```


