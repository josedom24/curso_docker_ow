# Curso Docker 2023

1. Introducción a Docker
	* Introducción a los contenedores
    * Introducción a Docker
    * [Instalación de Docker Engine en Linux](contenido/modulo1/instalacion_linux.md)
        * Aquí se puede introducir docker info (https://docs.docker.com/engine/reference/commandline/info/)
        * Es lo mismo que docker system info
        * Aquí se puede hablar de [Instalación de la versión de la comunidad de docker Moby en Linux](contenido/modulo1/instalacion_moby_linux.md)
    * Trabajando con Docker en Microsoft Windows
    
2. Ejecución de contenedores (https://docs.docker.com/engine/reference/run/)
    * [El "Hola Mundo" de Docker](contenido/modulo2/holamundo.md) 
        * Aquí se puede introducir docker create (https://docs.docker.com/engine/reference/commandline/create/)
    * [Ejecución simple de contenedores](contenido/modulo2/contenedor.md) 
    * [Ejecutando un contenedor interactivo](contenido/modulo2/interactivo.md)
    * [Creando un contenedor demonio](contenido/modulo2/demonio.md)
    * [Creando un contenedor con un servidor web](contenido/modulo2/web.md)
    * [Configuración de contenedores con variables de entorno](contenido/modulo2/entorno.md)
    * Etiquetando los contenedores con Labels (https://docs.docker.com/config/labels-custom-metadata/)
    * Limitando los recursos utilizados por un contenedor (https://docs.docker.com/config/containers/resource_constraints/)
        * Aquí se puede introducir docker stats (https://docs.docker.com/engine/reference/commandline/stats/)
        * https://blog.codmind.com/establecer-quotas-o-maximos-de-uso-de-cpu-ram-y-disco-de-un-contenedor-docker/
    * [Ejemplo: Configuración de un contenedor con la imagen mariadb](contenido/modulo2/mariadb.md)

3. Gestión de imágenes en Docker (https://docs.docker.com/docker-hub/)
    * Registro de imágenes: Docker Hub
    * Gestión de imágenes
    * ¿Cómo se organizan las imágenes?
    * Creación de contenedores desde imágenes
    * [Ejemplo: Desplegando la aplicación mediawiki](contenido/modulo3/mediawiki.md)
 
4. Almacenamiento en Docker (https://docs.docker.com/storage/)
    * Los contenedores son efímeros
    * Volúmenes Docker y bind mount
    * Asociando almacenamiento a los contenedores: volúmenes Docker
    * Asociando almacenamiento a los contenedores: bind mount
    * Ejemplo 1: Contenedor nextcloud con almacenamiento persistente
    * Ejemplo 2: Contenedor mariadb con almacenamiento persistente
    * Otros usos del almacenamiento 

5. Redes en Docker (https://docs.docker.com/network/)
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
    * Eliminar objetos Docker no utilizados (https://docs.docker.com/config/pruning/)
        * docker system prune

6. Creando escenarios multicontenedor con Docker Compose (https://docs.docker.com/compose/)
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

7. Creación de imágenes en Docker (https://docs.docker.com/build/; https://docs.docker.com/build/guide/)
    * Creación de imágenes a partir de un contenedor
    * Creación de imágenes a partir de un Dockerfile
    * Distribución de imágenes
    * Ejemplo 1: Construcción de imágenes con una página estática
    * Ejemplo 2: Construcción de imágenes con una una aplicación PHP
    * Ejemplo 3: Construcción de imágenes con una una aplicación Python
    * Ciclo de vida de las aplicaciones 

8. Docker Desktop (https://docs.docker.com/desktop/)
    * Instalación de Docker Desktop en Linux
    * Instalación de Docker Desktop en Windows
    * Introducción a la interfaz de Docker Desktop (https://docs.docker.com/desktop/use-desktop/)
    * Conectar Docker Desktop a la cuenta de Docker Hub
    * Gestión de imágenes
    * Gestión de contenedores


* docker (base command)
* docker attach
* docker build
* docker commit
* docker cp
* docker create
* docker diff
* docker events
* docker exec
* docker export
* docker history
    * docker image
    * docker image build
    * docker image history
    * docker image import
    * docker image inspect
    * docker image load
    * docker image ls
    * docker image prune
    * docker image pull
    * docker image push
    * docker image rm
    * docker image save
    * docker image tag
* docker images
* docker import
* docker info
* docker init (Beta)
* docker inspect
* docker kill
* docker load
* docker login
* docker logout
* docker logs
    * docker manifest
    * docker manifest annotate
    * docker manifest create
    * docker manifest inspect
    * docker manifest push
    * docker manifest rm
    * docker network
    * docker network connect
    * docker network create
    * docker network disconnect
    * docker network inspect
    * docker network ls
    * docker network prune
    * docker network rm
    * docker node
    * docker node demote
    * docker node inspect
    * docker node ls
    * docker node promote
    * docker node ps
    * docker node rm
    * docker node update
* docker pause
    * docker plugin
    * docker plugin create
    * docker plugin disable
    * docker plugin enable
    * docker plugin inspect
    * docker plugin install
    * docker plugin ls
    * docker plugin rm
    * docker plugin set
    * docker plugin upgrade
* docker port
* docker ps
* docker pull
* docker push
* docker rename
* docker restart
* docker rm
* docker rmi
* docker run
* docker save
    * docker scout
    * docker scout cache
    * docker scout cache df
    * docker scout cache prune
    * docker scout compare
    * docker scout config
    * docker scout cves
    * docker scout enroll
    * docker scout environment
    * docker scout integration
    * docker scout integration configure
    * docker scout integration delete
    * docker scout integration list
    * docker scout policy
    * docker scout quickview
    * docker scout recommendations
    * docker scout repo
    * docker scout repo disable
    * docker scout repo enable
    * docker scout repo list
    * docker scout sbom
    * docker scout stream
    * docker scout version
    * docker scout watch
* docker search
    * docker secret
    * docker secret create
    * docker secret inspect
    * docker secret ls
    * docker secret rm
* docker start
    * docker service
    * docker service create
    * docker service inspect
    * docker service logs
    * docker service ls
    * docker service ps
    * docker service rollback
    * docker service rm
    * docker service scale
    * docker service update
    * docker stack
    * docker stack config
    * docker stack deploy
    * docker stack ls
    * docker stack ps
    * docker stack rm
    * docker stack services
* docker stats
* docker stop
    * docker swarm
    * docker swarm ca
    * docker swarm init
    * docker swarm join-token
    * docker swarm join
    * docker swarm leave
    * docker swarm unlock-key
    * docker swarm unlock
    * docker swarm update
    * docker system
    * docker system df
    * docker system events
    * docker system info
    * docker system prune
* docker tag
* docker top
    * docker trust
    * docker trust inspect
    * docker trust key
    * docker trust key generate
    * docker trust key load
    * docker trust revoke
    * docker trust sign
    * docker trust signer
    * docker trust signer add
    * docker trust signer remove
* docker unpause
* docker update
* docker version
    * docker volume create
    * docker volume inspect
    * docker volume ls
    * docker volume prune
    * docker volume rm
    * docker volume update
* docker wait