# Uso de la red bridge por defecto

Hasta ahora todos los contenedores lo hemos conectado a la red **bridge** por defecto. Como ya hemos dicho las caracteristicas más importantes de este tipo de red son las siguientes:
    * Se crea en el host un *Linux Bridge* llamado **docker0**.
    * El direccionamiento de esta red es 172.17.0.0/16.
    * Usamos el parámetro `-p` en `docker run` para exponer algún puerto. Se crea una regla DNAT para tener acceso al puerto.
    * Los contenedores conectados a un red **bridge** tiene acceso a internet por medio de una regla SNAT.

## Ejemplo de uso de la red bridge por defecto

Vamos a crear un contenedor conectado a la red **bridge** por defecto a partir de la imagen `alpine`, aunque no ofrece un servidor vamos a mapear el puerto 80 para comprobar su funcionamiento:

```bash
docker run -it -p 8080:80 --name cont1 alpine ash
```

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

Hemos visto que su dirección IP está en la red `172.17.0.0/16` y la puerta de enlace es la `172.17.0.1` que corresponde al host Docker. Además podemos ver que la configuración DNS que se guarda en el fichero `/etc/resolv.conf` es el mismo que el del host Docker.



En el host Docker podemos comprobar que se ha creado un Linux Bridge al que esta conectado el host Docker y el contenedor:

```bash
$ ip a
...
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:09:f8:81:32 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
```

