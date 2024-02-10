# Curso Docker 2024

## Ejemplos

* [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_docker_ow)

## Contenido

1. Introducción a Docker
	* [Introducción a los contenedores](contenido/modulo1/contenedores.md)
    * [Introducción a Docker](contenido/modulo1/docker.md)
    * [Instalación de Docker Engine en Linux](contenido/modulo1/instalacion_linux.md)
    * Instalación de Docker Desktop en Windows
    
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
    * [Imágenes Docker](contenido/modulo3/imagenes.md)
    * [Registro de imágenes: Docker Hub](contenido/modulo3/dockerhub.md)
    * [Gestión de Imágenes](contenido/modulo3/gestion.md)
    * [¿Cómo se organizan las imágenes?](contenido/modulo3/organizacion.md)
    * [Demostración: Almacenamiento de imágenes y contenedores](contenido/modulo3/almacenamiento.md)
    * [Ejemplo: Desplegando la aplicación mediawiki](contenido/modulo3/mediawiki.md)

4. Almacenamiento en Docker
    * [Los contenedores son efímeros](contenido/modulo4/efimeros.md)
    * [Almacenamiento en Docker](contenido/modulo4/almacenamiento.md)
    * [Asociando almacenamiento a los contenedores: volúmenes Docker](contenido/modulo4/volumen.md)
    * [Asociando almacenamiento a los contenedores: bind mount](contenido/modulo4/bindmount.md)
    * [Ejemplo 1: Contenedor NextCloud con almacenamiento persistente](contenido/modulo4/nextcloud.md)
    * [Ejemplo 2: Contenedor MariaDB con almacenamiento persistente](contenido/modulo4/mariadb.md)
    * [Otros usos del almacenamiento en Docker](contenido/modulo4/otrosusos.md)

5. Redes en Docker
    * [Introducción a las redes en Docker](contenido/modulo5/redes.md)
    * [Uso de la red host en Docker](contenido/modulo5/host.md)
    * [Uso de la red bridge por defecto](contenido/modulo5/bridge.md)
    * [Redes bridge definidas por el usuario](contenido/modulo5/usuario.md)
    * [Uso de la red bridge definidas por el usuario](contenido/modulo5/usuario2.md)
    * [Ejemplo 1: Despliegue de la aplicación Guestbook](contenido/modulo5/ejemplo1.md)
    * [Ejemplo 2: Despliegue de la aplicación Temperaturas](contenido/modulo5/ejemplo2.md)
    * [Ejemplo 3: Despliegue de Wordpress + MariaDB](contenido/modulo5/ejemplo3.md)
    * [Ejemplo 4: Despliegue de tomcat + nginx](contenido/modulo5/ejemplo4.md) 

6. Creando escenarios multicontenedor con Docker Compose
    * [Creando escenarios multicontenedor con Docker Compose](contenido/modulo6/compose.md) 
    * [El fichero compose.yaml](contenido/modulo6/docker_compose.md) 
    * [El comando docker compose](contenido/modulo6/comando.md) 
    * [Almacenamiento con Docker Compose](contenido/modulo6/almacenamiento.md)
    * [Redes con Docker Compose](contenido/modulo6/redes.md)
    * [Ejemplo 1: Despliegue de la aplicación guestbook](contenido/modulo6/ejemplo1.md)
    * [Ejemplo 2: Despliegue de la aplicación Temperaturas](contenido/modulo6/ejemplo2.md)
    * [Ejemplo 3: Despliegue de WordPress + MariaDB](contenido/modulo6/ejemplo3.md)
    * [Ejemplo 4: Despliegue de tomcat + nginx](contenido/modulo6/ejemplo4.md)
    * [Uso de variables de entorno con Docker Compose](contenido/modulo6/variables.md)
    * Ejemplos reales de despliegues usando Docker Compose 

7. Creación de imágenes en Docker
    * [Introducción a la construcción y distribución de imágenes Docker](contenido/modulo7/introduccion.md)
    * [Creación de imágenes a partir de un contenedor](contenido/modulo7/contedor.md)
    * [El fichero dockerfile](contenido/modulo7/dockerfile.md)
    * [Creación de imágenes a partir de un Dockerfile](contenido/modulo7/build.md)
    * Uso de variables de entorno con Dockerfile
    * Distribución de imágenes
    * Ejemplo 1: Construcción de imágenes con una página estática
    * Ejemplo 2: Construcción de imágenes con una una aplicación PHP
    * Ejemplo 3: Construcción de imágenes con una una aplicación Python
    * Creación de imágenes con Docker Compose
    * Ciclo de vida de las aplicaciones 
    * Eliminar objetos Docker no utilizados

8. Docker Desktop
    * Instalación de Docker Desktop en Linux
    * Instalación de Docker Desktop en Windows
    * Introducción a la interfaz de Docker Desktop
    * Conectar Docker Desktop a la cuenta de Docker Hub
    * Gestión de imágenes
    * Gestión de contenedores
    * Docker init????
