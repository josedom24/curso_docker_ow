# Ejemplo 4: Construcción de imágenes configurables con variables de entorno

En este ejemplo vamos a construir una imagen de una aplicación PHP que necesita conectarse a una base de datos MariaDB para guardar o leer información. Por lo tanto, vamos a construir la imagen que tendrá distintas variables de entorno para configurar las credenciales de acceso a la base de datos. 

Puedes encontrar los ficheros necesarios en el [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_docker_ow).

## Aplicación PHP

Como ejemplo vamos a "dockerizar" una aplicación PHP simple que accede a una tabla de una base de datos. La aplicación la puedes encontrar en el directorio `build/app/index.php`.

Algunas cosas que hay que tener en cuenta:

* Cuando programamos una aplicación tenemos que tener en cuenta que va a ser implantada usando Docker tenemos que hacer algunas modificaciones, por ejemplo en este caso, las credenciales para el acceso a la base de datos la leemos de variables de entorno (que posteriormente serán creadas en el contenedor):

```php
<?php
 // Database host
 $host = getenv('DB_HOST');
 // Database user name
 $user = getenv('DB_USER');
 //Database user password
 $pass = getenv('DB_PASS');
 //Database name
 $db = getenv('DB_NAME');
 // check the MySQL connection status
 $conn = new mysqli($host, $user, $pass,$db);
 if ($conn->connect_error) {
     die("Connection failed: " . $conn->connect_error);
 } else {
     $sql = 'SELECT * FROM users';
     
     if ($result = $conn->query($sql)) {
         while ($data = $result->fetch_object()) {
             $users[] = $data;
         }
     }
     
     foreach ($users as $user) {
        echo "<br>";
        echo $user->username . " " . $user->password;
        echo "<br>";
    }
 }
 mysqli_close($conn);
 ?>
```

* En el fichero `schema.sql` encontramos las instrucciones sql necesarias para inicializar la base de datos.

## Configurar nuestra aplicación con variables de entorno

En los casos en que necesitamos modificar algo en la aplicación o hacer algún proceso en el momento de crear el contenedor, lo que hacemos es crear un script en bash que copiaremos en la imagen y que será la instrucción que indiquemos en el `CMD`. Este script en concreto hará las siguiente operaciones:

1. Utilizando el fichero `schema.sql` (que también guardaremos en la imagen) inicializará la base de datos.
2. Ejecutar el servidor web en segundo plano.

En el directorio de trabajo encontramos:

* `build`: Será el contexto necesario para crear la imagen de la aplicación.
* El fichero `compose.yaml`: Para crear el escenario.

## El contexto (directorio build)

En el directorio de contexto tendremos tres ficheros:

### Fichero script.sh

El fichero `script.sh` que se guardará en la imagen y se ejecutará con al iniciar el contenedor. Su contenido es el siguiente:

```bash
#!/bin/bash
while ! mysql -u ${DB_USER} -p${DB_PASS} -h ${DB_HOST}  -e ";" ; do
	sleep 1
done	
mysql -u ${DB_USER} -p${DB_PASS} -h ${DB_HOST} ${DB_NAME} < /opt/schema.sql
apache2ctl -D FOREGROUND
```

Esperamos hasta que la base de datos esté disponible, inicializamos la base de datos con el fichero `schema.sql` y finalmente iniciamos el servidor web.

### Fichero schema.sql

Son las instrucciones sql que nos permiten crear la tabla necesaria en la base de datos.

### Fichero Dockerfile

El fichero`Dockerfile` sería el siguiente:

```Dockerfile
# syntax=docker/dockerfile:1
FROM php:7.4-apache
RUN apt-get update && apt-get install -y mariadb-client
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
COPY app /var/www/html/
EXPOSE 80
ENV DB_USER user1
ENV DB_PASS asdasd
ENV DB_NAME usuarios
ENV DB_HOST mariadb
COPY script.sh /usr/local/bin/script.sh
COPY schema.sql /opt
RUN chmod +x /usr/local/bin/script.sh
CMD /usr/local/bin/script.sh

```

Algunas observaciones:

1. Creamos la imagen desde una imagen PHP.  
2. Instalamos el cliente de MariaDB que nos hará falta para inicializar la base de datos desde nuestro contenedor de la aplicación.
3. Siguiendo la documentación de la [imagen oficial de PHP](https://hub.docker.com/_/php) instalamos el módulo PHP `mysqli`.
4. Copiamos la aplicación en el servidor web.
6. Creamos las variables de entorno y le damos valores por defecto, por si no se indican en la creación del contenedor.
7. Copiamos los ficheros del script y del esquema de la base de datos a la imagen. Y le damos permisos de ejecución a `script.sh`.
8. Finalmente indicamos con `CMD` el comando que se va a ejecutar al iniciar el contenedor. En este caso ejecutaremos el script.

### Creación de la imagen

Ejecutamos dentro del directorio de contexto:

```bash
$ docker build -t josedom24/aplicacion_php .
```

## Despliegue de la aplicación 

Usaremos el fichero `compose.yaml`:

```yaml
version: '3.1'
services:
  app:
    container_name: contenedor_php
    image: josedom24/aplicacion_php
    restart: always
    environment:
      DB_HOST: servidor_mariadb
      DB_USER: user1
      DB_PASS: asdasd
      DB_NAME: usuarios
    ports:
      - 8080:80
    depends_on:
      - db
  db:
    container_name: servidor_mariadb
    image: mariadb
    restart: always
    environment:
      MARIADB_DATABASE: usuarios
      MARIADB_USER: user1
      MARIADB_PASSWORD: asdasd
      MARIADB_ROOT_PASSWORD: asdasd
    volumes:
      - mariadb_data:/var/lib/mysql
volumes:
    mariadb_data:
```

Y ya podemos levantar el escenario, ejecutando:

```bash
$ docker compose up -d
```

Y finalmente podemos acceder a la aplicación y comprobar que funciona.