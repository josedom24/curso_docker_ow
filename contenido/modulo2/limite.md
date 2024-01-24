# Limitando los recursos utilizados por un contenedor docker

Puedes limitar los recursos de CPU y memoria al crear un contenedor en Docker utilizando las opciones --cpus y --memory respectivamente. Aquí te dejo ejemplos de cómo hacerlo:
Limitar la cantidad de CPU:

Para limitar la cantidad de CPU que puede utilizar un contenedor, puedes utilizar la opción --cpus. Por ejemplo, para limitar un contenedor a utilizar 0.5 CPUs (la mitad de un núcleo):

bash

docker run --cpus 0.5 nombre_imagen

Limitar la memoria:

Para limitar la cantidad de memoria que puede utilizar un contenedor, puedes utilizar la opción --memory. Puedes especificar la memoria en bytes, kilobytes, megabytes, gigabytes, o utilizando el formato abreviado con las letras b, k, m, g. Aquí hay un ejemplo para limitar un contenedor a 512 megabytes de memoria:

bash

docker run --memory 512m nombre_imagen

Limitar la memoria y CPU al mismo tiempo:

Puedes combinar ambas opciones si necesitas limitar tanto la memoria como la CPU al mismo tiempo:

bash

docker run --cpus 0.5 --memory 512m nombre_imagen

Estos son solo ejemplos y debes adaptar los valores según los requisitos y las características de tu aplicación. Limitar los recursos es útil para evitar que un contenedor consuma todos los recursos disponibles en el sistema y garantizar un comportamiento más predecible y controlado.




docker stats

docker update


¡Por supuesto! El parámetro --cpus en Docker se utiliza para limitar la cantidad de recursos de CPU que un contenedor puede utilizar. Puedes especificar la cantidad de CPUs que deseas asignar al contenedor, ya sea en términos de núcleos completos o fracciones de núcleos.

A continuación, te proporcionaré algunos ejemplos para ilustrar cómo usar --cpus:
Ejemplo 1: Asignar 1 núcleo completo al contenedor

bash

docker run --cpus=1.0 nombre_imagen

Este comando asigna al contenedor la capacidad de utilizar un núcleo completo de la CPU del sistema.
Ejemplo 2: Asignar medio núcleo al contenedor

bash

docker run --cpus=0.5 nombre_imagen

En este caso, el contenedor solo puede utilizar la mitad de un núcleo de la CPU.
Ejemplo 3: Asignar múltiples CPUs al contenedor

bash

docker run --cpus=2.0 nombre_imagen

Aquí, el contenedor puede utilizar dos núcleos completos.
Ejemplo 4: Asignar una fracción específica de CPU

bash

docker run --cpus=0.75 nombre_imagen

Este comando limita el contenedor a utilizar el 75% de un núcleo de la CPU.

Recuerda que estos son solo ejemplos y debes ajustar la cantidad de CPUs según tus necesidades y los recursos disponibles en tu sistema. También es importante mencionar que --cpus no garantiza que los recursos de CPU estarán dedicados exclusivamente al contenedor, pero limita la cantidad de tiempo de CPU que el contenedor puede utilizar.



--------------

Cuando inspeccionas un contenedor Docker con el comando docker inspect, puedes utilizar el campo NanoCpus para ver la cantidad de CPU asignada al contenedor en unidades de nanocpus. Un nanocpu es una unidad de medida que representa la fracción de tiempo de CPU.

A continuación, te proporciono un ejemplo de cómo puedes usar docker inspect para obtener la información sobre NanoCpus:

bash

docker inspect --format '{{.HostConfig.NanoCpus}}' nombre_contenedor

Este comando mostrará la cantidad de CPU asignada al contenedor en unidades de nanocpus. Ten en cuenta que la interpretación de estos valores puede no ser intuitiva, pero representan la capacidad de procesamiento relativa asignada al contenedor en comparación con la capacidad total del sistema.

Si deseas obtener información más legible, puedes convertir los nanocpus a CPUs utilizando la fórmula:

bash

nano_cpus=$(docker inspect --format '{{.HostConfig.NanoCpus}}' nombre_contenedor)
cpus=$(echo "scale=2; $nano_cpus / 1000000000" | bc)
echo "CPUs asignadas al contenedor: $cpus"

Este fragmento de código convierte los nanocpus a CPUs dividiendo por 1e9 (mil millones) utilizando la herramienta bc (calculadora de precisión arbitraria). La variable cpus contendrá la cantidad de CPUs asignadas al contenedor.


------

Para obtener el límite de memoria asignado a un contenedor Docker en un formato más legible, puedes usar el comando docker inspect con el filtro --format y el campo HostConfig.Memory.

A continuación, te muestro un ejemplo de cómo hacerlo:

bash

docker inspect --format '{{.HostConfig.Memory}}' nombre_contenedor

Este comando te dará el límite de memoria en bytes. Si deseas convertirlo a un formato más humano, puedes hacerlo utilizando comandos adicionales en Bash. Por ejemplo:

bash

memory_limit=$(docker inspect --format '{{.HostConfig.Memory}}' servidor_web)
memory_limit_mb=$(echo "scale=2; $memory_limit / 1048576" | bc)
echo "Límite de memoria asignado al contenedor: $memory_limit_mb MB"

En este fragmento de código, estoy dividiendo el límite de memoria en bytes por 1048576 (que es 1024 al cubo) para convertirlo a megabytes. Ajusta según sea necesario para tus necesidades específicas.