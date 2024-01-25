# P8_NR_DHT22
## INTRODUCCION

### DESCRIPCION 
.
La **Esp32**  es un dispositivo electronico en el cual podemos realizar practicas y simulaciones, en esta practica realizaremos  con  la ayuda de un servidor en linea la simulacion de una conexion wifi entre nuestra tarjeta y  **Node-red**, esta accion nos permitira visualizar en tiempo real nuestras graficas de acuerdo a los valores detectados por nuestros sensores. Cabe aclarar que para esta practica se usara un simulador llamado **https://wokwi.com/**.

### MATERIAL A OCUPAR

Para realizar esta practica ocuparemos lo siguente:

-WORKI

-Tarjeta ESP 32

-DTH22

-Node red

-Un servidor virtual que nos genere una IP

### Requisitos previos

Para poder realizar esta practica es nesesario contar con conosimientos basicos de programacion y electronica.

## INSTRUCIOES DE ELAVORACION 

1.Buscar un servidor virtual y generar una IP:

 a) Entraremos al siguiente link **(https://www.hivemq.com/mqtt/public-mqtt-broker/)** y copiaremos la direccion señalada
 ![](https://github.com/nijs17/P8_NR_DHT22/blob/main/W1.png)
 b)Abriremos el simbolo del sistema y escribimos **nslookup**  seguido del link anteriormente copiado **broker.hivemq.com** esta accion nos generara una IP, copiamos esa IP
 ![](https://github.com/nijs17/P8_NR_DHT22/blob/main/W2.png) 
 2. Armar el proyecto en node red.
 
 Acontinuacion se muestran los cambios que se le tiene que hacer al proyecto, en los que se encuentran pegar la IP y cambiar el nombre al topico, elegir al leguaje de programacion que se va a convertir, programar las funciones  y editar los valores de los indicadores y sensores como se muestra en la imagen.
 ![](https://github.com/nijs17/P8_NR_DHT22/blob/main/w3.png)
 3. Abrir la terminal de programación y colocar el siguente codigo:
 

 
 #include <ArduinoJson.h>

#include <WiFi.h>

#include <PubSubClient.h>

#define BUILTIN_LED 2

#include "DHTesp.h"

const int DHT_PIN = 15;

DHTesp dhtSensor;

// Update these with values suitable for your network.

const char* ssid = "Wokwi-GUEST";

const char* password = "";

const char* mqtt_server = "3.65.168.153";

String username_mqtt="NessJ";

String password_mqtt="12345678";

WiFiClient espClient;

PubSubClient client(espClient);

unsigned long lastMsg = 0;

#define MSG_BUFFER_SIZE  (50)

char msg[MSG_BUFFER_SIZE];

int value = 0;

void setup_wifi() {

  delay(10);

  // We start by connecting to a WiFi network
  
  Serial.println();
  
  Serial.print("Connecting to ");
  
  Serial.println(ssid);

  WiFi.mode(WIFI_STA);
  
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
  
    delay(500);
    
    Serial.print(".");
 
  }

  randomSeed(micros());

  Serial.println("");
  
  Serial.println("WiFi connected");
  
  Serial.println("IP address: ");
  
  Serial.println(WiFi.localIP());

}

void callback(char* topic, byte* payload, unsigned int length) {

  Serial.print("Message arrived [");
  
  Serial.print(topic);
  
  Serial.print("] ");
  
  for (int i = 0; i < length; i++) {
  
    Serial.print((char)payload[i]);
 
  }
  
  Serial.println();

  // Switch on the LED if an 1 was received as first character
  
  if ((char)payload[0] == '1') {
  
    digitalWrite(BUILTIN_LED, LOW);   
    
    // Turn the LED on (Note that LOW is the voltage level
    
    // but actually the LED is on; this is because
    
    // it is active low on the ESP-01)
  
  } else {
  
    digitalWrite(BUILTIN_LED, HIGH);  
    
    // Turn the LED off by making the voltage HIGH
 
  }

}

void reconnect() {
  
  // Loop until we're reconnected
  
  while (!client.connected()) {
  
    Serial.print("Attempting MQTT connection...");
    
    // Create a random client ID
    
    String clientId = "ESP8266Client-";
    
    clientId += String(random(0xffff), HEX);
    
    // Attempt to connect
    
    if (client.connect(clientId.c_str(), username_mqtt.c_str() , password_mqtt.c_str())) {
    
      Serial.println("connected");
      
      // Once connected, publish an announcement...
      
      client.publish("outTopic", "hello world");
      
      // ... and resubscribe
      
      client.subscribe("inTopic");
    
    } else {
    
      Serial.print("failed, rc=");
      
      Serial.print(client.state());
      
      Serial.println(" try again in 5 seconds");
      
      // Wait 5 seconds before retrying
      
      delay(5000);
    
    }
  
  }

}

void setup() {
  
  pinMode(BUILTIN_LED, OUTPUT);     // Initialize the BUILTIN_LED pin as an output
  
  Serial.begin(115200);
  
  setup_wifi();
  
  client.setServer(mqtt_server, 1883);
  
  client.setCallback(callback);
  
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);

}

void loop() {


delay(1000);

TempAndHumidity  data = dhtSensor.getTempAndHumidity();

  if (!client.connected()) {
  
    reconnect();
 
  }

  client.loop();


  
  unsigned long now = millis();
  
  if (now - lastMsg > 2000) {
  
    lastMsg = now;
    
    //++value;
    
    //snprintf (msg, MSG_BUFFER_SIZE, "hello world #%ld", value);

    
    StaticJsonDocument<128> doc;

    doc["DEVICE"] = "ESP32";
    
    //doc["Anho"] = 2022;
    
    //doc["Empresa"] = "Educatronicos";
    
    doc["TEMPERATURA"] = String(data.temperature, 1);
    
    doc["HUMEDAD"] = String(data.humidity, 1);
   

    String output;
    
    serializeJson(doc, output);

    Serial.print("Publish message: ");
    
    Serial.println(output);
    
    Serial.println(output.c_str());
    
    client.publish("Ness", output.c_str());
 
  }

}

4. Hacer las conexiones de la tarjeta con los demas elementos, anteriormente mensionados,  y agregar las librerias como se muestra en la siguente imagen
 ![](https://github.com/nijs17/P8_NR_DHT22/blob/main/w4.png)
## INSTRUCCIONES DE OPERACION 


    1. Iniciar simulador.
    
    2. Realizar las conexiones mostradas en la imagen anterior.
    
    3. Mover la sencibilidad del sensor para poder visualizar los resultados de manera grafica.
    
## RESUSLTADOS
Al finalizar tendremos nuestra tarjeta conectada con nuestro servidor en linea y podremos visualizar en tiempo real los datos de los sensores, como se muestra en la siguiente imagen
![](https://github.com/nijs17/P8_NR_DHT22/blob/main/w5.png)
