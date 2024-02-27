# Limitando los recursos utilizados por un contenedor Docker

Cuando creamos un contenedor, los procesos que se ejecuten en él pueden usar todos los recursos del Host Docker en el que se está ejecutando. Puedes limitar los recursos de CPU y memoria al crear un contenedor en Docker utilizando las opciones `--cpus` y `--memory` respectivamente. 

## Limitando el uso de CPU

Lo primero que podemos averiguar es el número de CPUs que tiene el Host Docker, para ello ejecutamos el comando:

```bash
$ nproc --all
```

Para limitar la cantidad de recursos de CPU que puede utilizar un contenedor, puedes utilizar la opción `--cpus`. Puedes especificar la cantidad de CPUs que deseas asignar al contenedor, ya sea en términos de núcleos completos o fracciones de núcleos. Por ejemplo, para limitar un contenedor a utilizar un núcleo completo:

```bash
docker run -d --cpus 1 --name servidor_web httpd:2.4
```

Podríamos indicar que utilice medio núcleo (`--cpus=0.5`), o usar 2 núcleos (`--cpus=2`) o usar una fracción de cpu, por ejemplo el 75% (`--cpus=0.75`).

Cuando inspeccionas un contenedor Docker con el comando `docker inspect`, puedes utilizar el campo `NanoCpus` para ver la cantidad de CPU asignada al contenedor en unidades de *nanocpus*. Un *nanocpu* es una unidad de medida que representa la fracción de tiempo de CPU.

```bash
$ docker inspect --format '{{.HostConfig.NanoCpus}}' servidor_web
```

Este comando mostrará la cantidad de CPU asignada al contenedor en unidades de *nanocpus*. Ten en cuenta que la interpretación de estos valores puede no ser intuitiva, pero representan la capacidad de procesamiento relativa asignada al contenedor en comparación con la capacidad total del sistema.

Si deseas obtener información más legible, puedes convertir los *nanocpus* a CPUs utilizando los siguientes comandos:

```bash
nano_cpus=$(docker inspect --format '{{.HostConfig.NanoCpus}}' servidor_web)
cpus=$(echo "scale=2; $nano_cpus / 1000000000" | bc)
echo "CPUs asignadas al contenedor: $cpus"
```

Este fragmento de código convierte los *nanocpus* a CPUs dividiendo por 1000000000 (mil millones) utilizando la herramienta `bc` (calculadora de línea de comandos). La variable `cpus` contendrá la cantidad de CPUs asignadas al contenedor.


## Limitando el uso de memoria


Para limitar la cantidad de memoria que puede utilizar un contenedor, puedes utilizar la opción `--memory`. Puedes especificar la memoria en bytes, kilobytes, megabytes, gigabytes, o utilizando el formato abreviado con las letras *b, k, m, g*. Por ejemplo para limitar un contenedor a 512 megabytes de memoria:

```bash
$ docker run -d --memory 512m --name servidor_web httpd:2.4
```

Con el comando `docker stats` podemos ver los recursos que está consumiendo un contenedor, y además vemos el límite de RAM que le hemos configurado:

```bash
$ docker stats servidor_web
CONTAINER ID   NAME           CPU %     MEM USAGE / LIMIT   MEM %     NET I/O       BLOCK I/O    PIDS
fd2ee83e755a   servidor_web   0.01%     6MiB / 512MiB       1.17%     4.25kB / 0B   0B / 4.1kB   82
```

También podemos usar `docker inspect` para ver el límite de memoria que hemos configurado en un contenedor:

```bash
$ docker inspect --format '{{.HostConfig.Memory}}' servidor_web
```

Este comando te dará el límite de memoria en bytes. Si deseas convertirlo a un formato más legible, puedes hacerlo utilizando las siguientes instrucciones en bash:

```bash
memory_limit=$(docker inspect --format '{{.HostConfig.Memory}}' servidor_web)
memory_limit_mb=$(echo "scale=2; $memory_limit / 1048576" | bc)
echo "Límite de memoria asignado al contenedor: $memory_limit_mb MB"
```

En este fragmento de código, estoy dividiendo el límite de memoria en bytes por 1048576 (que es 1024 al cubo) para convertirlo a megabytes.

## Modificando los límites en tiempo de ejecución

Evidentemente podemos limitar la memoria y el número de CPUs utilizadas al mismo tiempo al crear un contenedor:

```bash
$ docker run -d --memory 512m --cpus 0.5 --name servidor_web httpd:2.4
```

Con la instrucción `docker update` podemos cambiar distintos parámetros de configuración de un contenedor que se está ejecutando, entre ellos podemos cambiar los límites de memoria y CPUs:

```bash
$ docker update --memory 1g --cpus 1.5 servidor_web
```
```