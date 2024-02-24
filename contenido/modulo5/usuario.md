# Redes bridge definidas por el usuario

Además de poder usar la red **bridge** por defecto, podemos crear nuevas redes de este tipo, a las que llamamos **redes bridge definidas por el usuario**. Como hemos visto en la introducción este tipo de redes nos proporcionan un mecanismo **DNS** que nos permite el acceso entre contenedores usando su nombre. Este tipo de redes serán las deseadas en entornos de producción.

## Gestión de redes bridge definidas por el usuario

Para crear una red bridge definida por el usuario, ejecutamos la siguiente instrucción: 

```bash
$ docker network create -d bridge red1
```

La opción `-d bridge` es optativa, si no se se indica el tipo,  por defecto se creará una red de tipo bridge. Podemos visualizar las redes que tenemos creadas ejecutando:

```bash
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
...
6dc4142f78ed   red1      bridge    local
```

Para obtener información de la red, podemos ejecutar:

```bash
$ docker network inspect red1
...
    "Config": [
        {
            "Subnet": "172.18.0.0/16",
            "Gateway": "172.18.0.1"
        }
...
```

Al crear la red no hemos indicado el direccionamiento, por lo que se ha asignado uno por defecto. Además en el Host Docker se ha creado un nuevo bridge donde se conectarán los contenedores que estén conectados a esta red:

```bash
$ ip a
...
17: br-6dc4142f78ed: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    inet 172.18.0.1/16 brd 172.18.255.255 scope global br-6dc4142f78ed
...
```

Vemos que la puerta de enlace que recibirán los contenedores conectados a esta red corresponde con la dirección IP del Host Docker en esta red.

Por último, para borrar una red podemos ejecutar:

```bash
$ docker network rm red1
```

Teniendo en cuenta que no puedo borrar una red que tenga contenedores que la estén usando, deberé primero borrar los contenedores o desconectarlos de la red.

## Creación avanzada de redes bridge definidas por el usuario

En la creación de una red de tipo bridge podemos configurar algunos parámetros, por ejemplo el direccionamiento y la puerta enlace. Veamos un ejemplo:

```bash
$ docker network create --subnet 192.168.0.0/24 --gateway 192.168.0.100 red2
```

Podemos comprobar que el direccionamiento lo hemos configurado:


```bash
$ docker network inspect --format='{{range .IPAM.Config}}{{.Subnet}}{{end}}' red2
192.168.0.0/24
```

Y que la dirección IP que hemos asignado a la puerta de enlace, se ha configurado en la interfaz del Host Docker correspondiente al nuevo bridge virtual que se ha creado:

```bash
$ ip a
18: br-123d3e08ec9c: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    inet 192.168.0.100/24 brd 192.168.0.255 scope global br-123d3e08ec9c
```


