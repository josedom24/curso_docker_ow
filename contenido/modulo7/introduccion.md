# Introducción a la construcción y distribución de imágenes Docker

## Construcción de imágenes Docker

Hasta ahora hemos creado contenedores a partir de las imágenes que encontramos en Docker Hub. Estas imágenes las han creado otras personas.

Para crear un contenedor que sirva nuestra aplicación, tendremos que crear una imagen personalizada, es lo que llamamos **"dockerizar"** una aplicación.

![docker](img/build.png)

Tenemos dos mecanismos para construir nueva imágenes:

* **A partir de un contenedor**, podemos crea una nueva imagen usando el comando `docker commit`.
* **Automatizar la construcción** de una imagen Docker declarando los comandos que hay que ejecutar en un fichero llamado `Dockerfile` y usando el comando `docker build` para realizar la construcción.

## Ventajas de la construcción de imágenes usando Dockerfile

El método preferido para la creación de imágenes es el uso de ficheros `Dockerfile` y el comando `docker build`. Los motivos son los siguientes:

* **Podremos reproducir la imagen fácilmente** ya que en el fichero `Dockerfile` tenemos todas y cada una de las órdenes necesarias para la construcción de la imagen. Además el fichero `Dockerfile` se puede distribuir de manera muy sencilla y versionar usando un sistema de control de versiones.
* De manera sencilla podemos **cambiar la imagen base** usando un fichero `Dockerfile`, únicamente tendremos que modificar la primera línea de ese fichero como explicaremos posteriormente.

## Distribución de imágenes Docker

Una vez que hemos creado nuestra imagen personalizada, es hora de distribuirla para desplegarla en el entorno de producción. Para ello vamos a tener varias posibilidades:

* Utilizando los comandos `docker load`, que nos permite guardar una imagen en un fichero que podemos distribuir, para luego recuperar la imagen desde ese fichero con el comando `docker save`.
* Utilizando un registro de imágenes Docker, por ejemplo Docker Hub. En este caso usaremos el comando `docker push` para subir la imagen al registro y posteriormente podremos bajarla usando `docker pull`.