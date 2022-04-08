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
Lo segundo que hay que tener instalado es [git](https://git-scm.com/). Igual que Ruby, hay [installers para windows](https://gitforwindows.org/),
o se lo puede bajar desde `brew`, `apt-get` o `pacman` (etc).

Desde la [página de GitHub](github.com) podemos crear un repositorio nuevo (antes hay que crearse un usuario, eso queda como ejercicio para el lector). En la esquina superior derecha, al lado de nuestra imagen de perfil, hay un signo `+`:

![Menú GitHub](https://mokocchi.github.io/assets/images/2022-04-08-Blog-Jekyll/github-menu.png)





