# ¿Qué es la virtualización?

Según la Wikipedia: *La virtualización utiliza el software para imitar las características del hardware y crear un sistema informático virtual*.

Esto nos permite ejecutar más de un sistema virtual, y múltiples sistemas operativos y aplicaciones, en un solo servidor, aumentando el rendimiento del hardware disponible e incrementando el tiempo de procesamiento de un equipo, ya que habitualmente se desaprovecha gran parte.

## ¿Para qué se utiliza la virtualización?

* Aislamiento e independencia de servicios y contenidos.
* Laboratorio de pruebas.
* Virtualización de arquitecturas de las que no se dispone.
* Creación de clúster de máquinas y sistemas distribuidos.
* Herramientas de aprendizajes

## Ventajas e inconvenientes de la virtualización

Las principales ventajas que podemos indicar serían:

* Importante ahorro económico.
* Seguridad.
* Mayor aprovechamiento de recursos.
* Migración en vivo. 
* Importante ahorro energético.

Como desventajas podríamos señalar:

* Muchos sistemas dependen de un sólo equipo físico.
* Penalizaciones en rendimiento.

## Tipos de virtualización

* **Emulación**: El sistema de virtualización emita o suplanta vía software una arquitectura al completo (procesador, memoria, conjunto de instrucciones, comunicaciones...). Ejemplo: QEMU, Microsoft Virtual PC, Wine, ...
* **Virtualización por hardware de tipo 1**: Simula un hardware suficiente para permitir que un sistema operativo no adaptado se ejecute de forma aislada. Controlan directamente el hardware físico del host ofreciéndolo directamente a la máquina virtual. Ejemplos: Xen, Kernel-based Virtual Machine (KVM), Microsoft Hyper-V, VMware ESXi,...
* **Virtualización completa de tipo 2**: En este caso este software no controla directamente el hardware físico, por lo que el rendimiento puede ser menor que el anterior. Ejemplos: VMware Workstation, Parallels Desktop, VirtualBox, VMware Player, ...
* **Virtualización ligera**: O también llamada **virtualización a nivel de sistema operativo**, o **virtualización basada en contenedores**. Es un método de virtualización en el que, sobre el núcleo del sistema operativo se ejecuta una capa de virtualización que permite que existan múltiples instancias aisladas de espacios de usuario. A cada espacio de usuario aislado lo llamamos **contenedor**.

## Contenedores

un contenedor es un conjunto de procesos aislados, que se ejecutan en un servidor, y que acceden a un sistema de ficheros propio, tienen una configuración red propio y accede a los recursos del host (memoria y CPU).
Podemos hacer la siguiente clasificación de contenedores:

* **Contenedores de Sistemas**: El uso que se hace de ellos es muy similar al que hacemos sobre una máquina virtual: se accede a ellos (normalmente por ssh), se instalan servicios, se actualizan, ejecutan un conjunto de procesos, ... Ejemplo: LXC(Linux Container).
* **Contenedores de Aplicación**: Se suelen usar para el despliegue de aplicaciones web Ejemplo: Docker, Podman, ...

