# Los contenedores son efímeros

**Los contenedores son efímeros**, es decir, los ficheros, datos y configuraciones que creamos en los contenedores sobreviven a las paradas de los mismos pero, sin embargo, son destruidos si el contenedor es destruido. 

Veamos un ejemplo:

```bash
$ docker run -d --name my-apache-app -p 8080:80 httpd:2.4
ac50cc24ef71ae0263be7794278600d5cc4f085b88cebbf97b7b268212f2a82f
    
$ docker exec my-apache-app bash -c 'echo "<h1>Hola</h1>" > /usr/local/apache2/htdocs/index.html'
    
$ curl http://localhost:8080
<h1>Hola</h1>
    
$ docker rm -f my-apache-app
my-apache-app
    
$ docker run -d --name my-apache-app -p 8080:80 httpd:2.4
bb94716205c780ec4a3a2695722fb35ac616ae4cea573308d9446208afb164dc
    
$ curl http://localhost:8080
<html><body><h1>It works!</h1></body></html>
```

Vemos como al eliminar el contenedor, la información que habíamos guardado en el fichero `index.html` se pierde, y al crear un nuevo contenedor ese fichero tendrá el contenido original.

Docker nos ofrece diferentes mecanismos para hacer persistentes los contenedores y no perder los datos de nuestras aplicaciones.