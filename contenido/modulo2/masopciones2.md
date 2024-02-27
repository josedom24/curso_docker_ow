# Más opciones en la ejecución de contenedores (2ª parte)

## Ejecutando un contenedor demonio

En esta ocasión hemos utilizado la opción `-d` del comando `docker run`, para que la ejecución del comando en el contenedor se haga en segundo plano, de manera desatendida, sin estar conectada a la entrada y salida estándar.

```bash
$ docker run -d --name contenedor4 ubuntu bash -c "while true; do echo hello world; sleep 1; done"
7b6c3b1c0d650445b35a1107ac54610b65a03eda7e4b730ae33bf240982bba08
```

> NOTA: En la instrucción `docker run` hemos ejecutado el comando con `bash -c` que nos permite ejecutar uno o más comandos en el contenedor de forma más compleja (por ejemplo, indicando ficheros dentro del contenedor).

Comprobamos que el contenedor se está ejecutando:

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
c6b1761c8831   ubuntu    "bash -c 'while true…"   5 seconds ago   Up 2 seconds             contenedor4
```

Podemos visualizar los logs del contenedor, ejecutando el siguiente comando:

```bash
$ docker logs contenedor4
```

Con la opción `logs -f` seguimos visualizando los logs en tiempo real.

Por último podemos parar el contenedor y borrarlo con las siguientes instrucciones:

```bash
$ docker stop contenedor4
$ docker rm contenedor4
```

Hay que tener en cuenta que un contenedor que esta ejecutándose no puede ser eliminado. Tendríamos que parar el contenedor y posteriormente borrarlo. Otra opción es borrarlo a la fuerza:

```bash
$ docker rm -f contenedor4
```

## Configuración de contenedores con variables de entorno

Más adelante veremos que al crear un contenedor que necesita alguna configuración específica, lo que vamos a hacer es crear variables de entorno en el contenedor, para que el proceso que inicializa el contenedor pueda realizar dicha configuración.

Para crear una variable de entorno al crear un contenedor usamos el flag `-e` o `--env`:

```bash
$ docker run -it --name contenedor5 -e USUARIO=prueba ubuntu bash
root@91e81200c633:/# echo $USUARIO
prueba
```
