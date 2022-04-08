---
layout: post
title: "Cómo hacer un blog en Jekyll"
date: 2022-04-08 16:00:00 -0300
categories: meta blog
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
El argumento `--skip-bundle` saltea la instalación, solo genera los archivos
(porque va a correr en el servidor de GitHub).

***
Si por alguna razón no pueden instalar Jekyll, pueden partir de los archivos en la rama `jekyll-config` de mi repositorio [jekyll-demo](https://github.com/mokocchi/jekyll-demo) copiando la carpeta `docs`:

```
git clone <link de mi repositorio copiado de GitHub>

cd jekyll-demo

git checkout jekyll-config
```
***
## Configuración de Jekyll

Hay dos archivos para editar:
- `Gemfile`: hay que comentar (poniendo un `#` al inicio de la línea) o borrar la línea que dice `gem "jekyll"...` y agregar esta línea:
```
gem "github-pages", "~> 225", group: :jekyll_plugins
```
- `_config.yml`: hay que editar los campos entre <picos>:
```
title: <titulo del blog>
email: <tu mail>
description: >-
  <Una descripción 
  de varias líneas
  acerca de tu blog.>
baseurl: "/tu-repositorio"
domain: tu-usuario.github.io
url: "http://tu-usuario.github.io/"
twitter_username: <tu twitter>
github_username:  <tu github>

theme: <tu tema elegido>
plugins:
  - jekyll-feed
```
El tema por defecto es `minima`, pero yo uso `slate`.

## Layouts
Se pueden sacar layouts (diseños de página) de los repositorios de [minima](https://github.com/jekyll/minima/tree/master/_layouts), [slate](https://github.com/pages-themes/slate/tree/master/_layouts), etc.

Yo me basé en los de minima, que es para blogs (hay otros temas como slate que son para páginas).

Hay que crear una carpeta `_layouts` dentro de docs. Estos layouts van a ser referenciados desde las páginas y posts usando el Front Matter (un pedazo de código YAML que sirve de metadatos) y están escritos en un lenguaje de plantillas que se apoya sobre HTML.

Un conjunto de layouts ejemplo podría ser el siguiente:

### `default.html`
Una página por defecto. Podría tener este contenido:

```
<!DOCTYPE html>
<html lang="{{ site.lang | default: "es-AR" }}">

  <!-- CABECERA HTML -->
  <head>
    <meta charset='utf-8'>
    <meta name="viewport" content="width=device-width,maximum-scale=2">
    <link rel="stylesheet" type="text/css" media="screen" href="{{ '/assets/css/style.css?v=' | append: site.github.build_revision | relative_url }}">
    <link rel="shortcut icon" type="image/png" href="favicon.png">
  </head>

  <body>

    <!-- CABECERA PÁGINA-->
    <div id="header_wrap" class="outer">
        <header class="inner">
          <a href="{{site.baseurl}}">
            <h1 id="project_title">{{ site.title }}</h1>
          </a>
          <h2 id="project_tagline">{{ site.description }}</h2>
          <a href="/about">Acerca de</a>
        </header>
    </div>

    <!-- CONTENIDO -->
    <div id="main_content_wrap" class="outer">
      <section id="main_content" class="inner">
        {{ content }}
      </section>
    </div>

    <!-- PIE DE PÁGINA  -->
    <div id="footer_wrap" class="outer">
      <footer class="inner">
        <p>mimail[at]dominio.dom</p>
        <p>Published with <a href="https://pages.github.com">GitHub Pages</a></p>
      </footer>
    </div>
  </body>
</html>
```

- `CABECERA HTML`: incluye los estilos CSS (que se van a generar a partir de archivos `.sass`) y el favicon (el ícono de la página en las pestañas, historial y marcadores).

- `CABECERA_PÁGINA`: incluye el título del proyecto en un `<h1>` (título principal) y su descripción en un `<h2>` (título secundario).

- `CONTENIDO`: simplemente muestra el contenido del post o la página

- `PIE DE PÁGINA`: se pueden agregar datos de contacto, de copyright, etc.

Van a ver que hay código entre `{{llaves}}`, es código dinámico que lee propiedades seteadas en la configuración o en el Front Matter del post o página que se está creando.

### `home.html`
La página principal:
