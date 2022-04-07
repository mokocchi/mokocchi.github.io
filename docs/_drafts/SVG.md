---
layout: post
title: "Imágenes vectoriales (SVG)"
date: 2022-04-07 9:00:00 -0300
categories: gráficos
author: Jose Arcidiacono (@mokocchi)
permalink: 2022-04-07-Imagenes-vectoriales-svg
---

Hoy voy a hablar de otro de mis pasatiempos: vectorizar imágenes.

Una imagen vectorial está compuesta por elementos geométricos como puntos, rectángulos, etc. definidos
mediante sus coordenadas. Esto quiere decir que sin importar cuanto zoom hagamos,
la imagen se vuelve a dibujar sin pixelarse.

Por ejemplo, tomemos esta esta imagen:

![Triangulo svg](https://mokocchi.github.io/assets/images/2022-04-07-triangulo.svg)

Si la abrimos en una pestaña nueva y hacemos zoom, no se deforma.

Si la abrimos con un editor de texto (al guardarla como .txt por ejemplo) vemos algo así:

```
<path
  style="fill:#ff00ff;
    stroke:#000000;
    stroke-width:2;
    stroke-linecap:butt;
    stroke-linejoin:miter;
    stroke-miterlimit:4;
    stroke-dasharray:none;
    stroke-opacity:1"
  d="M
    21.036737,99.274854
    74.021322,59.563732
    120.77816,94.293247
    Z"
  id="path10" />
```

Tenemos un `path`, es decir una ruta entre puntos. Lo primero que aparece son los estilos, `style`. Podemos ver las propiedades del trazo (`stroke`) y el relleno (`fill`). Este último es  `#ff00ff` (magenta) (ver _Nota 1: Colores_).

Después de eso viene lo que quería mostrar, en `d=` tenemos tres coordenadas: ¡los puntos del triángulo!

De esta forma el editor genera la imagen. La idea no es escribir las coordenadas a mano, no temáis. Pero creo que es interesante saber cómo funciona.








-------------------------
_Nota 1: Colores_ Como los colores son RGB y van de 0 a FF (255 en hexadecimal), FF en rojo (R, red), 00 en verde (G, green) y FF en azul (B, blue) nos da magenta.