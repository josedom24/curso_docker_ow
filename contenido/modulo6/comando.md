# El comando docker compose

Vamos a usar la instrucción `docker compose` para gestionar el ciclo de vida del escenario que tenemos definido en el fichero `compose.yaml`. 

Puedes encontrar el fichero en el [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_docker_ow).

**Es importante destacar que debemos ejecutar `docker compose` en el directorio en el que se encuentra el fichero `compose.yaml`**.

## Despliegue de Let's Chat

[Let’s Chat](https://github.com/sdelements/lets-chat) es una aplicación web escrita en Node.js que utilizando una base de datos MongoDB nos posibilita la creación de salas de chats.

Accedemos al directorio donde se encuentra el fichero `compose.yaml` donde hemos definido el escenario para ejecutar la aplicación Let's Chat y  ejecutamos la siguiente instrucción para crear los contenedores:

```bash
docker compose up -d
[+] Running 4/4
 ✔ Network escenario_letschat_default  Created                                      0.1s 
 ✔ Volume "escenario_letschat_mongo"   Create...                                    0.0s 
 ✔ Container mongo                     Started                                      0.3s 
 ✔ Container letschat                  Started                                      0.2s 
```

El parámetro `-d` de la instrucción `docker compose up` nos permite ejecutar los contenedores de forma desatendida (similar al parámetro `-d` en `docker run`). 

Tenemos que tener en cuenta que si no tenemos las imágenes en nuestro registro local, se descargarán. Además podemos ver cómo se ha creado una red bridge definida por el usuario. Esta red se crea con el nombre del proyecto (en nuestro caso el indicado con el parámetro `name`, si no indicamos este parámetro el nombre será el del directorio donde se encuentra el fichero `compose.yaml`) y el termino `default`. También observamos que se ha creado un volumen, en este caso su nombre será el del proyecto unido al nombre que hemos indicado en su definición.

Podemos ver los contenedores que se están ejecutando:

```bash
$ docker compose ps
NAME       IMAGE                  COMMAND                         SERVICE   CREATED              STATUS              PORTS
letschat   sdelements/lets-chat   "npm start"                     app       About a minute ago   Up About a minute   5222/tcp, 0.0.0.0:80->8080/tcp, :::80->8080/tcp
mongo      mongo:4                "docker-entrypoint.sh mongod"   db        About a minute ago   Up About a minute   27017/tcp
```

Podemos acceder desde el navegador a la aplicación:

![letschat](img/letschat.png)


Veamos más comandos que podemos ejecutar con Docker Compose:

* `docker compose stop`: Detiene los contenedores que previamente se han lanzado con `docker compose up`.
* `docker compose run`: Inicia los contenedores descritos en el `compose.yaml` que estén parados.
* `docker compose rm`: Borra los contenedores parados del escenario. Con las opción `-f` elimina también los contenedores en ejecución.
* `docker compose pause`: Pausa los contenedores que previamente se han lanzado con `docker compose up`.
* `docker compose unpause`: Reanuda los contenedores que previamente se han pausado.
* `docker compose restart`: Reinicia los contenedores. Orden ideal para reiniciar servicios con nuevas configuraciones.
* `docker compose logs`: Muestra los logs de todos los servicios del escenario. Con el parámetro `-f` podremos ir viendo los logs en "vivo".
* `docker compose logs servicio1`: Muestra los logs del servicio llamado `servicio1` que estaba descrito en el `compose.yaml`.
* `docker compose exec servicio1 /bin/bash`: Ejecuta una orden, en este caso `/bin/bash` en el servicio `servicio1` que estaba descrito en el `compose.yaml`
* `docker compose top`: Muestra  los procesos que están ejecutándose en cada uno de los contenedores de los servicios.


Para destruir los contenedores creados en el escenario y la red, ejecutamos:

```bash
$ docker compose down
[+] Running 3/3
 ✔ Container letschat        Removed                                             10.4s 
 ✔ Container mongo           Removed                                              0.4s 
 ✔ Network letschat_default  Removed                                              0.1s 
```

Si además queremos eliminar el volumen que se ha creado, usaremos el parámetro `-v`:

```bash
$ docker compose down -v
```


