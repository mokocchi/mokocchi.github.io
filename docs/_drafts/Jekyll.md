---
layout: post
title: "Cómo hacer un blog en Jekyll"
date: 2022-04-08 16:00:00 -0300
categories: meta, blog
author: Jose Arcidiacono (@mokocchi)
permalink: 2022-04-08-Blog-Jekyll
---

A pedido del público, hoy voy a hacer un tutorial de cómo hacer un blog como este mismo.

La herramienta que uso se llama [Jekyll](https://jekyllrb.com/) y está hecha en lenguaje Ruby.

![Logo Jekyll](https://mokocchi.github.io/assets/images/2022-04-08-Blog-Jekyll/jekyll-logo.png)

El código lo subo a [GitHub](github.com/), que es un almacenamiento en la nube para repositorios Git. Un repositorio git es básicamente una base de datos en directorios que guarda distintas versiones del código. Otro día hago un post sobre git.

Github tiene un hosting gratuito para páginas que se llama [GitHub Pages](https://pages.github.com/). Puede correr código hecho en HTML simple, en frameworks como React, o Jekyll, su motor incorporado de plantillas.

Lo interesante que tiene Jekyll es que, en teoría, se puede levantar un sitio con sólo un par de comandos.

## Instalando Jekyll
Primero que nada tenemos que instalar [Ruby](https://www.ruby-lang.org/).

Chequeamos si ya lo tenemos con `ruby -v`.

Si no, lo instalamos con un installer (Windows) o usando nuestro gestor de paquetes (`brew` en macOS, `apt-get` 
para Debian/Ubuntu, `pacman` para Arch Linux, etc). Las instrucciones están en [este link](https://www.ruby-lang.org/es/documentation/installation/).

Después tenemos que instalar [Bundler](https://bundler.io/), una herramienta que nos ayuda a instalar "gemas" (las librerías de Ruby).
Teniendo Ruby instalado, basta con correr el siguiente comando:
```
gem install bundler
```
Después instalamos Jekyll propiamente dicho:
```
gem install jekyll
```
## Preparando el repositorio
Lo próximo que hay que tener instalado es [git](https://git-scm.com/). Igual que Ruby, hay [installers para windows](https://gitforwindows.org/),
o se lo puede bajar desde `brew`, `apt-get` o `pacman` (etc).

Desde la [página de GitHub](github.com) podemos crear un repositorio nuevo (antes hay que crearse un usuario, eso queda como ejercicio para el lector). En la esquina superior derecha, al lado de nuestra imagen de perfil, hay un signo `+`, y en el menú que se despliega hay que elegir "New repository".

![Menú GitHub](https://mokocchi.github.io/assets/images/2022-04-08-Blog-Jekyll/github-menu.png)

Elegimos un nombre para el repositorio (se va ver en la URL del blog) y le ponemos una descripción: 

![Repositorio nuevo](https://mokocchi.github.io/assets/images/2022-04-08-Blog-Jekyll/new-repository.png)

Opcionalmente se puede agregar:
- un README.md, archivo que explica de qué trata el repositorio a la gente que lee el código
- un .gitignore, archivo donde se lista lo que no queremos sincronizar con el repositorio (cache, por ejemplo)
- una licencia, para indicar a quienes usen nuestro repositorio qué derechos tienen (compartir, modificar, etc)

![Repositorio nuevo 2](https://mokocchi.github.io/assets/images/2022-04-08-Blog-Jekyll/new-repository-2.png)

Yo en este caso elegí la licencia de Mozilla y un README.md.

Después tenemos que clonar (descargar) el repositorio en nuestra máquina. Hay tres opciones: HTTPS (requiere contraseña o personal access token), SSH (requiere una clave SSH registrada), GitHub CLI (tiene que estar instalado pero permite loguearse desde el navegador) o simplemente bajar un ZIP.

Yo voy a clonarlo con SSH. Configurarlo es una larga historia para otro día.

![Clonando repositorio](https://mokocchi.github.io/assets/images/2022-04-08-Blog-Jekyll/clone-1.png)

Para clonarlo hay que abrir una terminal en el directorio donde queremos guardarlo y ejecutar el siguiente comando:

```
git clone <dirección copiada de GH>
```
Esta es mi salida:
```
$ git clone git@github.com:mokocchi/jekyll-demo.git
Cloning into 'jekyll-demo'...
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (4/4), 5.86 KiB | 5.86 MiB/s, done.
```
Cambiamos al directorio de nuestro repositorio:

```
cd <repositorio>
```
y creamos un directorio que se llame `docs`:
```
mkdir docs
```
Cambiamos al directorio `docs`:
```
cd docs
```
## Creando el proyecto
Cambiamos con `git` a una rama `gh-docs`:
```
git checkout --orphan gh-pages
```
Y ahora, sí, creamos el proyecto:
```
jekyll new --skip-bundle .
```

