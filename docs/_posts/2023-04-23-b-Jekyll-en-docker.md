---
layout: post
title: "Jekyll local con Docker"
date: 2023-04-23 21:00:00 -0300
categories: blog, docker
author: Jose Arcidiacono (@mokocchi)
permalink: 2023-04-23-b-Jekyll-en-docker
---

Hoy voy a contar cómo hago para correr Jekyll local en mi Debian usando Docker.

Recordemos que Jekyll es un generador de sitios estáticos que usa GitHub en GitHub Pages para servir sitios como blogs ([Post sobre Jekyll y GHPages](2022-04-08b-Blog-Jekyll)).

## El problema

Instalé un montón de veces Ruby, de varias formas distintas, pero las variantes se reducen a estas categorías:

1. Ruby 3.2+ con el OpenSSH v3 del sistema: todo anda bien. El programa corre.
2. Ruby anterior con el OpenSSH v3 del sistema: se rompe porque ruby no espera la versión 3 de OpenSSH.
3. Ruby anterior, con OpenSSH v1 compilado por mí: problemas de certificados. No puedo conectarme a ningún lado.
4. Ruby anterior, con OpenSSH v1 que trae rvm: segmentation fault por todos lados.

## Una posible solución

Después de renegar durante días con este problema, me di cuenta de que los programas que quería correr eran aplicaciones web, es decir, que no necesitaban el escritorio sino un puerto.

Esto quiere decir que podría correrlas dentro de Docker y exponer únicamente el puerto que necesita.

Lo bueno de docker es que tiene un ambiente congelado en el tiempo con todas las dependencias correctas, y me permite correr versiones viejas de ruby sin ensuciar mi máquina.

## Dockerizando Jekyll con un wrapper

Al final lo que hice fue levantar un contenedor con ruby2.7, que es la versión que usa GitHub.

Para esto necesito una versión de ruby que no se cierre. Esto lo logro con el siguiente Dockerfile:

```dockerfile
  FROM ruby:2.7

  WORKDIR /app
  
  CMD ["tail", "/dev/null"]
```

Por qué no hago el install y el serve acá? Primero, no tengo los archivos para hacer el install en tiempo de build. Por otra parte, si el comando que se corre es `jekyll s` (servir el sitio desde jekyll), cada vez que falla el comando (por un error en scss por ejemplo) tengo que volver a iniciar el conteenedor.

Para construir la imagen, el comando que uso es:

```shell
docker image build -t ruby27 .
```

El siguiente paso es compartir los archivos fuente con el contenedor. No creamos la imagen con los archivos adentro porque queremos hacer hot reloading de lo que vamos modificando.

```shell
docker run -d \
  --name jekyll \
  --mount type=bind,source="$(pwd)"/docs,target=/app \
  -p 4000:4000 \
  ruby27:latest
```

Línea por línea:

1. Creamos un contenedor de docker en modo detached, es decir que la salida del contenedor no estará conectada a la consola
2. le damos un nombre para identificarlo. Probablemente tengamos un contenedor distinto para cada blog o sitio que queramos servir
3. montamos el directorio donde esta nuestro código en /app
4. exponemos el puerto 4000 del contenedor en el 4000 de nuestro host
5. usamos la imagen que creamos con el Dockerfile anterior

Una vez que tenemos el contenedor corriendo hay que correr este otro comadno:

```shell
docker container exec -it jekyll bash
```

Lo que hace este comando es abrirnos una consola bash (root e interactiva) dentro del contenedor que creamos antes y se llama `jekyll`.

Una vez adentro, tenemos que instalar todo con

```shell
bundle install
```

Cuando está todo instalado (esto es necesario una única vez) pasamos a servir el sitio localmente:

```shell
bundle exec jekyll s --host 0.0.0.0
```

Es importante el parámetro `--host`, porque si no está no se abre el puerto hacia el exterior.

Deberíamos ver algo como lo siguiente:

```log
Server address: http://0.0.0.0:4000//
Server running... press ctrl-c to stop.
```

Entonces podemos ir a nuestro navegador, y entrar a [localhost:4000](http://localhost:4000) para ver nuestro sitio corriendo.

## Conclusión

Un wrapper es una buena forma de correr jekyll sin tener que ensuciar nuestro sistema con versiones viejas de ruby.

