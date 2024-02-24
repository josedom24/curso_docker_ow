# Ejemplo 4: Despliegue de Apache Tomcat + nginx 

En este ejemplo vamos a desplegar con Docker Compose la aplicación Java con Tomcat y nginx como proxy inverso que vimos en un módulo anterior.

Puedes encontrar el fichero `compose.yaml` en el [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_docker_ow).

El fichero `compose.yaml` sería:

```yaml
version: '3.1'
services:
  aplicacionjava:
    container_name: tomcat
    image: tomcat:9.0
    restart: always
    volumes:
      - ./sample.war:/usr/local/tomcat/webapps/sample.war:ro
  proxy:
    container_name: nginx
    image: nginx
    ports:
      - 80:80
    volumes:
      - ./default.conf:/etc/nginx/conf.d/default.conf:ro
```

Como podemos ver en el directorio donde tenemos guardado el `compose.yaml`, tenemos los dos ficheros: por un lado la aplicación en el fichero `sample.war` y por otro, el fichero de configuración de nginx `default.conf`.

Creamos el escenario:

```bash
$ docker compose up -d
...
```

Comprobar que los contenedores están funcionando:

```bash
$ docker compose ps
...
```

Y acceder al puerto 80/tcp de la dirección IP del Host Docker para acceder a la aplicación.