# Creación de imágenes con Docker Compose

En este ejemplo vamos a ver la configuración de Docker Compose para construir la imagen que va a utilizar en la creación del servicio. En este caso no se indica la imagen, se indica el directorio de contexto donde encontramos el fichero `Dockerfile` para la construcción de la imagen.

Puedes encontrar los ficheros necesarios para realizar este ejemplo en el [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_docker_ow).

## Ejemplo de construcción de imagen en Docker Compose

En este ejemplo vamos a crear una imagen a a partir de una aplicación Python construida con el framework Flask. El código de la aplicación lo guardamos en el fichero `app.py` y es el siguiente:

```python
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hola!!! has entrado en esta página {} veces.\n'.format(count)
```

El programa guarda en una base de datos redis un contador que se incrementa cada vez que accedemos a la página. Es importante apuntar que el nombre que se usa para acceder al servidor de base de datos es `redis`.

En el fichero `requirements.txt` tenemos las dependencias necesarias para que el programa funcione, en nuestro caso el contenido de este fichero es:

```
flask
redis
```

El fichero `Dockerfile` que vamos a usar para la construcción de la imagen tiene el siguiente contenido:

```Dockerfile
# syntax=docker/dockerfile:1
FROM python:3.10-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```

Por último el fichero `compose.yaml` que vamos a usar para levantar el escenario tendrá el siguiente contenido:

```yaml
version: '3.1'
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
```

Como podemos observar, en el servicio `web` no hemos indicado la imagen. Hemos indicado el directorio de contexto, en nuestro caso un punto, en el parámetro `build:`. Al levantar el escenario con Docker Compose, si es necesario se construirá la imagen que vamos a usar para crear el contenedor. Cómo vemos usamos el puerto 8000/tcp para acceder a la aplicación, que internamente usa el puerto estándar de Flask que es el 5000/tcp.

Vamos a construir la aplicación:

```bash
$ docker compose up -d
...
[web internal] load build definition from Dockerfile
...
```

Como vemos para crear el servicio `web` se realiza el proceso de construcción de la imagen a a partir del fichero `Dockerfile`. Podemos acceder a la aplicación accediendo a la URL `http://localhost:8000`, por ejemplo usando el comando `curl`:

```bash
$ curl http://localhost:8000
Hola!!! has entrado en esta página 1 veces.
```

Evidentemente si borramos el escenario y volvemos a acceder no será necesario de nuevo la construcción de la imagen:

```bash
$ docker compose down
...
$ docker compose up -d
[+] Running 2/3
 ✔ Network build_default    Created 
 ✔ Container build-redis-1  Started 
 ✔ Container build-web-1    Started                         
```

Si se produce un cambio en la aplicación o en el fichero `Dockerfile`, la próxima vez que levantemos el escenario tendremos que indicar que queremos volver a construir la imagen con el parámetro `--build`. Por ejemplo, hemos cambiado el mensaje que muestra la aplicación en la función principal de la aplicación, borramos el escenario y lo volvemos a crear indicando que queremos volver a construir la imagen del servicio `web`:

```bash
$ docker compose down
...
$ docker compose up -d --build 
...
 => [web internal] load build definition from Dockerfile       
```


