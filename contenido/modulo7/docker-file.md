# El fichero Dockerfile

* Podemos automatizar la creación de imágenes Docker, declarando las instrucciones para crear la nueva imagen en un fichero llamado **Dockerfile**. A partir de este fichero y usando el comando **docker build** podemos construir una nueva imagen.
* En el fichero `Dockerfile` tenemos un conjunto de instrucciones que serán ejecutadas de forma secuencial para construir una nueva imagen Docker. 
* Las instrucciones que cambian el sistema de fichero crearán **una nueva capa**.
* La primera línea a añadir a un Dockerfile es una directiva `# syntax=docker/dockerfile:1` que se utiliza para especificar la versión del formato del fichero `Dockerfile` que se va a utilizar. Es opcional, pero recomendable.

## Ejemplo de Dockerfile

```Dockerfile
# syntax=docker/dockerfile:1
FROM debian:stable-slim
RUN apt-get update  && apt-get install -y  apache2 
WORKDIR /var/www/html
COPY index.html .
CMD apache2ctl -D FOREGROUND
```

## Instrucciones en el dockerfile

Las principales instrucciones que podemos usar:

* **FROM**: Sirve para especificar la imagen base sobre la que vamos a construir la nueva.
* **RUN**: Ejecuta una orden creando una nueva capa.  Ejemplo: `RUN apt update && apt install -y git`. En este caso es muy importante que pongamos la opción `-y` porque en el proceso de construcción no puede haber interacción con el usuario.
* **WORKDIR**: Establece el directorio de trabajo dentro de la imagen que estoy creando, las siguientes instrucciones se ejecutarán en este directorio.
* **COPY**: Copia ficheros desde mi equipo a la imagen. Esos ficheros deben estar en el mismo contexto, es decir en el mismo directorio que el fichero `Dockerfile`. Su sintaxis es `COPY [--chown=<usuario>:<grupo>] src dest`. 
* **ADD**: Es similar a COPY pero tiene funcionalidades adicionales como especificar URLs  y tratar archivos comprimidos.
* **LABEL**: Sirve para añadir metadatos a la imagen mediante clave=valor.
* **EXPOSE**: Nos da información acerca de qué puertos tendrá abiertos el contenedor cuando se cree uno en base a la imagen que estamos creando. Es meramente informativo.  
* **ENV**: Para establecer variables de entorno dentro del contenedor. Puede ser usado posteriormente en las órdenes `RUN` añadiendo `$` delante de el nombre de la variable de entorno. 
* **ARG**: Para definir variables para las cuales los usuarios pueden especificar valores a la hora de hacer el proceso de build mediante el parámetro  `--build-arg`. 
* **ENTRYPOINT**: Para establecer el ejecutable que se ejecuta en los contenedores que creamos a partir de esta imagen. El comando no se puede cambiar al crear el contenedor.
* **CMD**: Para establecer el ejecutable por defecto igual que el parámetro anterior. Pero en este caso, si podemos cambiarlo al crear el contenedor.

Para una descripción completa sobre el fichero `Dockerfile`, puedes acceder a la [documentación oficial](https://docs.docker.com/engine/reference/builder/).

## Construyendo imágenes con docker build

* El directorio donde se encuentra el fichero `Dockerfile` lo llamamos **entorno**. En este directorio tendremos los ficheros necesarios para crear la nueva imagen.
* El comando `docker build` construye la nueva imagen leyendo las instrucciones del fichero `Dockerfile` y los ficheros que hay en el **entorno**. El comando `docker build` ejecuta las instrucciones de un `Dockerfile` línea por línea y va mostrando los resultados en pantalla.
* La creación de la imagen es ejecutada por el demonio Docker, que recibe toda la información del entorno. 
* Tenemos que tener en cuenta que cada instrucción ejecutada crea una imagen intermedia, una vez finalizada la construcción de la imagen nos devuelve su id. 
* Algunas imágenes intermedias se guardan en **caché**, otras se borran. 
* Si en algún momento falla la creación de la imagen, al corregir el `Dockerfile` y volver a construir la imagen, los pasos que habían funcionado anteriormente no se repiten ya que tenemos a nuestra disposición las imágenes intermedias guardadas en caché, y el proceso continúa por la instrucción que causó el fallo.

## Buenas prácticas al crear Dockerfile

* **Los contenedores deben ser "efímeros"**: Cuando decimos "efímeros" queremos decir que la creación, parada, despliegue de los contenedores creados a partir de la imagen que vamos a generar con nuestro `Dockerfile` debe tener una mínima configuración.
* **Uso de ficheros `.dockerignore`**: Como hemos indicado anteriormente, todos los ficheros del contexto se envían al demonio Docker. Para aumentar el rendimiento, y no enviar ficheros innecesarios podemos hacer uso de un fichero `.dockerignore`, para excluir ficheros y directorios.
* **No instalar paquetes innecesarios**: Para reducir la complejidad, dependencias, tiempo de creación y tamaño de la imagen resultante, se debe evitar instalar paquetes extras o innecesarios. 
* **Minimizar el número de capas**: Debemos encontrar el balance entre la legibilidad del fichero `Dockerfile` y minimizar el número de capas que generamos.
* **No utilizar la etiqueta `latest`** al indicar la imagen base, ya que está va cambiando con el tiempo y si volvemos a crear la imagen dentro de un tiempo, es posible que estemos usando una imagen base diferente.
