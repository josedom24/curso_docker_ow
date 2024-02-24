# Redes con Docker Compose

Como hemos indicado anteriormente, cuando creamos un escenario con Docker Compose **se crea una nueva red bridge definida por el usuario donde se conectan los contenedores**, por lo tanto, obtenemos resolución por DNS que resuelve tanto el nombre del contenedor, como el nombre del servicio.

Sin embargo en el fichero `compose.yaml` podemos definir y configurar las redes que necesitemos en nuestro escenario, así como la conexión de los distintos contenedores a dichas redes.

Veamos un ejemplo, puedes encontrar el fichero en el [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_docker_ow).

El contenido del fichro `compose.yaml` es:

```yaml
version: '3.1'
services:
  c1:
    container_name: contenedor1
    image: alpine
    restart: always
    networks:
      red_externa:
        ipv4_address: 192.168.10.10
      red_interna:
        ipv4_address: 192.168.20.10
    hostname: contenedor1
    command: ash

  c2:
    container_name: contenedor2
    image: alpine
    restart: always
    networks:
      red_interna:
        ipv4_address: 192.168.20.20
    hostname: contenedor2
    command: ash
networks:
    red_externa:
        ipam:
            config:
              - subnet: 192.168.10.0/24
                gateway: 192.168.10.1
    red_interna:
        ipam:
            config:
              - subnet: 192.168.20.0/24
                gateway: 192.168.20.1
```

## Definición de redes en Docker Compose

* En la definición de los contenedores, hemos configurado la conexión de los contenedores a las redes usando el parámetro `networks`:
    * Indicamos el nombre de la red a la que está conectada.
    * Indicamos su dirección IP con el parámetro `ipv4_address`. Este parámetro es optativo, sino se indica se tomará una dirección IP de forma dinámica.
    * También hemos indicado el nombre del host con el parámetro `hostname`.
* El parámetro `networks` a continuación de la definición de servicios nos permite configurar las redes que vamos a usar en el escenario:
    * Indicamos el nombre de las redes que vamos a crear.
    * Configuramos la red en el parámetro `IPAM` y `config`, por ejemplo indicando el direccionamiento con el parámetro `subnet` y la puerta de enlace de la red con el parámetro `gateway`.

## Creación del escenario

Iniciamos el escenario:

```bash
$ docker-compose up -d
[+] Running 2/4
 ✔ Network redes_red_interna  Created                                                                       5.8s 
 ✔ Network redes_red_externa  Created                                                                       5.4s 
 ✔ Container contenedor2      Started                                                                       4.8s 
 ✔ Container contenedor1      Started                                                                       5.1s 
```

Comprobamos que los dos contenedores se están ejecutando:

```bash
$ $ docker compose ps
NAME          IMAGE     COMMAND     SERVICE   CREATED          STATUS          PORTS
contenedor1   alpine    "/bin/sh"   c1        19 seconds ago   Up 13 seconds   
contenedor2   alpine    "ash"       c2        19 seconds ago   Up 14 seconds
```

Comprobamos en el `contenedor1` que el hostname se ha configurado de manera adecuada:

```bash
$ docker compose exec c1 hostname
contenedor1
```

Comprobamos que el `contenedor1` está conectado a las dos redes y tiene las direcciones que hemos indicado:

```bash
$ docker compose exec c1 ip a
257: eth1@if258: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    inet 192.168.20.10/24 brd 192.168.20.255 scope global eth1
...
261: eth0@if262: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    inet 192.168.10.10/24 brd 192.168.10.255 scope global eth0
...
```

Comprobamos que tenemos resolución DNS tanto con el nombre del servicio como con el nombre del contenedor:

```bash
$ docker compose exec c1 nslookup contenedor2
...
Non-authoritative answer:
Name:	contenedor2
Address: 192.168.20.20

$ docker compose exec c1 nslookup c2
...
Non-authoritative answer:
Name:	c2
Address: 192.168.20.20
```

Y por último, comprobamos que hay conectividad:

```bash
docker compose exec c1 ping contenedor2
PING contenedor2 (192.168.20.20): 56 data bytes
64 bytes from 192.168.20.20: seq=0 ttl=64 time=0.072 ms
...
```

