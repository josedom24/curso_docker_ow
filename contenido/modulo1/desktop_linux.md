# Instalación de Docker Desktop en Linux

Podemos [instalar](https://docs.docker.com/desktop/install/linux-install/) **Docker Desktop** en distintas distribuciones Linux: Debian, Fedora, Ubuntu,... En este apartado vamos a realizar la instalación la distribución [Ubuntu](https://docs.docker.com/desktop/install/ubuntu/).

Los requisitos mínimos necesarios son:

* CPU con arquitectura de 64 bits y soporte de viirtualización.
* Virtualización KVM.
* Entorno gráfico Gnome, KDE o MATE.
* 4 Gb de RAM.
* A partir de Ubuntu 22.04.

1. Configuramos los repositorios de Docker:
    ```
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    
    echo \
    "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

2. Descargamos el paquete deb de la página oficial de instalación:

    ![linux](img/linux1.png)

3. Instalamos Docker Desktop:

    ```
    sudo apt-get update
    sudo apt-get install ./docker-desktop-<version>-<arch>.deb
    ```

    La `<version>` y el `<arch>` dependerán de qué versión y arquitectura te hayas descargado.

4. Tendremos a nuestra disposición un icono que permite iniciar **Docker Desktop**. En este momento se inciará la máquina virtual donde se está ejecutando el demonio Docker.

    ![linux](img/linux2.png)

    También tenemos a nuestra disposición en la máquina donde hemos instalado **Docker Desktop** el cliente Docker:

    ```bash
    $ docker version
    Client: Docker Engine - Community
     Cloud integration: v1.0.35+desktop.10
     Version:           25.0.3
     ...
    Server: Docker Desktop 4.27.2 (137060)
     Engine:
      Version:          25.0.3
      ...
