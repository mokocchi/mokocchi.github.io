---
layout: post
title: "Versionado"
date: 2023-08-26 15:00:00 -0300
categories: git, desarrollo, herramientas
author: Jose Arcidiacono (@mokocchi)
permalink: 2023-08-26-a-Versionado
---

¿Cuántas veces te pasó que tu archivo de un informe se llamaba "Informe final (2).ultimo.revisado.doc"?

El versionado de contenido es una problemática común que es bastante difícil de organizar por nuestra cuenta.

## Números de versión

Cuando se trata de software, en el mundo del software libre principalmente siguen la siguiente nomenclatura para identificar las versiones:

```text
version_mayor.version_menor.bugfix
```

Por ejemplo, el kernel de linux que estoy corriendo está en su versión:

```text
6.1.0
```

- Versión mayor: cambia cuando se hacen cambios grandes que implican pérdida de compatibilidad
- Versión menor: cambia cuando se agrega o actualiza una funcionalidad que no rompe compatibilidad
- Bugfix: cambia cuando se corrige un problema puntualmente sin agregar o quitar funcionalidad

Este tipo de versionado es lineal, es decir que todos los cambios son consecutivos y están en orden.

Ustedes se preguntarán, sí, obviamente, los cambios son consecutivos y lineales porque así funciona el tiempo y los cambios.

Pero no necesariamente es así. Ya veremos que algunos sistemas de versionado nos permiten _¡viajar en el tiempoooo!_ y _¡conocer realidades alternativas!_.

## Desafíos

Aún solucionado nuestro problema de nomenclatura, nos quedan algunos problemas:

- ¿Cómo colaboro con otros sin "pisarnos" (sobreescribir)?

- ¿Cómo hago que a los demás les lleguen mis cambios? ¿En qué formato lo envío?

- ¿Qué pasa si quiero probar una feature y después no me gusta? ¿Y si tengo que tener dos versiones, una en producción y otra con lo que ya desarrollé y todavía no quiero desplegar?

## Sistemas de versionado

Existen varias soluciones para versionado de código fuente (SCM: Source Code Management), entre ellas:

- Subversion o SVN: últimamente estos repositorios están siendo migrados a otras soluciones, como git

- Git: el sistema de versionado que se hizo para administrar el código del kernel de Linux, y se usa en servicios online como GitLab, GitHub y BitBucket

- CVS: provee repositorios en un sistema cliente-servidor. Hace tiempo que no se actualiza.

- Bazaar: mantenido por Canonical, se usa en sitios como Launchpad y Sourceforge, y para proyectos como Ubuntu, Emacs, Inkscape y MySQL.

- Mercurial: sistema de versionado completamente distribuido centrado en el rendimiento. Se usa en proyectos de Mozilla, entre otros.

## Conclusión

Esta fue una pequeña intro a lo que es versionado, con la idea de escribir próximamente una entrada sobre git, y después una sobre GitHub.
