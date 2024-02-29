# Imágenes Docker

## ¿Qué es una imagen Docker?

* Una imagen es una plantilla de sólo lectura con instrucciones para crear un contenedor Docker. 
* Contiene el sistema de fichero que tendrá el contenedor. 
* Además establece el comando que ejecutará el contenedor por defecto. 
* Podemos crear nuestras propias imágenes o utilizar las creadas por otros y publicadas en un registro. 
* Un contenedor es una instancia ejecutable de una imagen. 

## Registro de imágenes

* El Registro Docker es un componente donde se almacena las imágenes Docker.
* Tenemos un **registro local** donde se almacenan las imágenes desde las que vamos a crear contenedores. 
* Existen **registros remotos** que nos permiten distribuir las imágenes.
* Los registros pueden ser **públicos o privados**. 
* El registro público que nos ofrece Docker se llama [**Docker Hub**](https://hub.docker.com/). 

## Nombre de las imágenes

El nombre de una imagen suele estar formado por tres partes:

**usuario/nombre:etiqueta**

* **usuario**: El nombre del usuario que la ha generado. Las imágenes oficiales en Docker Hub no tienen nombre de usuario.
* **nombre**: Nombre significativo de la imagen.
* **etiqueta**: Nos permite versionar las imágenes. De esta manera controlamos los cambios que se van produciendo en ella. Si no indicamos etiqueta, por defecto se usa la etiqueta **latest**, por lo que la mayoría de las imágenes tienen una versión con este nombre.

## Ejemplo de etiquetas para la imagen wikimedia

[**MediaWiki**](https://www.mediawiki.org/wiki/MediaWiki/es) es una aplicación web escrita en PHP que nos permite elaborar una wiki. Si estudiamos la [documentación](https://hub.docker.com/_/mediawiki) de la imagen `mediawiki`, podemos ver las etiquetas disponibles para la imagen que corresponden a versiones distintas de la aplicación. En enero de 2024 serían las siguientes:

![ ](img/mediawiki_versiones.png)

Cada imagen está identificada por un **identificador**. Una misma imagen, puede estar etiquetada por etiquetas diferentes.

Por ejemplo, en la imagen `mediawiki`, las etiquetas `1.41.0`, `1.41`, `stable` y `latest` apuntan a la misma versión de la imagen.

## La etiqueta latest

Si utilizamos el nombre de una imagen sin indicar la etiqueta, se toma por defecto la etiqueta **latest** que suele corresponder a la última versión de la aplicación. En el caso concreto de la imagen `mediawiki`, observamos que la etiqueta `latest` corresponde a la última versión la `1.41`. Es más, podemos usar las siguientes etiquetas para indicar la misma versión: `1.41.0, 1.41, stable, latest`.

## ¿Para que sirvan las etiquetas de las imágenes?

* Normalmente las etiquetas nos permiten **versionar** las imágenes. 
* Podemos seguir observando que algunas etiquetas, nos indican además de la versión, los **servicios que tienen instalada** la imagen, por ejemplo si usamos la etiqueta `1.41.0-fpm` estaremos creando un contenedor con la ultima versión de la aplicación pero que además tendrá un servidor de aplicaciones php-fpm para servir la aplicación.
* Otro ejemplo: si usamos la etiqueta `1.41.0-fpm-alpine`, además de la última versión y que tiene instalado php-fpm, nos indica que **la imagen base** que se ha usado para crear la imagen es una distribución `alpine` que se caracteriza por ser una distribución muy liviana.

