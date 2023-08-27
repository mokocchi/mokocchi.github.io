---
layout: post
title: Intro a la terminal de Linux
date: 2023-08-26 18:00:00 -0300
categories: terminal, herramientas
author: Jose Arcidiacono (@mokocchi)
permalink: 2023-08-27-a-Intro-a-la-terminal-de-Linux
---
Hoy vengo a contar como empezar a moverse en la terminal de Linux. Hay muchos tipos de consolas, pero yo uso con la consola o shell **bash**, que viene
por default en distribuciones de Linux como Debian.

Voy a dejar un par de desafíos para que puedan poner en práctica lo que dice el artículo.

## ¿Empezamos? 

Podemos acceder a la consola de varias maneras. Una de ellas es simplemente ir a las
aplicaciones y buscar la que se llama **Terminal**. Otra opción es abrir la consola
integrada en aplicaciones como **VSCode**.

Una vez en la terminal nos encontramos con el prompt, es decir que el intérprete de comandos espera que escribamos algo.
El prompt default tiene la forma `nombreusuario@nombresistema: directorio`.

**Desafío 1:** ¿Qué dice tu prompt cuando abrís la terminal?

Cómo saber cuál es mi nombre de usuario? Con el comando `whoami` (Who am I?):

En la caja de código de acá abajo, tenemos un signo `$`, eso representa nuestro prompt, y lo que está al lado es lo que tenemos que escribir (el `$` NO se escribe).

La segunda línea es la respuesta de la terminal.

```
$ whoami
mokocchi
```

## Encontrando nuestro camino

Si abrieron la terminal desde la lista de aplicaciones, se inicia desde un directorio llamado `~` (como el moñito de la ñ).

¿Qué directorio es ese? Para averiguarlo vamos a usar nuestro primer comando: `pwd`.

```terminal
$ pwd
/home/mokocchi
```

`pwd` significa Print Working Directory: dónde estoy parado ahora. 

**Desafío 2**: Ejecutá el comando `pwd` en la terminal. Es lo mismo que dice el prompt?

Una vez que sabemos en qué directorio estamos, lo próximo que podemos hacer es listar lo que está en este directorio, con el comando `ls`.

```
$ ls
Documentos  Imágenes  Plantillas  Vídeos
Descargas  Escritorio  Música    Público
```

Aparecen los nombres de todo lo que hay en el directorio, pero solo tenemos los nombres. 

### Usando argumentos

La mayoría de los comandos y aplicaciones de Linux aceptan información necesaria para arrancar (opciones) en forma de **argumentos**. Existen dos tipos de argumentos, **posicionales** y **de palabra clave**.
Los **argumentos posicionales** se identifican por el orden en que aparecen.

Por ejemplo, si queremos saber qué contiene el directorio Descargas, podemos enviarle el nombre del directorio como argumento posicional a `ls` (recordar que Linux es case-sensitive, las mayúsculas importan):

```terminal
$ ls Descargas
com.vscodium.codium.flatpakref
```

A mí me aparece solo un archivo que descargué hace un rato.

**Desafío 3**: Para cada uno de los directorios que aparecieron al hacer `ls`, hacé `ls <directorio>` para ver su contenido.

Los **argumentos de palabra clave** vienen en dos formas: cortos (una letra) y largos (una palabra). Pueden llevar un valor o no.

Para saber qué versión del comando `ls` que acabamos de ver tenemos, podemos mandarle el argumento largo `--version` de la siguiente forma:

```terminal
$ ls --version
ls (GNU coreutils) 9.3
Copyright © 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Escrito por Richard M. Stallman y David MacKenzie.
...
```

**Desafío 4**: Qué versión tenés de ls?

Un argumento corto que entiende `ls` es `-a` (all): 

```terminal
$ ls -a
.              bin        Documentos        .local      .profile  Vídeos
..             .cache     Escritorio        Música      Público   .vscode
.bash_history  .config    .git-config       .pki        .ssh      .vscode-oss
.bashrc        Descargas  Imágenes          Plantillas  .var

```

Aparecen algunos elementos nuevos: todos los que empiezan con `.` (punto). En Linux, los archivos que comienzan con `.` están ocultos. Además, hay dos directorios especiales: `.` (el directorio actual) y `..` (el directorio superior o padre). Vamos a volver a esto cuando tengamos que movernos de directorio.

Otro argumento corto que podemos usar con `ls` es `-l` (long):

```terminal
$ ls -l
total 0
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 14:18 bin
drwxr-xr-x. 1 mokocchi mokocchi 60 ago 27 15:33 Descargas
drwxr-xr-x. 1 mokocchi mokocchi  6 ago 27 15:28 Documentos
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Escritorio
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Imágenes
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Música
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Plantillas
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Público
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Vídeos
```

Nos muestra detalles acerca de los elementos que están en el directorio (aunque no se entiende nada). Lo importnante en este caso es que en todas las filas la primera letra es una `d`, es decir que todos son directorios. El número que aparece después del segundo mokocchi es el peso en bytes del elemento.

Podemos ver el contenido detallado de un directorio más interesante, como `/etc`. En este directorio se guarda la configuración del sistema y las aplicaciones. Primero van el argumentos con palabra clave y el posicional al final:

```terminal
$ ls -l /etc

-rw-r--r--. 1 root root       16 jun  8 03:49 adjtime
drwxr-xr-x. 1 root root       26 jun 25 01:22 alacritty
-rw-r--r--. 1 root root     2579 ago 12 10:24 aliases
drwxr-xr-x. 1 root root        0 feb 14  2023 aliases.d
drwxr-xr-x. 1 root root       12 ago 21 06:59 alsa
drwxr-xr-x. 1 root root        0 ago 12 10:24 alternatives
drwxr-xr-x. 1 root root       50 ago 12 14:48 apparmor.d
drwxr-x---. 1 root root       86 ago 12 13:28 audit
...
```
En este caso tenemos dos archivos (sin la `d`): `adjtime` y `aliases`. Pero ahora se ponen más difíciles de leer los números del tamaño. Podemos pedir la versión "para humanos" con el argumento `-h`. Podemos combinarlo con `-l` dejando un espacio (`-l -h`) o sacando el segundo guion (`-lh`):

```terminal
$ ls -lh /etc

total 500K
-rw-r--r--. 1 root root     16 jun  8 03:49 adjtime
drwxr-xr-x. 1 root root     26 jun 25 01:22 alacritty
-rw-r--r--. 1 root root   2,6K ago 12 10:24 aliases
drwxr-xr-x. 1 root root      0 feb 14  2023 aliases.d
drwxr-xr-x. 1 root root     12 ago 21 06:59 alsa
drwxr-xr-x. 1 root root      0 ago 12 10:24 alternatives
drwxr-xr-x. 1 root root     50 ago 12 14:48 apparmor.d
drwxr-x---. 1 root root     86 ago 12 13:28 audit
```

Ahora podemos ver que aliases pesa 2,6 KB (en lugar de 2579).

`ls` tiene muchos más argumentos y funcionalidad. Podemos verlos todos con el argumento de palabra clave largo `--help`:

```terminal
$ ls --help
Modo de empleo: ls [OPCIÓN]... [FICHERO]...
Muestra información acerca de los FICHEROs (del directorio actual por defecto).
Ordena las entradas alfabéticamente si no se especifica ninguna de las
opciones -cftuvSUX ni --sort.

Los argumentos obligatorios para las opciones largas son también obligatorios
para las opciones cortas.
  -a, --all                  no oculta las entradas que comienzan con .
  -A, --almost-all           no muestra las entradas . y .. implícitas
      --author               con -l, imprime el autor de cada fichero
  -b, --escape               imprime escapes en estilo C para los caracteres no
                             gráficos
      --block-size=SIZE      with -l, scale sizes by SIZE when printing them;
                             e.g., '--block-size=M'; see SIZE format below
...
```

## Cambiando de aire

Ya sabemos cómo saber dónde estamos y qué hay a nuestro alrededor.

Vamos a probar algunos comandos para movernos un poco.

Para empezar vamos a movernos a nuestro home, si no estamos ahí, con el comando `cd` (Change Directory):

```terminal
$ cd
```

Comprobamos donde estamos con `pwd`:

```terminal
$ pwd
/home/mokocchi
```

Efectivamente estamos en nuestro home.

Ahora vamos a ver qué directorios tenemos a mano con `ls -l` (sólo podemos entrar si tienen la `d` al principio):

```terminal
$ ls -l
total 0
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 14:18 bin
drwxr-xr-x. 1 mokocchi mokocchi 60 ago 27 15:33 Descargas
drwxr-xr-x. 1 mokocchi mokocchi  6 ago 27 15:28 Documentos
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Escritorio
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Imágenes
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Música
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Plantillas
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Público
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 15:14 Vídeos
```

Vamos al directorio `Descargas` pasándole a `cd` el nombre del directorio como argumento posicional:

```terminal
$ cd Descargas
```

Comprobamos donde estamos:

```terminal
$ pwd
/home/mokocchi/Descargas
```

Si queremos volver, tenemos que hacer `cd` hacia un directorio especial, `..` (punto punto) que representa el directorio superior o padre del que estamos viendo.

```terminal
$ cd ..
$ pwd
/home/mokocchi
```

Vayamos al directorio `Documentos`:

```
$ cd Documentos
```


### Creando cosas

Para ir cerrando, vamos a ver cómo crear archivos y directorios.

Primero, un directorio: se crean con el comando `mkdir` (Make Directory) y el nombre del directorio a crear. Vamos a hacer un directorio `playground`:

```terminal
$ mkdir playground
```

Comprobamos que es un directorio con `ls -l`:

```terminal
$ ls -l
total 0
drwxr-xr-x. 1 mokocchi mokocchi  0 ago 27 18:39 playground
```
Tiene la `d`. Genial. Nos metemos en el directorio:

```terminal
$ cd playground
```

Ahora, una forma de crear un archivo es el comando `touch`. En realidad es para otra cosa, pero para crear archivos nos sirve igual.

```terminal
$ touch README.md
```

Comprobamos con `ls -l`:

```terminal
$ ls -l
total 0
-rw-r--r--. 1 mokocchi mokocchi  0 ago 27 18:41 README.md
```

## Cerrando

Con esto más o menos podemos empezar a movernos a través del sistema de archivos. Este es un pantallazo de cómo funcionan algunos de los comandos para la consola de Linux. 

---------------------


## Referencia de comandos:

- `whoami`: mostrar nombre del usuario actual
- `pwd`: mostrar directorio actual
- `ls`: listar contenido del directorio actual - argumentos: -l (long), -a (all), --version, &lt;string&gt; (leer de ese directorio)
- `cd`: cambiar de directorio
- `mkdir`: crear directorios
- `touch`: crear archivos