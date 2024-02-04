# Introducción a las redes en Docker

Cada vez que creamos un contenedor, esté se conecta a una red virtual y Docker hace una configuración del sistema (usando puentes virtuales e iptables) para que la máquina tenga una ip interna, tenga acceso al exterior, podamos mapear (DNAT) puertos,...
Por lo tanto, los contenedores tienen la capacidad de conectarse a otros contenedores y realizar conexiones a servicios de internet. 

## Tipos de redes

* **Red bridge**: Por defecto los contenedores que creamos se conectan a la red de tipo bridge llamada **bridge**:
    * Se crea en el host un *Linux Bridge* llamado **docker0**.
    * El direccionamiento de esta red es 172.17.0.0/16.
    * Usamos el parámetro `-p` en `docker run` para exponer algún puerto. Se crea una regla DNAT para tener acceso al puerto.

    Además de la red por defecto **bridge** podremos crear **redes bridge definidas por el usuario**.

    Este tipo de red nos van a permitir:

    * Aislar los distintos contenedores que tengo en distintas redes Docker, de tal manera que desde cada una de las sredes solo podremos acceder a los equipos de esa misma red.
    * Aislar los contenedores del acceso exterior.
    * Publicar servicios que tengamos en los contenedores mediante redirecciones que docker implementará con las pertinentes reglas DNAT de iptables.

* **Red host**: Si conecto un contenedor a la red **host**, el contenedor ofrece el servicio que tiene configurado en el puerto de la red del anfitrión. No tiene ip propia, sino es cómo si tuviera la ip del anfitrión. Por lo tanto, los puertos son accesibles directamente desde el host.
* **Red none**: La red **none** no configurará ninguna IP para el contenedor y no tiene acceso a la red externa ni a otros contenedores. Tiene la dirección loopback y se puede usar para ejecutar trabajos por lotes.
* Existen otros tops de redes para configuraciones avanzadas: -**overlay**, **ipvlan** y **macvlan**.

## Redes definidas por defecto

Al instalar Docker tenemos una red de cada tipo de las vistas anteriormente:

```bash
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
e77b9d55f17a   bridge    bridge    local
aae71c859f9b   host      host      local
211d98d9ac01   none      null      local
```

## Redes bridge

Como hemos indicado tenemos que realizar una diferencia entre:

* La red **bridge** creada por defecto por Docker para que de forma predeterminada loos contenedores tengan conectividad.
* Y las **redes bridge definidas por el usuario**.

## Diferencias entre la red bridge por defecto y las definidas por el usuario

* Las redes que nosotros definamos proporcionan **resolución DNS** entre los contenedores, cosa que la red por defecto no hace a no ser que usemos opciones que ya se consideran obsoletas ("deprectated") (`--link`).
* Puedo **conectar en caliente** a los contenedores redes “bridge” definidas por el usuario. Si uso la red por defecto tengo que parar previamente el contenedor.
* Me permite **gestionar de manera más segura el aislamiento de los contenedores**, ya que si no indico una red al arrancar un contenedor éste se incluye en la red por defecto donde pueden convivir servicios que no tengan nada que ver.
* Tengo **más control sobre la configuración de las redes** si las defino yo. Los contenedores de la red por defecto comparten todos la misma configuración de red (MTU, reglas ip tables etc...).
* Los contenedores dentro de la red **bridge** por defecto comparten todos ciertas variables de entorno lo que puede provocar ciertos conflictos.
* Es importante que nuestro contenedores en producción se estén ejecutando sobre una red definida por el usuario.

