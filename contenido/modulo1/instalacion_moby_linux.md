# Instalación de la versión de la comunidad de docker Moby en Linux

Si usamos debian o ubuntu podemos realizar la instalación de la versión de la comunidad [Moby](https://mobyproject.org/):

    sudo apt install docker.io

Si queremos usar el cliente de docker con un usuario sin privilegios:

    sudo usermod -aG docker $USER

Debes volver a iniciar una terminal con el usuario, o ejecutar el siguiente comando:

    su - $USER

El caso de Debian 12 la versión de la comunidad es la siguiente:

    $ docker --version
    Docker version 20.10.24+dfsg1, build 297e128

En es caso de Ubuntu 22.04, la versión será:

    $ docker --version
    Docker version 20.10.21, build 20.10.21-0ubuntu1~22.04.3x

Con esta distribución de Docker no podremos instalar **Docker Desktop**.
