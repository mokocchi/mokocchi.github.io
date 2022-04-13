---
layout: post
title: "Markdown"
date: 2022-04-13 9:00:00 -0300
categories: blog
author: Jose Arcidiacono (@mokocchi)
permalink: 2022-04-13-Markdown
---

El otro día hice un [post](2022-04-08-Blog-Jekyll) sobre cómo hacer un blog con Jekyll y GitHub pages.

Uno de los temas que quedaron pendientes fue Markdown.

## Lenguajes de marcado

Hay una discusión acerca de si HTML es un lenguaje de programación o no. Según me contaron técnicamente permite enviar datos al servidor (con las etiquetas `<form>`).

Más allá de si lo es o no, de lo que estamos totalmente seguros es de que es un lenguaje, pero un *lenguaje de marcado* (HTML es HyperText Markup Language, lenguaje de marcardo para hipertexto).

Un lenguaje de marcado básicamente es texto con etiquetas o códigos que hablan sobre ese texto y lo enriquecen. Suelen ser interpretables por máquinas y hay un montón de librerías que pueden interpretarlos.

Algunos ejemplos:
- `HTML`: HyperText Markup Language (lenguaje de marcardo para hipertexto). Se usa para etiquetar texto para convertirlo en una página web. Algunos ejemplos de etiquetas son `<h1>` y `<input>`. Pueden tener atributos: 

  ```
  <a href='https://google.com>Google</a>
  ```

. El texto `Google` está marcado con la etiqueta `a` (anchor, una referencia) y tiene el atributo `href` (HyperRefrence) que es un link a la página de Google.
- `XML`: eXtensible Markup Language (lenguaje de marcado extensible). Se usa para estructurar texto en jerarquías para representar entidades con atributos. Pueden tener cualquier etiqueta. Por ejemplo:
  ``` 
  <xml>
    <estudiante>
      <nombre>Juan</nombre>
      <apellido>Rodríguez</apellido>
    </estudiante>
  </xml>
  ```
  Además pueden tener un `schema` que sirve al programa que lo lee para validar que el contenido tenga la estructura correcta. Los servicios SOAP suelen usar XML para comunicarse entre ellos. `XHTML`, es una versión más estricta de HTML que también es XML (requiere, por ejemplo, que todas las etiquetas tengan etiqueta de cierre)

## Markdown

Markdown es un lenguaje de marcado ligero, es decir, que es fácil de editar y ocupa poco espacio. Está pensado para darle formato al texto, de forma tal que sea legible aún cuando en su versión fuente.

Uno de los lugares donde está totalmente integrado es GitHub, donde todos los archivos Markdown (`.md`) tienen la opción de previsualizarse. Incluso los archivos `README.md` y `LICENSE.md` que se crean automáticamente en los repositorios se escriben en Markdown. 

Como decía en el post que menciono al principio, en este blog escribo los artículos en HTML y el motor se encarga de convertirlo a HTML.

Si saben escribir HTML y quieren ver cómo quedaría en Markdown, pueden usar la herramienta `html2text`. Se la instala con `pip` (Python)

```
pip3 install html2text

html2text documento.html > destino.md
```

