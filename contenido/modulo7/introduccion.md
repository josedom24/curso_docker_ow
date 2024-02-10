# Introducción a la construcción y distribución de imágenes Docker

## Construcción de imágenes Docker

Hasta ahora hemos creado contenedores a partir de las imágenes que encontramos en Docker Hub. Estas imágenes las han creado otras personas.

Para crear un contenedor que sirva nuestra aplicación, tendremos que crear una imagen personalizada, es lo que llamamos **"dockerizar"** una aplicación.

![docker](img/build.png)

Tenemos dos mecanismos para construir nueva imágenes:

* A partir de un contenedor que está en ejecución, podemos crea una nueva imagen usando el comando `docker commit`.
* Automatizar la construcción de una imagen Docker declarando los comandos que hay que ejecutar en un fichero llamado `Dockerfile` y usar el comando `docker build` para realizar la construcción.

## Inconvenientes de la construcción de imágenes a partir de contenedores

* **No se puede reproducir la imagen**. Si la perdemos tenemos que recordar toda la secuencia de órdenes que habíamos ejecutado desde que arrancamos el contenedor hasta que teníamos una versión definitiva e hicimos `docker commit`.
* **No podemos configurar el proceso que se ejecutará en el contenedor creado desde la imagen**. Los contenedores creados a partir de la nueva imagen ejecutaran por defecto el proceso que estaba configurado en la imagen base.
* **No podemos cambiar la imagen de base**. Si ha habido alguna actualización, problemas de seguridad, etc. con la imagen de base tenemos que descargar la nueva versión, volver a crear un nuevo contenedor basado en ella y ejecutar de nuevo toda la secuencia de órdenes.

## Ventajas de la construcción de imágenes usando Dockerfile

Por todas estas razones, el método preferido para la creación de imágenes es el uso de ficheros `Dockerfile` y el comando `docker build`. Con este método vamos a tener las siguientes ventajas:

* **Podremos reproducir la imagen fácilmente** ya que en el fichero `Dockerfile` tenemos todas y cada una de las órdenes necesarias para la construcción de la imagen. Si además ese `Dockerfile` está guardado en un sistema de control de versiones como git podremos, no sólo reproducir la imagen si no asociar los cambios en el `Dockerfile` a los cambios en las versiones de las imágenes creadas.
* **Podremos configurar el proceso que se ejecutará por defecto en los contenedores creados a partir de la nueva imagen**.
* Si queremos **cambiar la imagen de base** esto es extremadamente sencillo con un `Dockerfile`, únicamente tendremos que modificar la primera línea de ese fichero tal y como explicaremos posteriormente.

## Distribución de imágenes Docker

Una vez que hemos creado nuestra imagen personalizada, es la hora de distribuirla para desplegarla en el entorno de producción. Para ello vamos a tener varias posibilidades:

* Utilizando los comandos `docker load`, que nos permiten guardar una imagen en un fichero que podemos distribuir, para luego recuperar la imagen desde el fichero con el comando `docker save`.
* Utilizando un registro de imágenes Docker, por ejemplo Docker Hub. en este caso usaremos el comando `docker push` para subir la imagen al registro y posteriormente podremos bajar la iamgen usando `docker pull`.