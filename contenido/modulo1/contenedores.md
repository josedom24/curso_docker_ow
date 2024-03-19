# Introducción a los contenedores

Según la Wikipedia: *La virtualización utiliza el software para imitar las características del hardware y crear un sistema informático virtual*.

Esto nos permite ejecutar más de un sistema virtual, y múltiples sistemas operativos y aplicaciones, en un solo servidor, aumentando el rendimiento del hardware disponible e incrementando el tiempo de procesamiento de un equipo, ya que habitualmente se desaprovecha gran parte.

## ¿Para qué se utiliza la virtualización?

* Aislamiento e independencia de servicios y contenidos.
* Laboratorio de pruebas.
* Virtualización de arquitecturas de las que no se dispone.
* Creación de clúster de máquinas y sistemas distribuidos.
* Herramienta de aprendizajes

## Ventajas e inconvenientes de la virtualización

Las principales ventajas que podemos indicar serían:

* Importante ahorro económico.
* Seguridad.
* Mayor aprovechamiento de recursos.
* Migración en vivo. 
* Importante ahorro energético.

Como desventajas podríamos señalar:

* Muchos sistemas dependen de un solo equipo físico.
* Penalizaciones en rendimiento.

## Tipos de virtualización

* **Emulación**: El sistema de virtualización imita o suplanta vía software una arquitectura al completo (procesador, memoria, conjunto de instrucciones, comunicaciones...). Ejemplo: QEMU, Microsoft Virtual PC, Wine, ...
* **Virtualización por hardware de tipo 1**: Simula un hardware suficiente para permitir que un sistema operativo no adaptado se ejecute de forma aislada. Controla directamente el hardware físico del host ofreciéndolo directamente a la máquina virtual. Ejemplos: Xen, Kernel-based Virtual Machine (KVM), Microsoft Hyper-V, VMware ESXi,...
* **Virtualización completa de tipo 2**: En este caso este software no controla directamente el hardware físico, por lo que el rendimiento puede ser menor que el anterior. Ejemplos: VMware Workstation, Parallels Desktop, VirtualBox, VMware Player, ...
* **Virtualización ligera**: O también llamada **virtualización a nivel de sistema operativo**, o **virtualización basada en contenedores**. Es un método de virtualización en el que, sobre el núcleo del sistema operativo se ejecuta una capa de virtualización que permite que existan múltiples instancias aisladas de espacios de usuario. A cada espacio de usuario aislado lo llamamos **contenedor**.

## Contenedores

Un contenedor es un conjunto de procesos aislados, que se ejecutan en un servidor, y que acceden a un sistema de ficheros propio, tienen una configuración de red propia y acceden a los recursos del host (memoria y CPU). Utilizan para su funcionamiento el núcleo del host donde se ejecutan.

Podemos hacer la siguiente clasificación de contenedores:

* **Contenedores de Sistemas**: El uso que se hace de ellos es muy similar al que hacemos sobre una máquina virtual: se accede a ellos (normalmente por ssh), se instalan servicios, se actualizan, ejecutan un conjunto de procesos, ... Ejemplo: LXC(Linux Container).
* **Contenedores de Aplicación**: Se suelen usar para el despliegue de aplicaciones web. Ejemplo: Docker, Podman, ...

## Contenedores y aplicaciones

¿Qué aplicaciones web son más idóneas para desplegar en contenedores?

* Si tenemos aplicaciones monolíticas, vamos a usar un esquema multicapa, es decir cada servicio (servicio web, servicio de base de datos, ... ) se va a desplegar en un contenedor.
* Realmente, las aplicaciones que mejor se ajustan al despliegue en contenedores son la desarrolladas con microservicios:
    * Cada componente de la aplicación (“microservicio”) se puede desplegar en un contenedor.
    * Comunicación vía HTTP API REST y colas de mensajes.
    * Facilita enormemente las actualizaciones de versiones de cada componente.
    * ...

## Ventajas del uso de contenedores de aplicaciones

1. **Portabilidad**: Los contenedores encapsulan una aplicación y todas sus dependencias de manera aislada. Esto facilita la portabilidad, ya que los contenedores pueden ejecutarse de manera consistente en diferentes entornos, como desarrollo, pruebas y producción.
2. **Aislamiento**: Los contenedores proporcionan un nivel de aislamiento entre la aplicación y el sistema operativo del anfitrión. Esto asegura que las aplicaciones se ejecuten sin interferencias con otras aplicaciones o componentes del sistema, evitando conflictos de dependencias y problemas de compatibilidad.
3. **Eficiencia en el uso de recursos**: Al compartir el núcleo del sistema operativo y solo incluir las bibliotecas y dependencias necesarias, los contenedores son más eficientes en términos de recursos en comparación con las máquinas virtuales. Esto permite una utilización más efectiva de los recursos del sistema y una mayor densidad de aplicaciones por servidor.
4. **Despliegue rápido**: Los contenedores permiten la creación, el despliegue y la escalabilidad rápida de aplicaciones. La capacidad de implementar contenedores en cuestión de segundos o minutos facilita la entrega continua y el despliegue ágil de aplicaciones.
