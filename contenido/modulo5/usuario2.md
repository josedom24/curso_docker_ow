# Uso de la red bridge definidas por el usuario

En este apartado vamos a trabajar con las dos redes que hemos creado en el apartado anterior.
En primer lugar vamos a crear dos contenedores conectados a la primera red, para ello usaremos el parámetro `--network` en el comando `docker run`:

```bash
$ docker run -d --name servidorweb --network red1 nginx
$ docker run -it --name cliente --network red1 alpine
```

Lo primero que vamos a comprobar es la resolución DNS desde el contenedor `cliente`:

```bash
# nslookup servidorweb
Server:		127.0.0.11
Address:	127.0.0.11:53

Non-authoritative answer:

Non-authoritative answer:
Name:	servidorweb
Address: 172.18.0.2
```

Tenemos un servidor DNS en la dirección IP `172.0.0.11` que nos resuelve el nombre del primer contenedor con su dirección IP. Podemos comprobar que ese servidor DNS es el que tiene configurado el contenedor:

```bash
# cat /etc/resolv.conf 
nameserver 127.0.0.11
```

Y por lo tanto podemos realizar conexiones usando el nombre de los contenedores:

```bash
# ping servidorweb
PING servidorweb (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.399 ms
...
```

## Conectando los contenedores a otras redes

A continuación, creamos un contenedor conectado a la segunda red (`red2`) y comprobamos que no hay conectividad con los dos anteriores:

```bash
$ docker run -it --name cliente2 --network red2 alpine
```

Comprobamos su dirección IP y su puerta de enlace, e intentamos acceder a uno de los contenedores creados anteriormente:


```bash
# ip a
23: eth0@if24: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    inet 192.168.0.1/24 brd 192.168.0.255 scope global eth0
...

# ip r
default via 192.168.0.100 dev eth0 
...

# ping servidorweb
ping: bad address 'servidorweb'
```

Veamos cómo podemos conectar un contenedor a una red. Para ello usaremos el comando `docker network connect` y para desconectarla usaremos `docker network disconnect`. Salimos del contenedor que acabamos de crear, lo iniciamos y lo conectamos a la primera red:

```bash
$ docker start cliente2
$ docker network connect red1 cliente2
$ docker attach cliente2
```

Comprobamos que se ha creado una nueva interfaz de red con el direccionamiento de la red `red1`:

```bash
# ip a
25: eth0@if26: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    inet 192.168.0.1/24 brd 192.168.0.255 scope global eth0
...    
27: eth1@if28: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    inet 172.18.0.3/16 brd 172.18.255.255 scope global eth1
...
```

Ahora podemos comprobar si tenemos conectividad con el contenedor `servidorweb`:

```bash
# ping servidorweb
PING servidorweb (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.238 ms
...
```

Finalmente podemos desconectar el contenedor de la red, ejecutando el siguiente comando:

```bash
$ docker network disconnect red1 cliente2
```

## Más opciones al trabajar con redes en docker

Tanto al crear un contenedor con el parámetro `--network` para conectarlo a una red, como con la instrucción `docker network connect`, podemos usar algunos otros parámetros.

Veamos un ejemplo donde vamos a crear un contenedor en la red `red2` que tenemos creada:

```bash
$ docker run -it --name contenedor --network red2 \
                                   --ip 192.168.0.10 \
                                   --add-host=testing.example.com:192.168.0.20 \
                                   --dns 1.1.1.1 \
                                   --hostname servidor1 \
                                   alpine
```

* `--hostname servidor1`: Indicamos el nombre de la máquina. Lo comprobamos:

    ```bash
    # cat /etc/hostname 
    servidor1
    ```
* `--ip 192.168.0.10`: Nos permite poner una dirección IP fija en el contenedor. Vamos a comprobarlo:

    ```bash
    # ip a
    33: eth0@if34: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    inet 192.168.0.10/24 brd 192.168.0.255 scope global eth0
    ```
* `--add-host=testing.example.com:192.168.100.20`: Añadimos un nuevo nombre de host como resolución estática. Lo comprobamos:

    ```bash
    # cat /etc/hosts
    ...
    192.168.0.20	testing.example.com
    192.168.0.10	servidor1
    
    # ping testing.example.com
    PING testing.example.com (192.168.0.20): 56 data bytes
    ```
* `--dns 1.1.1.1`: Hemos configurado como DNS el servidor `1.1.1.1`. Veamos esto con detenimiento, como hemos visto anteriormente al conectar el contenedor a una red bridge definida por el usuario se crea un servidor DNS que nos permite la resolución por el nombre del contenedor veamos el servidor DNS:
    ```bash
    # cat /etc/resolv.conf 
    nameserver 127.0.0.11
    ...
    ```
    Por defecto este servidor hace forward con el servidor DNS que tenga configurado el anfitrión (es decir usa el DNS del anfitrión para resolver los nombre que no conoce). Con la opción `--dns 1.1.1.1`, estamos cambiando el DNS al que hacemos forwarding, por lo tanto ese cambio no se visualiza en el fichero `/etc/resolv.conf`.
