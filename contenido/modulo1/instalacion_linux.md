# Instalación de Docker Engine en Linux

## Instalación de Docker Engine

**Docker Engine** está disponible en varias distribuciones de Linux, macOS y Windows a través de Docker Desktop y como instalación binaria estática, sólo se puede instalar en distribuciones Linux.. 

En este apartado vamos a realizar la instalación binaria de Docker Engine sobre una distribución Linux Ubuntu 22.04. Puedes seguir la [documentación oficial](https://docs.docker.com/engine/install/) para aprender el método de instalación en otros sistemas.

Para el sistema operativo Ubuntu puedes seguir las instrucciones y requerimientos en el siguiente [enlace](https://docs.docker.com/engine/install/ubuntu/). Suponemos que vamos a realizar la instalación en un sistema operativo que no tiene instalado versiones antiguas de Docker.

Tenemos varios métodos de instalación disponible: instalando Docker Desktop, usando los repositorios oficiales de Docker, descargando manualmente los ficheros `deb` o ejecutando un script de instalación.

En nuestro caso usaremos los repositorios apt de de Docker, para ello:

1. Actualizamos los repositorios e instalamos los paquetes necesarios para instalar desde repositorios HTTPS:

        sudo apt-get update
        sudo apt-get install ca-certificates curl gnupg

2. Añadir la clave GPG oficial de Docker:

        sudo install -m 0755 -d /etc/apt/keyrings
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
        sudo chmod a+r /etc/apt/keyrings/docker.gpg

3. Añadimos el repositorio oficial de Docker:

        echo \
        "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
        "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

4. Instalamos Docker Engine:

        sudo apt-get update
        sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Para manejar Docker con un usuario sin privilegio, debemos añadir el usuario al grupo `docker`, para ello:

    sudo usermod -aG docker $USER

Debes volver a iniciar una terminal con el usuario, o ejecutar el siguiente comando:

    su - $USER

Y finalmente, comprobamos la versión con la que vamos a trabajar:

    docker --version
    Docker version 25.0.0, build ...

Si queremos más información de las versiones de los componentes con los que vamos a trabajar, ejecutamos:

        docker version

Para más información del sistema que hemos instalado podemos ejecutar:

        docker info