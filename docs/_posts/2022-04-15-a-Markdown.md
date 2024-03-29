---
layout: post
title: "Markdown"
date: 2022-04-15 19:00:00 -0300
categories: blog
author: Jose Arcidiacono (@mokocchi)
permalink: 2022-04-15a-Markdown
---

El otro día hice un [post](2022-04-08-Blog-Jekyll) sobre cómo hacer un blog con Jekyll y GitHub pages. Uno de los temas que quedaron pendientes fue Markdown.

## Lenguajes de marcado

Hay una discusión acerca de si HTML es un lenguaje de programación o no. Según me contaron técnicamente permite enviar datos al servidor (con las etiquetas `<form>`).

Más allá de si lo es o no, de lo que estamos totalmente seguros es de que es un lenguaje, pero un *lenguaje de marcado* (HTML es HyperText Markup Language, lenguaje de marcardo para hipertexto).

Un lenguaje de marcado básicamente es texto con etiquetas o códigos que hablan sobre ese texto y lo enriquecen. Suelen ser interpretables por máquinas y hay un montón de librerías para trabajar con ellos.

Algunos ejemplos:
- `HTML`: HyperText Markup Language (lenguaje de marcardo para hipertexto). Se usa para etiquetar texto para convertirlo en una página web. Algunos ejemplos de etiquetas son `<h1>` y `<input>`. Pueden tener atributos: 

  ```html
  <a href='https://google.com>Google</a>
  ```

  El texto `Google` está marcado con la etiqueta `a` (anchor, una referencia) y tiene el atributo `href` (HyperRefrence) que es un link a la página de Google.

- `XML`: eXtensible Markup Language (lenguaje de marcado extensible). Se usa para estructurar texto en jerarquías para representar entidades con atributos. Pueden tener cualquier etiqueta. Por ejemplo:
  ```xml
  <xml>
    <estudiante>
      <nombre>Juan</nombre>
      <apellido>Rodríguez</apellido>
    </estudiante>
  </xml>
  ```
  Además pueden tener un `schema` que sirve al programa que lo lee para validar que el contenido tenga la estructura correcta. Los servicios SOAP suelen usar XML para comunicarse entre ellos. `XHTML` es una versión más estricta de HTML que también es XML (requiere, por ejemplo, que todas las etiquetas tengan etiqueta de cierre).

## Sobre Markdown

Markdown es un lenguaje de marcado ligero, es decir, que es fácil de editar y ocupa poco espacio. Está pensado para darle formato al texto, de forma tal que sea legible aún cuando en su versión fuente.

Uno de los lugares donde está totalmente integrado es GitHub, donde todos los archivos Markdown (`.md`) tienen la opción de previsualizarse. Incluso los archivos `README.md` y `LICENSE.md` que se crean automáticamente en los repositorios se escriben en Markdown. 

Como decía en el post que menciono al principio, en este blog escribo los artículos en HTML y el motor se encarga de convertirlo a HTML.

Si saben escribir HTML y quieren ver cómo quedaría en Markdown, pueden usar la herramienta `html2text`. Se la instala con `pip` (Python)

```bash
pip3 install html2text

html2text documento.html > destino.md
```

## Cómo escribir en Markdown

Voy a ir poniendo códigos de ejemplo. Si tienen cuenta en GitHub, pueden entrar a [GitHub Gist](https://gist.github.com) y probar escribir su propio texto. Basta con poner de extensión de archivo `.md`,

Primero que nada, los **títulos**. Para los que conocen HTML, equivalen a `<h1>`, `<h2>`, etc.

```markdown
# Título nivel 1
## Título nivel 2
### Título nivel 3
#### Título nivel 4
##### Título nivel 5
###### Título nivel 6
``` 
***
# Título nivel 1
## Título nivel 2
### Título nivel 3
#### Título nivel 4
##### Título nivel 5
###### Título nivel 6

***

Hay una variante que usa la subrayados con `=` (igual) y `-` (menos / guion). No es muy quisquilloso con la cantidad, basta que sea uno o más:

```markdown
Título nivel 1
===============

Título nivel 1
========
``` 

***

Título nivel 1
===============

Título nivel 1
========
***

Siguiente: **párrafos**. Un párrafo en Markdown es un bloque de texto que termina cuando se deja una línea en blanco. Esto es útil
cuando poner saltos de línea (`enter`) para que sea legible el código
pero no queremos que salgan en el texto resultante. Nota: en general no se pueden crear líneas vacías, salvo que usemos espacios HTML (`&nbsp;`)

```markdown
Este es el primer párrafo. Tiene 
varias líneas porque al final
de cada una hay un enter, pero
el texto resultante queda todo pegado

Cuando dejo una lína en blanco, cambia de párrafo. Y si quiero dejar líneas en blanco:

&nbsp;

&nbsp;

Dos líneas en blanco!
```
***
Este es el primer párrafo. Tiene 
varias líneas porque al final
de cada una hay un enter, pero
el texto resultante queda todo pegado

Cuando dejo una lína en blanco, cambia de párrafo. Y si quiero dejar líneas en blanco:

&nbsp;

&nbsp;

Dos líneas en blanco!

***

Siguen las **listas**.
```
Tres formas de hacer una lísta sin números:

- un ítem
- otro ítem

+ un ítem
+ otro ítem

* un ítem
* otro ítem

Lista ordenada: 
1. primer ítem
2. segundo ítem
```
***
Tres formas de hacer una lísta sin números:

- un ítem
- otro ítem

+ un ítem
+ otro ítem

* un ítem
* otro ítem

Lista ordenada: 
1. primer ítem
2. segundo ítem

***

Ahora, qué pasa si quiero empezar una línea con un año?

>2009. Ese fue el año de la tormenta.

Se puede _escapar_ el punto con una barra invertida (`\`):
```markdown
2009\. Ese fue el año de la tormenta.
```
***
2009\. Ese fue el año de la tormenta.

***

Para decorar el texto se pueden usar cursiva, negrita, o ambas:

```
*cursiva* / _cursiva_

**negrita** / __negrita__

***ambas*** / ___ambas____
```

*cursiva* / _cursiva_

**negrita** / __negrita__

***ambas*** / ___ambas___

Para quienes escriben sobre programción y/o con símbolos, existen los bloques de código y el formato de código. Se usa una fuente tipográfica monospace, es decir que todas las letras tienen en mismo ancho haciendo fácil comparar líneas y contar caracteres.

<pre>
En esta línea hay `formato de código` en el medio.

```
estas tres líneas

están escritas

como si fuera código de programación
```
</pre>

***
En esta línea hay `formato de código` en el medio.

```markdown
estas tres líneas

están escritas

como si fuera código de programación
```
***

Los **links** se escriben de la siguiente forma:
```
[texto](link)
```
***

[texto](link)

***

Las **imágenes** se insertan de forma similar:

```markdown
![texto alternativo](https://mokocchi.github.io/portada.png)
```
***

![texto alternativo](https://mokocchi.github.io/portada.png)

***

Eso es todo por ahora. Espero que les sirva para su blog o para armar la documentación de sus repositorios.

