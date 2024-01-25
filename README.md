# Curso Docker 2023

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
    * [Ejemplo: Configuración de un contenedor con la imagen mariadb](contenido/modulo2/mariadb.md)
    * [Etiquetando los contenedores con Labels](contenido/modulo2/labels.md)
    * Limitando los recursos utilizados por un contenedor docker

3. Gestión de imágenes en Docker
    * Imágenes docker
    * ¿Cómo se organizan las imágenes? https://docs.docker.com/storage/storagedriver/
    * Registro de imágenes: Docker Hub




    * Registro de imágenes: Docker Hub
    * Imágenes de confianza en Docker Hub (https://docs.docker.com/trusted-content/official-images/)
    * Gestión de imágenes
    * ¿Cómo se organizan las imágenes?
    * Creación de contenedores desde imágenes
    * [Ejemplo: Desplegando la aplicación mediawiki](contenido/modulo3/mediawiki.md)

4. Almacenamiento en Docker
    * Los contenedores son efímeros
    * Volúmenes Docker y bind mount
    * Asociando almacenamiento a los contenedores: volúmenes Docker
    * Asociando almacenamiento a los contenedores: bind mount
    * Ejemplo 1: Contenedor nextcloud con almacenamiento persistente
    * Ejemplo 2: Contenedor mariadb con almacenamiento persistente
    * Otros usos del almacenamiento 

5. Redes en Docker
    * Introducción a las redes en Docker
    * Tipos de redes en Docker
    * Gestionando las redes en Docker
    * Uso de la red bridge por defecto
    * Uso de las redes bridge definidas por el usuario
    * Convertir el uso de la red bridge por defecto por una definida por el usuario
    * Ejemplo 1: Despliegue de la aplicación Guestbook
    * Ejemplo 2: Despliegue de la aplicación Temperaturas
    * Ejemplo 3: Despliegue de Wordpress + mariadb
    * Ejemplo 4: Despliegue de tomcat + nginx 
    * Eliminar objetos Docker no utilizados

6. Creando escenarios multicontenedor con Docker Compose
    * Instalación de Docker Compose
    * El fichero docker-compose.yml
    * El comando docker compose
    * Almacenamiento con Docker Compose
    * Redes con Docker Compose
    * Ejemplo 1: Despliegue de la aplicación guestbook
    * Ejemplo 2: Despliegue de la aplicación Temperaturas
    * Ejemplo 3: Despliegue de WordPress + Mariadb
    * Ejemplo 4: Despliegue de tomcat + nginx
    * Ejemplos reales de despliegues usando Docker Compose 

7. Creación de imágenes en Docker 
    * Creación de imágenes a partir de un contenedor
    * Creación de imágenes a partir de un Dockerfile
    * Distribución de imágenes
    * Ejemplo 1: Construcción de imágenes con una página estática
    * Ejemplo 2: Construcción de imágenes con una una aplicación PHP
    * Ejemplo 3: Construcción de imágenes con una una aplicación Python
    * Análisis de imágenes con Docker Scout (https://docs.docker.com/scout/quickstart/)
    * Ciclo de vida de las aplicaciones 

8. Docker Desktop
    * Instalación de Docker Desktop en Linux
    * Instalación de Docker Desktop en Windows
    * Introducción a la interfaz de Docker Desktop
    * Conectar Docker Desktop a la cuenta de Docker Hub
    * Gestión de imágenes
    * Gestión de contenedores
