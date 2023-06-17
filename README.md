# Sensor de DHT22 con node-red

## Instrucciones 
1. Colocar bloque ```mqqtt in```.

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/bloquemqtt.png?raw=true)

2. Configurar el bloque con el puerto mqtt con el ip ```52.57.167.175``` como se muestra en la imagen.

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/Ipmqtt.png?raw=true)

3. Colocar el bloque json y configurarlo como se muestra en la imagen.

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/JSON.png?raw=true)

4. Colocamos dos bloques function y lo configuramos con el siguente codigo.

```
msg.payload = msg.payload.TEMPERATURA;
msg.topic = "TEMPERATURA";
return msg;
```

```
msg.payload = msg.payload.HUMEDAD;
msg.topic = "HUMEDAD";
return msg;
```

5. Colocamos los bloques de chart y gauge.

## Programación en ESP32
```
#include <ArduinoJson.h>
#include <WiFi.h>
#include <PubSubClient.h>
#define BUILTIN_LED 2
#include<max6675.h>
int CSK=13;
int CS=12;
int SO=14;
MAX6675 termopar(CSK,CS,SO);
// Update these with values suitable for your network.

const char* ssid = "Eventos_FCQeI";
const char* password = "2023FCQEI";
const char* mqtt_server = "52.57.167.175";
String username_mqtt="educatronicosiot";
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
}

void loop() {

int temperatura;
 temperatura=termopar.readCelsius();
delay(1000);

  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  unsigned long now = millis();
  if (now - lastMsg > 30000) {
    lastMsg = now;
    //++value;
    //snprintf (msg, MSG_BUFFER_SIZE, "hello world #%ld", value);

    StaticJsonDocument<128> doc;

    doc["DEVICE"] = "ESP32";
    //doc["Anho"] = 2022;
    //doc["Empresa"] = "Educatronicos";
    doc["TEMPERATURA"] = temperatura;
    doc["AMPERAJE"] = random(0);
   

    String output;
    
    serializeJson(doc, output);

    Serial.print("Publish message: ");
    Serial.println(output);
    client.publish("Tamulbaout", output.c_str());
  }
}
```

## Resultados

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/Resultado.png?raw=true)

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/Resultados2.png?raw=true)