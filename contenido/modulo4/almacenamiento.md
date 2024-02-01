# Almacenamiento en Docker

![docker](img/almacenamiento.png)

Ante la situación anteriormente descrita Docker nos proporciona varias soluciones para persistir los datos de los contenedores. En este curso nos vamos a centrar en las dos que considero que son más importantes:

* Los **volúmenes docker**.
* Los **bind mount**.
* Los **tmpfs mounts**.


## Volúmenes Docker

* Los volúmenes son creados y gestionados por Docker.
* Un volumen corresponde a un directorio en el host, por tanto, la información se almacena en una parte del sistema de ficheros que es gestionada por Docker.
* Cuando se usa un volumen en un contenedor, el direcotrio correspondiente se monta en el sistema de archivo del contenedor.
* En sistemas Linux, los volúmenes se crean en `/var/lib/docker/volumes/`.
* Los procesos ajenos a Docker no deben modificar esta parte del sistema de archivos.
* Un volumen dado puede ser montado en múltiples contenedores simultáneamente. 
* Cuando ningún contenedor en ejecución está utilizando un volumen, el volumen sigue estando disponible para Docker y no se elimina automáticamente. 
* Cuando montas un volumen, puede tener nombre o ser anónimo. 
* A los volúmenes anónimos se les da un nombre aleatorio que se garantiza que sea único dentro de un host Docker dado. 
* La gestión de los volúmenes se hace con el comando `docker volume`.

## Bind Mount

* Nos permite que un archivo o directorio de la máquina anfitriona se monte en un contenedor.
* El archivo o directorio es referenciado por su ruta completa en la máquina anfitriona.
* No es necesario que el archivo o directorio ya exista en el host Docker. Se crea bajo demanda si aún no existe.
* No puedes utilizar el cliente Docker para gestionarlos.
* Al realizar cambios sobre los ficheros del bind mount en el anfitrión, se cambian directamente en el contenedor.

## tmpfs mounts

* Un montaje tmpfs no persiste en disco, ni en el host Docker ni dentro de un contenedor. 
* Puede ser utilizado por un contenedor durante la vida útil del contenedor, para almacenar el estado no persistente o información sensible.

## Cuando usar volúmenes Docker

* Compartir datos entre múltiples contenedores en ejecución.
* Cuando no se garantiza que el host Docker tenga una determinada estructura de directorios o archivos. Los volúmenes te ayudan a desacoplar la configuración del host Docker del tiempo de ejecución del contenedor.
* Cuando desea almacenar los datos de su contenedor en un host remoto o en un proveedor de nube.
* Cuando necesite hacer copias de seguridad, restaurar o migrar datos de un host Docker a otro, los volúmenes son una mejor opción. Simplemente copiando el directorio `/var/lib/docker/volumes/<nombre-volumen>`.

## Cuando usar bind mounts

* Compartir archivos de configuración desde la máquina anfitriona a los contenedores.
* Compartir código fuente o artefactos de construcción entre un entorno de desarrollo en el host Docker y un contenedor.
* Que otras aplicaciones que no sean docker tengan acceso a esos ficheros, ya sean código, ficheros etc...

## Aspectos en el uso de volúmenes o bind mount

* Si montas un volumen vacío en un directorio del contenedor en el que existen archivos o directorios, estos archivos o directorios se propagan (copian) al volumen. 
* Del mismo modo, si inicias un contenedor y especificas un volumen que aún no existe, se creará un volumen vacío para ti. 
* Si montas un bind mount o un volumen no vacío en un directorio del contenedor en el que existen algunos archivos o directorios, estos archivos o directorios quedan ocultos por el montaje.



