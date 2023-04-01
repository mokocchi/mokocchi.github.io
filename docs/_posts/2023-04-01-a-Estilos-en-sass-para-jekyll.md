---
layout: post
title: "Estilos en Sass para Jekyll"
date: 2023-04-01 19:00:00 -0300
categories: blog
author: Jose Arcidiacono (@mokocchi)
permalink: 2023-04-01a-estilos-en-sass-para-jekyll
---

Continuando con el [post](2022-04-08b-Blog-Jekyll) que hice el año pasado sobre Jekyll para GitHub Pages, esta vez voy a hablar de la configuración de los estilos en Sass, que es algo que me quedó pendiente.

## Ubicación de los archivos

En general tenemos un archivo `styles.scss` que va a ser el punto de entrada a nuestros estilos. Este archivo debe estar en el directorio `/css` en la raíz del proyecto (no dentro de `/assets`, por ejemplo, sino al mismo nivel).

Todos los archivos que importemos deben estar en el directorio configurado para Sass. El default es `/_sass` (`"_sass"`) pero puede ser modificado con la siguiente configuración en `/config.yml`:

```yml
sass:
  sass_dir: _sass
```

Quedaría algo así:

```
...
├── _sass
|   ├── categoryA
|   |   ├── _part2.scss
|   |   ├── _part1.scss
|   |   └── index.scss
|   ├── _file1.scss
|   ├── _file2.scss
...
|   └─ _fileN.scss
...
├──css
│   └── styles.scss
...
```

Notar el guion bajo al principio de los nombres de los archivos.

## Configuración mínima

Como Sass es un superset de Css, lo más sencillo que podemos hacer es crear el mismo archivo que crearíamos en CSS.
Por ejemplo, poniendo la siguiente declaración...:

```css
span {
  color: red;
}
```

...en un archivo `styles.scss` en el directorio `/css`.

Esto hace que Jekyll convierta `styles.scss` (el nombre de archivo es indistinto) en `styles.css` y lo deje público en el directorio `/css`del sitio. Esto quiere decir que podemos importarlo en nuestro `layout.html` de la siguiente forma:

```html
<link
  rel="stylesheet"
  type="text/css"
  href="{{ '/css/style.css?v=' | append: site.github.build_revision | relative_url }}"
/>
```

Analicemos un poco lo que mandamos al atributo `href`:

```liquid
{% raw %}
{{'/css/style.css?v=' | append: site.github.build_revision | relative_url }}
{% endraw %}
```

En primer lugar (antes del pipe - `|`), tenemos la ubicación del archivo generado en nuestro proyecto (`/css/style.css`)), más el string `?v=`. Vamos a ver qué hace cuando esté completo.

En segundo lugar tenemos la función `append` que recibe el dato `site.github.build_revision`. Esto es, el hash del commit que está desplegado en github pages. Nos queda, entonces, algo similar a `/css/style.css?v=684e33f330501510fd447f9cf84ea37545f3d518`.

Por último, `relative_url`genera adecuadamente una url para nuestro enlace, considerando que puede no estar en la raíz del dominio.

`?` indica que los parámetros que siguen serán eviados mediante un query string, es decir, en la misma url.
¿Qué hace el parámetro `v`?

**Nada.**

Como estamos hablando de un archivo CSS, no hay nada que pueda hacer con un parámetro. Pero.

Es una forma de hacerle saber al navegador que versión del sitio estamos viendo. ¿Para qué sirve esto? Para que el navegador no pueda cachear el archivo entre versiones, y veamos los cambios al hacer un deploy.

## Resultados

En este momento, si agregamos la línea mencionada a nuestro `default.html` (preferentemente después del css default) ya podemos ver nuestros cambios al hacer

```terminal
$ bundle exec jekyll s
```

y veremos el siguiente output:

```
       Regenerating: 1 file(s) changed at 2023-04-01 20:52:00
                    css/style.scss
       Jekyll Feed: Generating feed for posts
                    ...done in 0.524818887 seconds.
```

## Un poco de Sass

[Sass](https://sass-lang.com/) es un preprocesador de CSS. Extiende el lenguaje con features como anidamiento, funciones, partials, mixins, entre otros.
Sass viene en dos tipos de sintaxis definidas por la extensión del archivo: `,scss`, que es compatible con Css, y `.sass`, que es similar pero usa indentación en lugar de llaves (al estilo de Python). Nosotros vamos a trabajar con el primero porque es más fácil de aprender si ya sabemos algo de css.

En este post vamos a ver una de ellas.

### Partials

Se pueden crear archivos parciales con un guion bajo al principio del nombre de archivo (`\_archivo.scss`). Estos archivos no se precompilan a menos que sean llamados desde un archivo de scss completo.
Podemos crear partials en el directorio (`/_sass`) y es a ese lugar a donde apunta la resolución de imports.
Podemos, por ejemplo, crear un partial que defina los estilos del layout, y llamarlo `_layout.scss` con el siguiente contenido:

```scss
body {
  width: 400px;
}
```

Dentro de nuestro archivo principal (`styles.scss`) llamamos a otro archivo con el siguiente código:

```scss
@import "layout";
```

Notar que no tiene ni extensión ni el guion bajo del principio. De esta forma podemos organizar mejor nuestros estilos.

Es común crear un archivo de variables `_variables.scss` con algunos colores o tamaños predefinidos. Por ejemplo:

```scss
$light-gray: #888888;
$primary: #82aaff;
```

También podemos crear un directiorio para guardar partials relacionados y facilitar su importación. Por ejemplo podemos crear un directorio `components` dentro de `/_scss`. Dentro creamos dos archivos: `_menu.scss` e `index.scss` (sin guion bajo)
El contenido de `_menu.scss` podría ser

```scss
.menu {
  background-color: $light-gray;
}
```

Notar que usa la variable que definimos antes sin necesidad de importarla (porque al final es todo el mismo archivo),
El contenido de `index.scss` irá creciendo con cada archivo que agreguemos, pero inicialmente sería el siguiente:

```scss
@import "menu";
```

Ahora podemos agregar el directorio completo a nuestro `styles.scss` de la siguiente forma:

```scss
@import "components";
```

Automáticamente importa el archivo `index.scss`! Y este importa cada componente del directorio. De esta forma podemos crear muchos archivos e importarlos de forma estructurada.

La principal feature de CSS es que combina los estilos de acuerdo a las declaraciones que se refieren al mismo elemento, por orden de llegada si colisionan. Por ejemplo...

```css
.nav {
  color: red;
}

.nav {
  font-size: 20px;
  color: blue;
}
```

va a resultar en un elemento con letras de color azul (colisión) y fuente de tamaño 20px (combinación).

¿Como hacemos para llevar la cuenta de todos los elementos si los tenemos en archivos distintos?

Podemos usar Notación BEM.

## Un poco de notación BEM

La notación BEM es lo que nos salva del caos de colisiones de nombres, solo siguiendo convenciones de nombres de clase.

En una estructura pensada como componentes, cada componte tiene su propio archivo de estilos. Por ejemplo:

```html
<div class="componente-raiz">
  <div>
    <div class="sub-componente-1"></div>
  </div>
  <div>
    <div class="sub-componente-2"></div>
  </div>
</div>
```
tendría los archivos `_componente-raiz.scss`, `_sub-componente-1.scss` y `_sub-componente-2.scss`.

Supongamos que queremos pintar el componente 1 de rojo, el 2 de azul y el raíz de amarillo. Y además que el componente 1 tenga 10px de margen y el componente 2 tenga 20 px de margen.

La notación BEM pide que llamemos al componente con guiones como separadores de palabras (`componente-raiz`) y las partes con el nombre del componente, dos guiones bajos `__` y el nombre de la parte (hasta 1 nivel preferentemente). Por ejempo, podemos considerar que el componetente raíz tiene dos partes (los divs sin clase) que nos van a ayudar con los márgenes. En ese caso vamos a ponerles como clase `componente-raiz__parte-1` y `componente-raiz__parte-2`.

```html
<div class="componente-raiz">
  <div class="componente_raiz__parte-1">
    <div class="sub-componente-1"></div>
  </div>
  <div class="componente_raiz__parte-2">
    <div class="sub-componente-2"></div>
  </div>
</div>
```
En el archivo `_componente-raiz.scss` creamos las siguentes clases:

```scss
.componente-raiz {
  background-color: yellow;
}

.componente_raiz__parte-1 {
  margin: 10px;
}

.componente_raiz__parte-2 {
  margin: 20px;
}
```

De esta forma sabemos cómo se llama el archivo que estamos buscando con solo mirar el nombre de la clase.

Entonces podemos tener una estructura más o menos así:

```
...
├── _sass
|   ├── components
|   |   ├── _componente-raiz.scss
|   |   ├── _sub-componente-1.scss
|   |   ├── _sub-componente-2.scss
|   |   └── index.scss
|   ├── _defaults.scss
|   └── _variables.scss
...
├──css
│   └── styles.scss
...
```

## Conclusiones

En el post de hoy era vimos dónde agregar los archivos Sass en nuestro proyecto Jekyll, un poco de Sass como para aprovechar esta funcionalidad y por último cómo alinear los archivos y las clases para mantener el proyecto ordenado.