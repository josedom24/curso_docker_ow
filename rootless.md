apt install uidmap

grep ^$(whoami): /etc/subuid
usuario2:231072:65536

 grep ^$(whoami): /etc/subgid
usuario2:231072:65536


Si no está instalado:

sudo apt-get install -y dbus-user-session

Y hacer relogin

Deshabilitamos el demonio de docker:

sudo systemctl disable --now docker.service docker.socket

usuario2@docker:~$ dockerd-rootless-setuptool.sh install

Si este comando no existe debes instalar el paquete: docker-ce-rootless-extras

The systemd unit file is installed as ~/.config/systemd/user/docker.service.

systemctl --user start docker

To launch the daemon on system startup, enable the systemd service and lingering:

 systemctl --user enable docker

 sudo loginctl enable-linger $(whoami)


La instrucción sudo loginctl enable-linger $(whoami) habilita el servicio de "linger" para el usuario actual. Permitir que un usuario tenga habilitado el linger significa que su sesión no se cerrará automáticamente al cerrar la sesión de usuario o al apagar el sistema, sino que persistirá en segundo plano. Esto es útil para aplicaciones o servicios que necesitan ejecutarse continuamente incluso cuando el usuario no ha iniciado sesión activamente en el sistema.

Client

You need to specify either the socket path or the CLI context explicitly.

To specify the socket path using $DOCKER_HOST:

 export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock

 docker run -d -p 8080:80 nginx



 ping

On some distributions, ping does not work by default.

 sysctl -w "net.ipv4.ping_group_range=0 2147483647