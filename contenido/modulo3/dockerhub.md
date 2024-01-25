# Registro de imágenes: Docker Hub

## Docker Hub

[**Docker Hub**](https://hub.docker.com/) es un registro público de imágenes Docker.

![ ](img/docker2.png)

## Conceptos de Docker Hub

* **Repositorio**: Un repositorio nos permite guardar una imagen. De cada imágenes podemos tener distintas versiones indicada con su etiqueta.
* **Usuarios**: Nos podemos dar de alta en **Docker Hub** para subir y distribuir nuestras imágenes.
* El nombre de una imagen: **usuario/nombre:etiqueta**.
* Las imágenes oficiales en Docker Hub no tienen nombre de usuario.

## Tipos de imágenes

Podemos encontrar con varios tipos de imágenes,s egún lo que nos ofrece:

* Imágenes que nos ofrecen una **distribución completa de un sistema operativo** (Ubuntu, CentOs, Debian, Fedora....). La distribución **alpine** se empaquetan en imaǵenes pequeños, que sólo incluyen los elementos esenciales necesarios para ejecutar una aplicación. Además nos podemos encontrar imágenes de este tipo con la etiqueta **slim**, en este caso serán imágenes más livianas.
    * Ejemplo: **debian:bookworm**, **debian:bookworm-slim**, **ubuntu:22.04**, **alpine:3**.
* Imágenes que nos ofrecen distintos **servidores** (servidor web, servidor de base de datos,...). En este caso las etiquetas suelen indicar la versión y el sistema operativo base que ofrece el servicio.
    * Ejemplo: **http:2.4-bookworm**, **http:2.4-alpine**, **mariadb:11.2-jammy**, **mariadb:10.6-focal**.
* Imágenes que ofrecen **lenguajes de programación** (php, python, java, nodejs,...). En este caso la etiqueta nos puede indicar la versión, el sistema operativo base que se utiliza, el servicio que se está ofreciendo.
    * Ejemplo: **php:bbookworm**, **php:fpm-bookworm**, **python:3.12-slim-bookworm**, **openjdk:23-ea-6-jdk-bookworm**.
* Imágenes que ofrecen un CMS completo (WordPress, NextCloud, Drupal,...). Como en el caso anterior, las etiquetas nos informan de la versión, del sistema operativo, del servicio ofrecido,...
    * Ejemplos: **wordpress:6.4.2-php8.1-apache**, **wordpress:6.4.2-php8.1-fpm**, **wordpress:6.4.2-fpm-alpine**.


## Contenido de confianza

![ ](img/official-image-badge-iso.png)

* **Imágenes oficiales Docker**: Son mantenidas y distribuidas directamente por Docker, Inc. Son confiables, bien mantenidas y son una opción segura para utilizar en entornos de producción.
* **Editor verificado Docker**
* **Imágenes de código abierto patrocinadas por Docker**
