---
layout: post
title: "Teclado japonés en Debian"
date: 2023-04-23 19:00:00 -0300
categories: jp, debian, idiomas
author: Jose Arcidiacono (@mokocchi)
permalink: 2023-04-23-a-Teclado-jp-en-debian
---

Hace rato que no escribo en japonés en la compu porque no tenía instalado el soporte para escritura en japonés.

Esto es porque agregué el teclado en la configuración y seguía escribiendo letras romanas, no se podía convertir a kanji.

Esta vez me puse las pilas y pude configurar `mozc` para que me permita la conversión.

## Índice

- [TL;DR](#tldr)
- [Métodos de entrada](#métodos-de-entrada)
- [Mi lío con el japonés](#mi-lío-con-el-japonés)
- [El caso de los emojis](#el-caso-de-los-emojis)
- [IBus](#ibus)
- [Conversión de Romaji a Kanji](#conversión-de-romaji-a-kanji)
- [Conclusión](#conclusión)

## TL;DR

Usar `ibus-setup` en la consola y elegir Japonés (Mozc).

## Métodos de entrada

En los idiomas que tienen una tecla para cada letra como el español o en inglés, por lo general nos basta con conectar el teclado o usar el que trae nuestra notebook. El orden en el que están las letras y los símbolos en el teclado se denomina *distribución del teclado*. Hay veces que compramos una computadora con Windows en español de España, y por lo tanto espera que tengamos ese teclado (la arroba en el 2). O tenemos un sistema operativo en español de Latinoamérica (la arroba en la Q) y le queremos conectar un teclado en distribución española.

Para estos casos hay que cambiar la manera que el sistema interpreta esas teclas, es decir, el *método de entrada*. Si además estamos escribiendo en un idioma en el que los caracteres no se pueden representar con una sola tecla, lo que necesitamos es un *editor de método de entrada* (IME).

## Mi lío con el japonés

En el caso del japonés, yo empecé usando Microsoft IME para Windows XP. Había que bajarse el pack de "Idiomas de Asia Oriental" que pesaba 300mb (un montón para 2008). Lo tenía subido a Dropbox y en un CD por si tenía que instalarlo en otra computadora. Actualmente ya viene instalado en la mayoría de las versiones de Windows.

También recuerdo que era fácil de configurar en Ubuntu, pero cuando me pasé a Debian no lo pude hacer andar más (de esto hace ya como 10 años 😰). Así que si quería escribir en japonés, Windows.

## El caso de los emojis

Windows 10 traía como chiche nuevo que apretando `Ctrl` + `.` se te desplegaba un selector de emojis. Esto es algo que ya hacía Microsoft IME como parte de la conversión de palabras (pero había que saberse el nombre del emoji en japonés).
En Debian no tenía esta herramienta, pero la mayoría de los clientes de chat traen algún tipo de conversión propia con `:nombre-emoji:` y no me importaba. Pero empecé a escribir documentación, diapositivas, y otro tipo de formatos en los que tenía que traerme los emojis de afuera.

Como solución intermedia me instalé [esta extensión](https://extensions.gnome.org/extension/1162/emoji-selector/) 🔗 que me agrega un widget en la barra de estado de gnome donde puedo elegir los emojis. La verdad, la usabilidad malisima, había que elegir el emoji, hacerle click, después `Ctrl`+ `c` y después pegarlo en el texto. Además, se iba sobrecargando tanto visualmente como en procesamiento mi Gnome.

Cuando me fui a quejar con un amigo me dijo "Y por qué no usás un teclado de emojis?". Existían.

## IBus

Uno de los frameworks de métodos de entrada que más se usan en Linux es [IBus](https://github.com/ibus/ibus) 🔗. Me jugué y tiré `sudo apt search ibus emoji`. Salió `ibus-table-emoji`. Me lo instalé. Y no sabía como usarlo.

Entonces busqué cómo se configuraba IBus, y encontré otro comando: `ibus-setup`. Ahí encontré la pestaña de Emoji, se activa con `Super` + `.`. Y al lado estaba la de Métodos de entrada. Agregué Mozc desde ahí y santo remedio.

¿Por qué no funciona desde la configuración del sistema de Gnome? No tengo idea. Pero ahora puedo escribir en japonés de nuevo.

## Conversión de Romaji a Kanji

El punto de todo esto es, recordemos, tipear letras de nuestro teclado y que salgan ideogramas japoneses.

¿Cómo se usa?

La idea es la siguiente, queremos llegar la palabra para "lluvia" (雨).

Para esto tenemos que saber como suena (あめ) y pasarlo a escritura estándar (ame).

Después apretamos `Espacio` y nos sale una lista de palabras posibles con esos sonidos. La primera es, efecto, 雨. Pero también podríamos querer escribir caramelo, que suena igual (飴). Para eso, apretamos `Espacio` otra vez y nos selecciona la segunda opción. Y así vamos armando la frase que queramos.

## Conclusión

Al final era tan sencillo como usar `ibus-setup` y agregar Japonés (Mozc) desde ahí. Pero en el camino conseguí un teclado de emojis! 🦕
