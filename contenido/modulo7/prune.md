# Eliminar objetos Docker no utilizados

## Eliminar imágenes huérfanas

Cuando empezamos a construir nuestras propias imágenes Docker, nos encontramos que al listar las imágenes aparecen algunas con el nombre y la etiqueta con el valor `<none>`. Estas son imágenes intermedias que se han generado y que no forman parte de ninguna imagen, por lo tanto pueden ocupar espacio en disco que no es necesario. Estas imágenes se llaman "colgadas" (dangling).

Por ejemplo:

```bash
$ $ docker images
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
josedom24/ejemplo1   v1        c23cf3f2d251   3 minutes ago    187MB
<none>               <none>    ea9228809799   36 minutes ago   187MB
...
```
Si queremos borrar estas imágenes podemos ejecutar:

```bash
$ docker image prune
WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N]
```

Si además añadimos el parámetro `-a` se eliminaran todas las imágenes que no tienen ningún contenedor creado.

```bash
$ docker image prune -a
WARNING! This will remove all images without at least one container associated to them.
Are you sure you want to continue? [y/N] 
```

## Eliminar otros objetos Docker

Si queremos eliminar los contenedores que están parados, podemos ejecutar:

```bash
$ docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] 
```

Para eliminar redes que no se estén utilizando podemos ejecutar:

```bash
$ docker network prune
WARNING! This will remove all custom networks not used by at least one container.
Are you sure you want to continue? [y/N] 
```

Para eliminar volúmenes que no se están utilizando por ningún contenedor:

```bash
$ docker volume prune
WARNING! This will remove anonymous local volumes not used by at least one container.
Are you sure you want to continue? [y/N] 
```

Por último, podemos borrar todos los elementos que no se están utilizando con una sola instrucción: `docker system prune`. Para borrar los volúmenes no usados debemos usar el parámetro `--volumes`:

```bash
$ docker system prune --volumes
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all anonymous volumes not used by at least one container
  - all dangling images
  - unused build cache

Are you sure you want to continue? [y/N] 
```



