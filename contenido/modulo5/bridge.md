# Uso de la red bridge por defecto

Hasta ahora todos los contenedores lo hemos conectado a la red **bridge** por defecto. Como ya hemos dicho las características más importantes de este tipo de red son las siguientes:
    
* Se crea en el Host Docker un *Linux Bridge* llamado **docker0**.
* El direccionamiento de esta red es 172.17.0.0/16.
* Usamos el parámetro `-p` en `docker run` para exponer algún puerto. Se crea una regla DNAT para tener acceso al puerto.
* Los contenedores conectados a un red **bridge** tiene acceso a internet por medio de una regla SNAT.

## Ejemplo de uso de la red bridge por defecto

Vamos a crear un contenedor conectado a la red **bridge** por defecto a partir de la imagen `alpine`. Vamos a mapear el puerto 80/tcp ya que a continuación instalaremos un servidor web en el contenedor:

```bash
docker run -it -p 8080:80 --name contenedor1 alpine ash
```

**Nota**: `ash` es la shell de la distribución Alpine. En realidad no habría que indicarlo, ya que el contenedor va a ejecutar ese comando por defecto.

### Configuración de red del contenedor

Accedemos al contenedor y comprobamos su configuración de red:

```bash
# ip a
...
4: eth0@if5: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0

# ip r
default via 172.17.0.1 dev eth0
...
```

Hemos visto que su dirección IP está en la red `172.17.0.0/16` y la puerta de enlace es la `172.17.0.1` que corresponde a la dirección IP del Host Docker en esta red. Además podemos ver que la configuración DNS, que se guarda en el fichero `/etc/resolv.conf`, es la misma que la del Host Docker.

```bash
# cat /etc/resolv.conf
```

En el Host Docker podemos comprobar que se ha creado un Linux Bridge al que esta conectado el Host Docker y el contenedor, esta instrucción la podemos ejecutar en otra terminal:

```bash
$ ip a
...
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:09:f8:81:32 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
...
5: vethe6b3199@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
...
```

### Conectividad del contenedor

Podemos comprobar que el contenedor tiene acceso al exterior:

```bash
# ping www.docker.com
PING www.docker.com (141.193.213.21): 56 data bytes
64 bytes from 141.193.213.21: seq=0 ttl=53 time=10.403 ms
```

Además si instalamos un servidor web podemos acceder utilizando el puerto que hemos mapeado:

```bash
# apk add apache2
# httpd -D foreground
```

Desde el Host Docker podemos probar el acceso:

```bash
$ curl http://localhost:8080
<html><body><h1>It works!</h1></body></html>
```

Vamos a comprobar la configuración de cortafuegos que se ha configurado en el Host Docker. 

* Para permitir que los contenedores conectados a la red **bridge** por defecto tengan conectividad al exterior tenemos que hacer una regla NAT, más concretamente SNAT. 
* Cuando hemos mapeado el puerto 8080/tcp del Host Docker al puerto 80/tcp del contenedor, se ha creado una regla NAT, en concreto DNAT, que hace que todas las peticiones al puerto 8080/tcp del Host Docker se redirijan al puerto 80/tcp del contenedor. Veamos estas reglas iptables, en el Host Docker ejecutando:

```bash
$ sudo iptables -L -n  -t nat
... 
Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination         
MASQUERADE  all  --  172.17.0.0/16        0.0.0.0/0 
...
Chain DOCKER (2 references)
target     prot opt source               destination         
DNAT       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:8080 to:172.17.0.2:80
...
```

La primera es la regla SNAT que permite a todos los contenedores de la red `172.17.0.0/16` tener acceso al exterior, y la segunda es la regla DNAT que permite la redirección de las peticiones al puerto 8080/tcp del Host Docker al puerto 80/tcp del contenedor que esta en la dirección `172.16.0.2`.

## Mapeo de puertos

Para terminar este punto, vamos a ver distintas opciones para mapear los puertos en las creación de un contenedor. Como sabemos usamos el parámetro `-p` o `--publish` en el comando `docker run`, veamos algunos ejemplos de las configuraciones que podemos hacer:

* `-p 8080:80`: Mapea el puerto 8080/tcp en el Host Docker al puerto 80/tcp en el contenedor.
* `-p 192.168.1.100:8080:80`: Asigna el puerto 8080/tcp en el Host Docker accediendo a la IP `192.168.1.100` al puerto 80/tcp en el contenedor.
* `-p 8080:80/udp`: Asigna el puerto 8080/tcp del Host Docker al puerto 80/udp del contenedor.
* `-p 8080:80/tcp -p 8080:80/udp`: Mapea el puerto 8080/tcp en el Host Docker al puerto 80/tcp en el contenedor, y mapea el puerto 8080/udp en el Host Docker al puerto 80/udp en el contenedor.

Por ejemplo este contenedor sólo sería accesible desde el Host Docker:

```bash
$ docker run -d -p 127.0.0.1:8081:80 --name contenedor2 nginx
```

## Conectividad entre los contenedores conectados a la red por defecto

Evidentemente los contenedores conectados a la red pode defecto podrán comunicarse usando su dirección IP, sin embargo esta red no ofrece ningún mecanismo de DNS para que podamos conectarnos a otro contenedor usando su nombre. Veamos un ejemplo con los contenedores creados en este apartado:

En primer lugar vamos a averiguar que dirección IP ha tomado el `contenedor2`:

```bash
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' contenedor2
172.17.0.3
```

A continuación desde el `contenedor1`intentamos conectamos al segundo contenedor:

```bash
$ docker attach contenedor1
/ # ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3): 56 data bytes
...

/ # ping contenedor2
ping: bad address 'contenedor2'
```

Como podemos observar tenemos conectividad desde el primer contenedor al segundo, pero sólo usando la dirección IP, no tengo un DNS que me permita conectarme al segundo contenedor utilizando su nombre.

Para terminar, indicar que existe una opción que ya se considera obsoleta ("deprectated") usando el parámetro `--link` para que los contenedores conectados a la red pode defecto se puedan comunicar usando sus nombres. Sin embargo con esta opción se utilizaría resolución estática, es decir se modificarían los ficheros `/etc/hosts` de los contenedores para que puedieran resolver los nombres de los otros contenedores.
