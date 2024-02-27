# Introducción a las redes en Docker

Cada vez que creamos un contenedor, esté se conecta a una red virtual y Docker hace una configuración del sistema (usando puentes virtuales e iptables) para que la máquina tenga una dirección IP interna, tenga acceso al exterior, podamos mapear (DNAT) puertos,...
Por lo tanto, los contenedores tienen la capacidad de conectarse a otros contenedores y realizar conexiones a servicios de internet. 

## Tipos de redes

* **Red bridge**: Nos permite que los contenedores estén conectados a una red privada, con un direccionamiento privado conectado a la Host Docker mediante un Linux Bridge. 
    * Nos permiten aislar los contenedores del acceso exterior.
    * Los contenedores conectados a un red **bridge** tienen acceso a internet por medio de una regla SNAT. 
    * Usamos el parámetro `-p` en `docker run` para exponer algún puerto. Se crea una regla DNAT para tener acceso al puerto.
* **Red host**: Si conecto un contenedor a la red **host**, el contenedor ofrece el servicio que tiene configurado en el puerto de la red del Host Docker. No tiene dirección IP propia, sino es cómo si tuviera la dirección IP del Host Docker. Por lo tanto, los puertos son accesibles directamente desde el Host Docker.
* **Red none**: La red **none** no configurará ninguna IP para el contenedor y no tiene acceso a la red externa ni a otros contenedores. Tiene la dirección loopback y se puede usar para ejecutar trabajos por lotes.
* Existen otros tipos de redes para configuraciones avanzadas: **overlay**, **ipvlan** y **macvlan**.

## Redes definidas en la instalación de Docker

Al instalar Docker tenemos una red de cada tipo de las vistas anteriormente:

```bash
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
e77b9d55f17a   bridge    bridge    local
aae71c859f9b   host      host      local
211d98d9ac01   none      null      local
```

## Redes bridge

Existen dos tipos de redes bridge:

* La red **bridge** creada por defecto por Docker para que de forma predeterminada los contenedores tengan conectividad.
* Y las **redes bridge definidas por el usuario**.

### Red bridge por defecto

* Por defecto los contenedores que creamos se conectan a la red de tipo bridge llamada **bridge**.
* Se crea en el Host Docker un *Linux Bridge* llamado **docker0**.
* El direccionamiento de esta red es 172.17.0.0/16.

![ ](img/bridge1.png)

### Red bridge definida por el usuario

* Nos permiten aislar los distintos contenedores que tengo en distintas redes Docker, de tal manera que desde cada una de las redes solo podamos acceder a los equipos de esa misma red.
* Nos proporcionan **resolución DNS** entre los contenedores, por lo que los contenedores puedan conectar a otros contenedores usando su nombre.
* Puedo **conectar en caliente** a los contenedores redes bridge definidas por el usuario. Si uso la red por defecto tengo que parar previamente el contenedor.
* Me permiten **gestionar de manera más segura el aislamiento de los contenedores**, ya que si no indico una red al arrancar un contenedor éste se incluye en la red por defecto donde pueden convivir servicios que no tengan nada que ver.
* Nos proporcionan **más control sobre la configuración de las redes**. Los contenedores de la red por defecto comparten todos la misma configuración de red (MTU, reglas de cortafuegos, etc...).
* Es importante que nuestros contenedores en producción se ejecuten conectados a una red bridge definida por el usuario.

![ ](img/bridge2.png)

