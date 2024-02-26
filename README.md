# Curso Docker 2024

## Ejemplos

* [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_docker_ow)

## Contenido

1. Introducción a Docker
	* [Introducción a los contenedores](contenido/modulo1/contenedores.md) (P)
    * [Introducción a Docker](contenido/modulo1/docker.md) (P)
    * [Instalación de Docker Engine en Linux](contenido/modulo1/instalacion_linux.md)
    * [Instalación de Docker Desktop en Linux](contenido/modulo1/desktop_linux.md)
    * [Instalación de Docker Desktop en Windows](contenido/modulo1/desktop_windows.md)
    
2. Ejecución de contenedores
    * [El "Hola Mundo" de Docker](contenido/modulo2/holamundo.md) 
    * [Ejecución simple de contenedores](contenido/modulo2/contenedor.md) 
    * [Más opciones en la ejecución de contenedores (1ª parte)](contenido/modulo2/masopciones.md)
    * [Más opciones en la ejecución de contenedores (2ª parte)](contenido/modulo2/masopciones2.md)
    * [Gestión de contenedores Docker](contenido/modulo2/gestion.md)
    * [Ejemplo: Creando un contenedor con un servidor web](contenido/modulo2/web.md)
    * [Ejemplo: Configuración de un contenedor con la imagen MariaDB](contenido/modulo2/mariadb.md)
    * [Etiquetando los contenedores con Labels](contenido/modulo2/labels.md)
    * [Limitando los recursos utilizados por un contenedor Docker](contenido/modulo2/limite.md)

3. Gestión de imágenes en Docker
    * [Imágenes Docker](contenido/modulo3/imagenes.md) (P)
    * [Registro de imágenes: Docker Hub](contenido/modulo3/dockerhub.md)
    * [Gestión de Imágenes](contenido/modulo3/gestion.md)
    * [¿Cómo se organizan las imágenes?](contenido/modulo3/organizacion.md) (P)
    * [Demostración: Almacenamiento de imágenes y contenedores](contenido/modulo3/almacenamiento.md)
    * [Ejemplo: Desplegando la aplicación MediaWiki](contenido/modulo3/mediawiki.md)

4. Almacenamiento en Docker
    * [Los contenedores son efímeros](contenido/modulo4/efimeros.md)
    * [Almacenamiento en Docker](contenido/modulo4/almacenamiento.md) (P)
    * [Asociando almacenamiento a los contenedores: volúmenes Docker](contenido/modulo4/volumen.md)
    * [Asociando almacenamiento a los contenedores: bind mount](contenido/modulo4/bindmount.md)
    * [Ejemplo 1: Contenedor NextCloud con almacenamiento persistente](contenido/modulo4/nextcloud.md)
    * [Ejemplo 2: Contenedor MariaDB con almacenamiento persistente](contenido/modulo4/mariadb.md)
    * [Otros usos del almacenamiento en Docker](contenido/modulo4/otrosusos.md)

5. Redes en Docker
    * [Introducción a las redes en Docker](contenido/modulo5/redes.md) (P)
    * [Uso de la red host en Docker](contenido/modulo5/host.md)
    * [Uso de la red bridge por defecto](contenido/modulo5/bridge.md)
    * [Redes bridge definidas por el usuario](contenido/modulo5/usuario.md)
    * [Uso de la red bridge definidas por el usuario](contenido/modulo5/usuario2.md)
    * [Ejemplo 1: Despliegue de la aplicación Guestbook](contenido/modulo5/ejemplo1.md)
    * [Ejemplo 2: Despliegue de la aplicación Temperaturas](contenido/modulo5/ejemplo2.md)
    * [Ejemplo 3: Despliegue de WordPress + MariaDB](contenido/modulo5/ejemplo3.md)
    * [Ejemplo 4: Despliegue de Apache Tomcat + nginx](contenido/modulo5/ejemplo4.md) 

6. Creando escenarios multicontenedor con Docker Compose
    * [Creando escenarios multicontenedor con Docker Compose](contenido/modulo6/compose.md) (P)
    * [El fichero compose.yaml](contenido/modulo6/docker_compose.md) 
    * [El comando docker compose](contenido/modulo6/comando.md) 
    * [Almacenamiento con Docker Compose](contenido/modulo6/almacenamiento.md)
    * [Redes con Docker Compose](contenido/modulo6/redes.md)
    * [Ejemplo 1: Despliegue de la aplicación Guestbook](contenido/modulo6/ejemplo1.md)
    * [Ejemplo 2: Despliegue de la aplicación Temperaturas](contenido/modulo6/ejemplo2.md)
    * [Ejemplo 3: Despliegue de WordPress + MariaDB](contenido/modulo6/ejemplo3.md)
    * [Ejemplo 4: Despliegue de Apache Tomcat + nginx](contenido/modulo6/ejemplo4.md)
    * [Uso de parámetros con Docker Compose](contenido/modulo6/variables.md)
    * [Ejemplos reales de despliegues usando Docker Compose](contenido/modulo6/ejemplos.md) 

7. Creación de imágenes en Docker
    * [Introducción a la construcción y distribución de imágenes Docker](contenido/modulo7/introduccion.md) (P)
    * [Creación de imágenes a partir de un contenedor](contenido/modulo7/contenedor.md)
    * [El fichero Dockerfile](contenido/modulo7/docker-file.md) (P)
    * [Creación de imágenes a partir de un Dockerfile](contenido/modulo7/build.md)
    * [Distribución de imágenes](contenido/modulo7/distribucion.md)
    * [Ejemplo 1: Construcción de imágenes con una página estática](contenido/modulo7/ejemplo1.md)
    * [Ejemplo 2: Construcción de imágenes con una una aplicación PHP](contenido/modulo7/ejemplo2.md)
    * [Ejemplo 3: Construcción de imágenes con una una aplicación Python](contenido/modulo7/ejemplo3.md)
    * [Ejemplo 4: Construcción de imágenes configurables con variables de entorno](contenido/modulo7/ejemplo4.md)
    * [Ejemplo 5: Configuración de imágenes con una aplicación Java](contenido/modulo7/ejemplo5.md)
    * [Creación de imágenes con Docker Compose](contenido/modulo7/compose_build.md)
    * [Uso de ficheros Dockerfile parametrizados](contenido/modulo7/variables.md)
    * [Ciclo de vida de nuestras aplicaciones con Docker](contenido/modulo7/ciclodevida.md)
    * [Eliminar objetos Docker no utilizados](contenido/modulo7/prune.md)

8. Docker Desktop
    * Introducción a la interfaz de Docker Desktop
    * Gestión de contenedores
    * Gestión de imágenes
    * Gestión de volúmenes
    * Gestión de creación de imágenes

