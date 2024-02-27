# Almacenamiento en Docker

![docker](img/almacenamiento.png)

Ante la situación anteriormente descrita, Docker nos proporciona varias soluciones para persistir los datos de los contenedores. Las opciones que nos ofrece Docker para gestionar el almacenamiento de los contenedores son:

* Los **volúmenes Docker**.
* Los **bind mount**.
* Los **tmpfs mounts**.


## Volúmenes Docker

* Los volúmenes son creados y gestionados por Docker.
* Un volumen corresponde a un directorio en el Host Docker, por tanto, la información se almacena en una parte del sistema de ficheros que es gestionada por Docker.
* Cuando se usa un volumen en un contenedor, el directorio correspondiente se monta en el sistema de archivo del contenedor.
* En sistemas Linux, los volúmenes se crean en `/var/lib/docker/volumes/`.
* Los procesos ajenos a Docker no deben modificar esta parte del sistema de archivos.
* Un volumen dado puede ser montado en múltiples contenedores simultáneamente. 
* Cuando ningún contenedor en ejecución está utilizando un volumen, el volumen sigue estando disponible para Docker y no se elimina automáticamente. 
* Cuando creas un volumen, puede tener nombre o ser anónimo. 
* A los volúmenes anónimos se les da un nombre aleatorio que se garantiza que sea único dentro de un Host Docker dado. 
* La gestión de los volúmenes se hace con el comando `docker volume`.

## Bind Mount

* Nos permite que un archivo o directorio del Host Docker se monte en un contenedor.
* El archivo o directorio es referenciado por su ruta completa en el Host Docker.
* No es necesario que el archivo o directorio ya exista en el Host Docker. Se crea bajo demanda si aún no existe.
* No puedes utilizar el cliente Docker para gestionarlos.
* Al realizar cambios sobre los ficheros del bind mount en el anfitrión, se cambian directamente en el contenedor.

## tmpfs mounts

* Un montaje tmpfs no persiste en disco, ni en el Host Docker ni dentro de un contenedor. 
* Puede ser utilizado por un contenedor durante la vida útil del contenedor, para almacenar el estado no persistente o información sensible.

## Cuando usar volúmenes Docker

* Compartir datos entre múltiples contenedores en ejecución.
* Cuando no se garantiza que el Host Docker tenga una determinada estructura de directorios o archivos. Los volúmenes te ayudan a desacoplar la configuración del Host Docker del tiempo de ejecución del contenedor.
* Cuando desea almacenar los datos de su contenedor en un servidor remoto o en un proveedor de nube.
* Cuando necesites hacer copias de seguridad, restaurar o migrar datos de un Host Docker a otro, los volúmenes son una mejor opción, ya que simplemente debes copiar el directorio `/var/lib/docker/volumes/<nombre-volumen>`.

## Cuando usar bind mounts

* Compartir archivos de configuración desde la máquina anfitriona a los contenedores.
* Compartir código fuente o artefactos de construcción entre un entorno de desarrollo en el Host Docker y un contenedor.
* Cuando hay necesidad de que otras aplicaciones que no sean Docker tengan acceso a esos ficheros, ya sean código, ficheros etc...

## Aspectos a tener en cuenta en el uso de volúmenes o bind mount

* Si montas un volumen vacío en un directorio del contenedor en el que existen archivos o directorios, estos archivos o directorios se propagan (copian) al volumen. 
* Del mismo modo, si inicias un contenedor y especificas un volumen que aún no existe, se creará un volumen vacío para ti. 
* Si montas un bind mount o un volumen no vacío en un directorio del contenedor en el que existen algunos archivos o directorios, estos archivos o directorios quedan ocultos por el montaje.

## Uso de almacenamiento en contenedores

En la creación de contenedores con `docker run` puedo indicar que vamos a usar almacenamiento para guardar la información de ciertos directorios. Tanto en el caso de uso de volúmenes como en el caso del uso de bind mount podemos indicar el uso de almacenamiento en la creación de un contenedor con las siguientes parámetros del comando `docker run`:

* El parámetro `--volume` o `-v`
* El parámetro `--mount`

En general, `--mount` es más explícito y detallado. La mayor diferencia es que la sintaxis `-v` combina todas las opciones en un solo campo, mientras que la sintaxis `--mount` las separa.

* Si usamos `-v` se debe indicar tres campos separados por dos puntos:
    * El primer campo es el nombre del volumen, debe ser único en una determinada máquina. Para volúmenes anónimos, el primer campo se omite.
    * El segundo campo es la ruta donde se montan el archivo o directorio en el contenedor.
    * El tercer campo es opcional, y es una lista de opciones separadas por comas. Por ejemplo podemos indicar `ro` para configurar el montaje sólo de lectura.
* Si usamos `--mount` hay que indicar un conjunto de datos de la forma `clave=valor`, separados por coma.
    * Clave `type`: Indica el tipo de montaje. Los valores pueden ser `bind`, `volume` o `tmpfs`.
    * Clave `source` o `src`: La fuente del montaje. Se indica el volumen o el directorio que se va montar con bind mount.
    * Clave `dst` o `target`: Será la ruta donde está montado el fichero o directorio en el contenedor. 
    * La opción `readonly` o `ro` es optativa, e indica que el montaje es de sólo lectura.
    * La clave `volume-opt` para indicar opciones más específicas del montaje.


## ¿Qué información tenemos que guardar?

¿Qué debemos guardar de forma persistente en un contenedor?

* Los datos de la aplicación.
* Los logs del servicio.
* La configuración del servicio. En este caso podemos añadirla a la imagen, pero será necesaria la creación de una nueva imagen si cambiamos la configuración. Si la guardamos en un volumen hay que tener en cuanta que ese fichero lo tenemos que tener en el entorno de producción (puede ser bueno, porque las configuraciones de los distintos entornos puede variar).



