# Gestión de contenedores Docker

## Ciclo de vida de los contenedores

start, stop, restart, pause, unpause

## Ejecución de comandos en contenedores

exec

## Copiar ficheros en contenedores

## Visualizar procesos que se ejecutan en un contenedor

docker top

## Obtener información de los contenedores

inspect




Si el contenedor se está ejecutando podemos ejecutar comandos en él con el subcomando `exec`:

```bash
$ docker start contendor1
contendor1
$ docker exec contenedor1 ls -al
```

Con la orden `docker restart` reiniciamos el contenedor, lo paramos y lo iniciamos.

Para mostrar información de un contenedor ejecutamos `docker inspect`:

```bash
$ docker inspect contenedor1 
[
    {
        "Id": "178871769ac2fcbc1c73ce378066af01436b52a15894685b7321088468a25db7",
        ...
```

Nos muestra mucha información, está en formato JSON (JavaScript Object Notation) y nos da datos sobre aspectos como:

* El id del contenedor.
* Los puertos abiertos y sus redirecciones
* Los *bind mounts* y volúmenes usados.
* El tamaño del contenedor
* La configuración de red del contenedor.
* El *ENTRYPOINT* que es la instrucción que se ejecuta al crear el contenedor.
* El valor de las variables de entorno.
* Y muchas más cosas....

