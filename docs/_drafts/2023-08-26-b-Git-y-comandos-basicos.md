---
layout: post
title: Git y comandos básicos
date: 2023-08-26 19:00:00 -0300
categories: git, desarrollo, herramientas
author: Jose Arcidiacono (@mokocchi)
permalink: 2023-08-26-b-Git-y-comandos-basicos
---

Como les decía en el [post anterior](2023-08-26-a-Versionado), administrar
 versiones de nuestro código es un asunto complicado.

Recordemos los desafíos que les planteaba un poco en el post anterior:

- ¿Cómo colaboro con otros sin "pisarnos" (sobreescribir)?

- ¿Cómo hago que a los demás les lleguen mis cambios? ¿En qué formato lo envío?

- ¿Qué pasa si quiero probar una feature y después no me gusta? ¿Y si tengo que tener dos versiones, una en producción y otra con lo que ya desarrollé y todavía no quiero desplegar?

Hoy voy a hablar sobre Git y cómo resuelve estos problemas.

## Git

Git es una herramienta de versionado de código fuente.

Cada vez que decidimos registrar nuestros cambios, git toma nota de qué archivos cambiaron y cómo.
Guarda esta información en su índice, donde mantiene el estado de todos los momentos que decidimos
guardar. Nosotros podemos navegar a cualquier momento de nuestro código que nosotros u otra persona
colaborando con nosotros haya registrado. Incluso se pueden hacer operaciones como tomar las diferencias
entre varios registros a la vez o llevar cambios de una "línea de tiempo a otra" (ramas).

Podés comprobar si ya tenés git abriendo una terminal, con el siguiente comando:

```terminal
git --version
```

Si no lo tenés y estás en Windows, podés descargarlo desde la [página oficial](https://git-scm.com/).

Si estás en una distribución de Linux lo más probable es que lo tengas en el repositorio oficial de paquetes.

Para instalarlo por ejemplo en Debian:

```terminal
sudo apt install git
```

Voy a mostrar los comandos básicos con ejemplos que podés seguir usando tu editor de código (como VSCode) y la consola.

## git init

Partamos de un directorio que conocemos y creemos un directorio nuevo con la consola:

```terminal
mkdir git-demo
```

Entramos al directorio que acabamos de crear

```terminal
cd git-demo
```

Este directorio va a contener nuestro repositorio git.

En este punto podemos abrir el directorio con VSCode.

Inicializamos nuestro repositorio con

```terminal
git init
```

Esto crea un directorio `.git` donde se guardan los archivos del índice.

## git commit

Un commit un registro de los cambios que queremos guardar en el índice. Los cambios son líneas agregadas, quitadas o modificadas, o creaciones y borrados de archivos completos. Cada commit se identifica su SHA-1*.

_*Es un string de 40 dígitos hexadecimales, un hash de resumen, que hasta hace un tiempo era teóricamente único. Recientemente se descubrieron colisiones en SHA-1 y se está proyectando pasar a usar SHA-256._
