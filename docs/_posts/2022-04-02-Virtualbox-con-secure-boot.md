---
layout: post
title: "Virtualbox con secure boot"
date: 2022-04-02 17:00:00 -0300
categories: virtualizacion
author: Jose Arcidiacono (@mokocchi)
permalink: 2022-04-02-Virtualbox-con-secure-boot
---

Una de mis preocupaciones con el proyecto de Morse con luz ([Parte I](2022-03-28-Morse-I), [Parte II](2022-03-28-Morse-II)) era quemar el flash de mi celular, así que me prestaron uno fuera de uso. Es de esos que se pueden programar en java, pero viene de la época en que los drivers venían en un CD aparte.

Por supuesto no espero que el CD sea compatible con Windows 10 u 11, por la época calculo que un Windows XP le andaría bien. Es por eso que quise instalar Virtualbox en mi notebook, para leer el CD dentro de una máquina virtual.

## Secure Boot
Tengo una notebook del 2021 con Windows 11 en una partición y Linux Debian 11 en la otra.

La compré específicamente porque tenía Windows 11 y no puedo actualizar mi Windows 10 de la otra notebook porque no tiene TPM ni Secure Boot.

Acá viene el chiste: ¿Para qué sirve el Secure Boot (arranque seguro)?

Para evitar que se instale código malicioso en el sistema, con Secure Boot no se puede arrancar con software que no esté firmado. En la partición de arranque hay una lista de claves de confianza, y si algún módulo del sistema operativo está sin firmar o está firmado con una clave que no está en la lista, el sistema no arranca.

Esto es un arreglo de Microsoft con los fabricantes de hardware, que tienen que incluir la funcionalidad en sus computadoras si quieren ser compatibles con Windows. Hasta Windows 10, era opcional.

Hasta hace un par de años, no se podía arrancar Linux si el sistema tenía habilitado Secure Boot. Por eso la comunidad pensaba que era una movida de Microsoft para dejar Linux fuera del mercado.

Actualmente las instalaciones de Linux incluyen las firmas necesarias para arrancar, y el kernel viene firmado.

Así que instalé mi Debian junto con Windows 11 y los dos funcionan con Secure Boot.

## Virtualbox

Resulta que no hay un paquete oficial de Virtualbox para Debian. Aparentemente hay conflictos entre Oracle (los dueños de Virtualbox) y Debian. Debian quiere seguir dando soporte a hardware viejo, con versiones parcheadas del software anterior, y Oracle no quiere hacer los parches porque no está de acuerdo con esta política.

La gente de Debian sigue dando soporte a este tipo de paquetes de forma no oficial mediante repositorios _"Fasttrack"_. El problema es que estos módulos del kernel (drivers de red etc.) no están firmados, porque no son oficiales.

## Cargando mi firma en el arranque
La solución más lógica es simplemente desactivar Secure Boot, total yo sé lo que instalo en mi máquina, ¿no?

Tengo un pequeño problema. Muchas de las funcionalidades de Windows 11 no corren si se desactiva Secure Boot. Eso incluye otro software como algunos juegos con sistemas anti cheats, por ejemplo.

Considerando que me compré la máquina porque no quería quedarme en Windows 10, sería una tontería desactivar el Secure Boot.

La otra alternativa es agregar una firma propia a la lista de firmas válidas, y con ella firmar los módulos de Virtualbox.

En los inicios del Secure Boot, para poner una firma nueva en el arranque había que tomar el control de las firmas, corriendo el riesgo de borrar las de Microsoft. Aparentemente existe la posibilidad de dejar inarrancable (brickeada) la máquina, porque hay firmware de la GPU que está firmado por Microsoft y no corre sin Secure Boot.

Para evitar este problema existe Shim. Es una aplicación EFI que primero intenta cargar el arranque de forma normal, pero si Secure Boot está habilitado chequea una lista propia de claves.

## Tutorial
(sacado de [acá](https://unix.stackexchange.com/questions/560895/sign-kernel-modules))

Para cargar una clave propia o MOK (Machine Owner Key, "Clave del propietario de la máquina") primero hay que crear una:
```
openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -days 36500 -subj "/CN=Mi nombre/" -nodes
```
Algunos comentarios:
- En `"/CN=Mi nombre/"` va el nombre de quien firma (se muestra en la lista de firmas).
- `-nodes` no encripta la clave con una contraseña, así que hay que guardarla en un lugar seguro o no usar esta opción.

Después hay que cargar la clave en shim:
```
mokutil --import MOK.der
```
Pide una contraseña de un solo uso que permite entrar al gestor de claves en el próximo arranque.

Para cargarla acá hay una guía con capturas de pantalla [acá](https://gist.github.com/reillysiemens/ac6bea1e6c7684d62f544bd79b2182a4)

Los pasos a seguir son:
- Elegir Enroll MOK
- Elegir Continue (muestra la clave en la otra opción)
- Elegir Yes (agregar la clave anterior)
- Ingresar la contraseña de un solo uso
- Elegir OK (la máquina se reinicia)

Para comprobar que se cargó se pueden ver los mensajes del kernel: 
```
sudo dmesg | grep '[U]EFI.*cert'
```

En el link anterior hay un script para firmar automáticamente todos los módulos del kernel de Virtualbox, pero yo prefiero hacerlo a mano:
### Firmar los módulos
``` 
cd /usr/src/kernels/$(uname -r)/scripts/

./sign-file sha256 /path/to/MOK.priv /path/to/MOK.der /path/to/modulo.ko
```
donde `/path/to/MOK.priv` es la clave privada generada con su ruta completa (se puede arrastrar el archivo a la consola), ídem para la clave pública en `/path/to/MOK.der`, y `/path/to/modulo`.ko es uno de los mólulos a firmar (vboxdrv.ko, etc).

La ruta de los módulos se puede encontrar usando `modinfo`:
```
$(modinfo -n vboxdrv)
```
### Instalar los módulos
```
sudo modprobe vboxdrv
```

Con eso quedan instalados los módulos y se pueden correr máquinas virtuales en Virtulabox (sin los módulos solo abre).

Acá es donde me gustaría decir que ya instalé Windows XP y tengo los drivers del celular, pero la verdad es que me olvidé y quedará para otra ocasión.