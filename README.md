# Practica ultrasonico con DHT22 Y node-red
Este repositorio muestra como podemos programar una ESP32 con el sensor DHT11.

## Introducción

### Descripción

La ```Esp32``` la utilizamos en un entorno de adquision de datos, lo cual en esta practica ocuparemos la platafoma  (```node-red```) para adquirir temperatura y humedad del entorno; Cabe aclarar que esta practica se usara un simulador llamado [WOKWI](https://https://wokwi.com/). y plataforma [node-red] (http://localhost:1880/ui/#!/0?socketid=GastYhmOZGDVaQArAAAN)


## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/)
- [node-red] (http://localhost:1880/ui/#!/0?socketid=GastYhmOZGDVaQArAAAN)
- [node-red] (http://localhost:1880/#flow/33551a14c9f5e89f)
- Tarjeta ESP 32
- Sensor DHT22



## Instrucciones

### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/)
y as la plataforma  [node-red] (http://localhost:1880/ui/#!/0?socketid=GastYhmOZGDVaQArAAAN)()
 [node-red] (http://localhost:1880/#flow/33551a14c9f5e89f)


### Instrucciones de preparación de entorno 

1. Abrir la terminal de programación y colocar la siguente programación:

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "44.195.202.69";
const int mqttPort = 1883;
const char* mqttUser = "SergioRB";
const char* mqttPassword = "1234";
const char* mqttTopic = "SergioRB";

WiFiClient espClient;
PubSubClient client(espClient);

int ledPin = 13; // Pin del LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conectado a la red WiFi");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword)) {
      Serial.println("Conectado");
      client.subscribe(mqttTopic);
    } else {
      Serial.print("Error de conexión, rc=");
      Serial.print(client.state());
      Serial.println(" Intentando de nuevo en 5 segundos");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido: [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, mqttTopic) == 0) {
    if ((char)payload[0] == '1') {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  }
}


```
2. Ingresamos a la plataforma **node-red** y acemos el diagrama como se muestra en la siguente imagen.

![](https://github.com/ser2784/Practica-esp32-con-node-red/blob/main/Practica%20esp32%20con%20node-red%20diagrama%201.png)

3. Hacer la conexion de **node-red** con la **ESP32** como se muestra en la siguente imagen.

![]()

### Instrucciónes de operación

1. Iniciar simulador.
2. Visualizar los datos en el monitor serial.
3. Colocar la temperatura y humedad dando *doble click* al sensor **DHT22** 

## Resultados

Cuando haya funcionado, verás los valores dentro del monitor serial como se muestra en la siguente imagen.

![](https://github.com/ser2784/Practica-esp32-con-node-red/blob/main/Practica%20esp32%20con%20node-red%20graficos.png)
![](https://github.com/ser2784/Practica-esp32-con-node-red/blob/main/Practica%20esp32%20con%20node-red%20diagrama.png)



## Evidencias

[Video de Youtube](no contamos con el video ))


# Créditos

Desarrollado por Ing. Sergio RB

- [GitHub](https://github.com/ser2784)