---
layout: post
title: "SHENZHEN I/O"
date: 2022-04-03 11:00:00 -0300
categories: juegos, assembly
author: Jose Arcidiacono (@mokocchi)
permalink: 2022-04-03-SHENZHEN-IO
---

![SHENZHEN I/O logo](https://mokocchi.github.io/assets/images/2022-04-03-SHENZHEN-IO-logo.png)

Me compré el juego [SHENZHEN I/O](https://store.steampowered.com/app/504210/SHENZHEN_IO/) en Steam hace un tiempo. Le puse como 15 horas de juego y avancé bastante, pero lo abrí después de un par de meses y ya no me acuerdo casi nada.

Así que pensé que empezarlo de nuevo era una buena ocasión para hacer una mini guía (sobre todo para no olvidarme otra vez). También sirve como una guía básica de lenguaje ensamblador.

## SHENZHEN I/O
En este juego sos un ingeniero que se muda a la ciudad de Shenzhen en China (yo lo pronuncio como _shenjen_ pero si alguien sabe chino me va a matar) para trabajar en una empresa de microcontroladores.

Te tiran por la cabeza un manual en PDF de 47 páginas donde te explican las partes que tenés que usar y cómo funciona el lenguaje.

En este punto aviso que no es muy apto para todo público el juego, básicamente hay que escribir programas concurrentes en un lenguaje ensamblador (instrucciones para el procesador), usando la menor cantidad posible de instrucciones y sin irse de presupuesto con las partes (esta última parte es un desafío pero es opcional).

No voy a traducir las 47 páginas pero sí lo mínimo para empezar a jugar.

## Introducción al lenguaje MCxxxx
Todas las partes de la familia MCxxxx que están disponibles entienden este lenguaje de programación.

Las partes necesitan dormir entre ciclos de operación para ahorrar energía, porque están diseñadas para operar en ambientes con recursos restringidos.

## Saltos y condicionales
En este lenguaje ensamblador no hay `if`, `for`, ni `while`. Lo unico que hay son los saltos y los condicionales. En los primeros lenguajes de programación de "alto nivel" esto se hacía con las infames instrucciones `goto` que saltaban a cualquier línea y terminaban haciendo el código ilegible (código spaghetti). Cabe aclarar que en el MSX88 que que usé en la facultad y está basado en el Intel 8088 sí tiene saltos condicionales. Bueno, acá no hay.

Volviendo a los saltos, la idea es ir dejando etiquetas en el código con `etiqueta:` que son "puntos de retorno" a los que se puede llegar con un salto.

Por ejemplo, para hacer un `while (true)` basta con saltar siempre a la misma etiqueta:
```
etiqueta: jmp etiqueta
```
Esto no sirve de mucho, ¿no?

Los condicionales permiten "prender" o "apagar" líneas según si se cumple la condición o no. El formato es `txx operando1 operando2` donde xx es la condición (ver Instrucciones condicionales más abajo).
```
  txx operando1 operando2
  instruccion1
+ instruccion2
- instruccion3
```
En el caso que se muestra arriba, `instruccion1` siempre se ejecuta. `instruccion2` solo se ejecuta si se cumple la condición, y `instruccion3` solo en caso contrario.

Una forma de hacer un `for (int i = 0; i<7; i++)` es la siguiente:
```
          guardar_0_en_el_acumulador
etiqueta: instruccion1
          instruccion2
          ...
          test_si_acumulador_es_7
        + jmp afuera
        - incrementar_acumulador
        - jmp etiqueta
  afuera: otras_instrucciones
```
Si se quiere hacer `while (i<7) { ... i++}` el código es exactamente igual.
## Registros
Existen cuatro tipos de registros:
- `acc`: registro acumulador. El lenguaje funciona con una sola dirección (la mayoría de las veces), así que cuando no se especifica un registro, el otro operando es el acumulador
- `dat`: es un registro extra para guardar datos en algunas partes avanzadas.
- registros de pines: contienen el dato que se recibe o se envía de un pin. Notar que algunos pines bloquean la lectura cuando no hay un dato (son concurrentes).
## Conjunto de instrucciones
Este es el set de instrucciones. Los comentarios empiezan con `#` y no se ejecutan, como en Pyhton. Los enteros van de `-999` a `999`.
### Instrucciones básicas

```
# no hacer nada (para rellenar loops o condiciones)
nop

# cargar un dato leido de un registro R o un entero I al registro R
mov R/I R

# saltar a la etiqueta L (para formar loops o esquivar instrucciones)
jmp L

# esperar la cantidad de tiempo indicada en el registro R o en el entero I
slp R/I

# esperar a que haya una entrada en el pin P
slx P

``` 

### Instrucciones aritméticas

```
# sumar el contenido del registro R o el entero I al acumulador y guardarlo en el acumulador
add R/I

# restar el contenido del registro R o el entero I al acumulador y guardarlo en el acumulador
sub R/I

# multiplicar  el contenido del registro R o el entero I con el acumulador y guardarlo en el acumulador
mul R/I

# guarda 100 en el acumulador si el acumulador está en 0. En caso contrario, guarda 0.
not

# deja en el acumulador solo el dígito indicado por el registro R o el entero I. (por ejemplo , si el acumulador tiene 32, dgt 0 hace que en el acumulador quede un 2).
dgt R/I

# guarda el valor del registro R2 o entero I2 en el dígito del acumulador indicado por el registro R1 o el entero R2.
dst R1/I1 R2/I2

```

### Instrucciones condicionales

```
# chequear si los dos valores son iguales
teq R/I R/I
# ejemplo
teq 4 3
+ no_se_ejecuta
- se_ejecuta

# chequear si el primer operando es mayor que el segundo
tgt R/I R/I
# ejemplo
tgt 4 3
+ se_ejecuta
- no_se_ejecuta

# chequear si el primer operando es menor que el segundo
tlt 4 3
+ no_se_ejecuta
- se_ejecuta

# comparar los dos operandos
tcp R/I R/I
# ejemplo 1
tcp 4 3
+ se_ejecuta
- no_se_ejecuta
# ejemplo 2
tcp 3 4
+ no_se_ejecuta
- se_ejecuta
# ejemplo 3
tcp 3 3
+ no_se_ejecuta
- no_se_ejecuta

``` 

## Ahora sí el juego
Bueno después de todo eso se puede empezar con el primer desafío del juego: FAKE SURVEILLANCE CAMERA (cámara de vigilancia falsa).

![Screenshot 1](https://mokocchi.github.io/assets/images/2022-04-03-SHENZHEN-IO-Screenshot-1.png)

Esta es la pantalla de diseño. Te dan las carcasas donde tienen que entrar los chips, y hay que hacer entrar tanto los chips como las instrucciones en los chips.

### `active`, la primera salida

El desarrollador anterior claramente se rindió después de hacer el primer chip, así que tenemos que terminar el trabajo nosotros.

En la pestaña `INFORMATION`, abajo, dice que hay que hacer que `active` y `network` (los dos círculos) reciban las señales que indica la pestaña `VERIFICATION`:

![Screenshot 2](https://mokocchi.github.io/assets/images/2022-04-03-SHENZHEN-IO-Screenshot-2.png)

En el gráfico, la posición baja es un 0 y la posición alta un 100.

Analicemos primero `active` (el primer carril):

- está 6 tiempos abajo
- después está 6 tiempos arriba

Si volvemos al código que escribió el pobre desarrollador anterior tenemos lo siguiente:
```
  mov 0 p0
  slp 6
  mov 100 p0
  slp 6
```
En castellano:
```
  mov 0 p0 # sacar 0 por el pin p0
  slp 6 # esperar 6 tiempos
  mov 100 p0 # sacar 100 por el pin p0
  slp 6 # esperar 6 tiempos
```
Cuanto termina la ejecución vuelve a empezar, así que no hace falta un `jmp` (se parece a la función `loop()` de Arduino).

Podemos ver si le damos al botón `SIMULATE` que cumple con lo pedido, pero en network (el segundo carril) hay una línea roja porque siempre recibe 0:

![Screenshot 3](https://mokocchi.github.io/assets/images/2022-04-03-SHENZHEN-IO-Screenshot-3.png)

### `network`, la segunda salida
Ahora nos toca a nosotros. 
Analicemos la salida esperada de `network` (segundo carril):

![Screenshot 2](https://mokocchi.github.io/assets/images/2022-04-03-SHENZHEN-IO-Screenshot-2.png)

- está 4 tiempos abajo
- está 2 tiempos arriba
- está 1 tiempo abajo
- está 1 tiempo arriba

¿Nos entrará todo eso en el chip?

Podemos usar el mismo tipo de chip, un MC4000 que sale 3 yuanes.

Si copiamos la misma idea del código anterior quedaría así:
```
  mov 0 p0
  slp 4
  mov 100 p0
  slp 2
  mov 0 p0
  slp 1
  mov 100 p0
  slp 1
```

No hay que olvidarse de conectar `p0` con `network`:

![Screenshot 4](https://mokocchi.github.io/assets/images/2022-04-03-SHENZHEN-IO-Screenshot-4.png)

Finalmente le damos a `SIMULATE` y después de comprobar varias veces que funciona, nos da por bueno el prototipo:

![Screenshot 5](https://mokocchi.github.io/assets/images/2022-04-03-SHENZHEN-IO-Screenshot-5.png)

Si se fijan hay una forma de hacerlo con con solo 3 líneas de código... pero para eso hacen falta instrucciones que todavía no tenemos.

## Cerrando
Este fue el primer prototipo del juego SHENZHEN I/O. Ayuda a entender cómo funciona el lenguaje ensamblador y es un buen ejercicio de lógica.

Si alguien sobrevivió a este post, ¡felicitaciones! hasta aquí llega :)