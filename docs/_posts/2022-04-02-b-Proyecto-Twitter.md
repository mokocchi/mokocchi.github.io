---
layout: post
title: "Proyecto Twitter"
date: 2022-04-02 20:00:00 -0300
categories: arduino, iot
author: Jose Arcidiacono (@mokocchi)
permalink: 2022-04-02b-Proyecto-Twitter-I
---

Estoy tratando de meterme en el mundo de Internet de las cosas (IoT) haciendo cosas en Arduino. Saqué de la biblioteca de la facultad un libro que se llama "Designing the Internet of Things", de Adrian McEwen y Hakim Cassimally.

## Los tres elementos de IoT
Según entendí para llamarse Internet de las Cosas, tiene que haber tres elementos:
1. procesamiento
2. Internet
3. una cosa

Hasta ahora las cosas que había hecho tenían procesamiento pero no se conectaban a Internet, y tampoco había una "cosa" que estuvieran "encantando" o "embrujando", como dicen en el libro (hablan de la frase _"Cualquier tecnología suficientemente avanzada es indistinguible de la magia"_).

Así que me puse a pensar un proyecto que sí pudiera llamarse de Internet de las Cosas, aunque sea para divertirme un rato.


### Procesamiento
Para el procesador elegí mi nodeMCU que tiene WiFi. Hay un firmware a medida que tiene muchas librerías, pero preferí usar las librerías de ESP8266 para Arduino.

### Internet
Necesitaba un servicio en la nube que ya existiera porque no quería gastar en hosting. Así que pensé en alguna API pública y gratuita. Me decidí por la [API de Twitter](https://developer.twitter.com/).

### Cosa
Esta es la parte más difícil para mí, porque no estoy siguiendo el orden normal de las cosas. Uno suele tener un problema y busca como resolverlo. Yo acá tengo una solución y estoy buscando un problema. Como uso la API de Twitter, pensé en la gente que sigue a youtuber y twitteros con devoción, y me imaginé que alguien tenía un altar personal en su casa con la foto de su twittero favorito en un portarretrato. Qué tal si el portarretrato... bueno eso ya sería parte del procesamiento, pero la cosa ya la tengo: un portarrettrato.

## Idea
Siguiendo con la línea del portarretrato, ¿qué tal si pudiera leer siempre el ultimo tweet de mi twittero favorito?
1. Necesitaría mostrar los tweets.
2. Tendría que poder elegirse la red WiFi y cargarse la contraseña.
3. Tendría que tener cómo mostrar el estado en el que está y estados de error..

## Pruebas
Fui haciendo una serie de pruebas (que no son ni prototipos) para irme acercando a lo que quería.

### #1: Conectarme al WiFi con la NodeMCU
Lo primero que hice fue conectarme al WiFi con una contraseña escrita en el código. Es bastante sencillo. 
- Primero hay que poner el dispositivo en modo... dispositivo (station):
```cpp
WiFi.mode(WIFI_STA);
```
- Después hay que agregar el punto de acceso con el nombre de la red (SSID) y la contraseña:
```cpp
WiFiMulti.addAP(nombre_SSID, contraSSID);
```
- Por último hay que hacer que intente conectarse hasta que lo logre:
```cpp
WiFiMulti.run()
```
- Como extra, si queremos saber qué IP le asignaron a la placa:
```cpp
WiFi.localIP()
```
Juntando todas las piezas, y con algo de salida por serie para entender que pasa, quedaría así:
```cpp 
#include <ESP8266WiFi.h>
#include <WiFiClient.h>
#include <ESP8266WiFiMulti.h>
#include "config.h"

ESP8266WiFiMulti WiFiMulti;

void setup() {
  Serial.begin(115200);
  Serial.println();
  Serial.print("Configurando access point...");

  WiFi.mode(WIFI_STA);
  WiFiMulti.addAP(nombreSSID, contraSSID);

  while ((WiFiMulti.run() != WL_CONNECTED)) {
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi conectado");
  Serial.println("Dirección IP: ");
  Serial.println(WiFi.localIP());
}

void loop() {

}
```
`nombreSSID` y `contraSSID` no están definidas en este archivo sino en un `.h` que tiene la información de configuración:
```cpp
#define nombreSSID ""

#define contraSSID ""
```
### #2 Conectarme a HTTPS
Lo segundo que hice fue conectarme a una página HTTPS. Me salté HTTP porque hay un ejemplo en la librería y es bien fácil.

Arranca igual que el código anterior, pero crea un cliente HTTPS:
```cpp
std::unique_ptr<BearSSL::WiFiClientSecure>client(new BearSSL::WiFiClientSecure);
```
Después carga la huella digital del certificado de la página a la que me quiero conectar. Idealmente hay que sacarla de la misma página, pero también se hace de esta forma y se lo llama _certificate pinning_ (para asegurarme que la página es la verdadera).

La verdad esto me costó bastante porque soy terrible en C++. Necesitaba convertir un String de hexadecimales en un arreglo de enteros.

`fingerprint_buf` es un String de la forma `ABCDABCDABCDABCDABCDABCDABCDABCDABCDABCD`.
```cpp
  uint8_t fingerprint [20] {0x0};
  std::string string_buf(fingerprint_buf);
  for (int i = 0; i < 40; i += 2) {
    fingerprint[i / 2] = std::stoi(string_buf.substr(i, 2).c_str(), 0, 16);
  }
```
Después hay que aplicar el fingerprint al cliente:
```cpp
client->setFingerprint(fingerprint);
```
Y comenzar la conexión pasando el cliente:
```cpp
https.begin(*client, HTTPS_URL)
```
Para realizar una petición GET:
```cpp
https.getString();
```
Para obtener efectivamente el contenido:
```cpp
https.GET();
```
Y todo junto:
```cpp
#include <ESP8266WiFi.h>
#include <WiFiClient.h>
#include <sstream>
#include <ESP8266HTTPClient.h>
#include <WiFiClientSecureBearSSL.h>
#include <ESP8266WiFiMulti.h>
#include "config.h"
using namespace std;

ESP8266WiFiMulti WiFiMulti;
HTTPClient https;

void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  WiFiMulti.addAP(nombreSSID, contraSSID);
  while ((WiFiMulti.run() != WL_CONNECTED)) {
    Serial.println(".");
  }
  std::unique_ptr<BearSSL::WiFiClientSecure>cliente(new BearSSL::WiFiClientSecure);

  uint8_t fingerprint [20] {0x0};
  std::string string_buf(fingerprint_buf);
  for (int i = 0; i < 40; i += 2) {
    fingerprint[i / 2] = std::stoi(string_buf.substr(i, 2).c_str(), 0, 16);
  }

  cliente->setFingerprint(fingerprint);
  if (https.begin(*cliente, HTTPS_URL)) {
    int httpCode = https.GET();
    if (httpCode > 0) {
      if (httpCode == HTTP_CODE_OK || httpCode == HTTP_CODE_MOVED_PERMANENTLY) {
        Serial.println("OK or MOVED");
        String payload = https.getString();
        Serial.println(payload);
        https.end();
      } else {
        Serial.println("Error HTTP");
      }
    } else {
      Serial.println("Error de conexión");
    }
  }
}
void loop() {

}
```

## Eso por ahora
Hasta el momento tengo la NodeMCU conectada al WiFi y una página HTTPS. Procesamiento: 0. Internet: 1. Cosa: 0.

