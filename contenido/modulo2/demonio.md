# Creando un contenedor demonio

En esta ocasión hemos utilizado la opción `-d` del comando `run`, para que la ejecución del comando en el contenedor se haga en segundo plano.

```bash
$ docker run -d --name contenedor2 ubuntu bash -c "while true; do echo hello world; sleep 1; done"
7b6c3b1c0d650445b35a1107ac54610b65a03eda7e4b730ae33bf240982bba08
```

> NOTA: En la instrucción `docker run` hemos ejecutado el comando con `bash -c` que nos permite ejecutar uno o mas comandos en el contenedor de forma más compleja (por ejemplo, indicando ficheros dentro del contenedor).

Podemos comprobar lo que está ejecutando el contenedor, ejecutando el siguiente comando:

```bash
$ docker logs contenedor2
```

Por último podemos parar el contenedor y borrarlo con las siguientes instrucciones:

```bash
$ docker stop contenedor2
$ docker rm contenedor2
```

Hay que tener en cuenta que un contenedor que esta ejecutándose no puede ser eliminado. Tendríamos que parar el contenedor y posteriormente borrarlo. Otra opción es borrarlo a la fuerza:

```bash
$ docker rm -f contenedor2
```