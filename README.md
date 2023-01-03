
<h1 align="center">Laporan Praktikum Sistem Embedded</h1>
<p>Disusun Oleh:</p>

<ul>
  <li>Naufal Faterna Zakky 4.31.20.0.17</li>
</ul>

<h3>Daftar Jobsheet</h3>
<p></p>

<ul>
  <li><a href="https://github.com/naufalzakky/JobSheet1">JobSheet1</a></li>
  <li><a href="https://github.com/naufalzakky/JobSheet1.1">JobSheet1.1</a></li>
  <li><a href="https://github.com/naufalzakky/JobSheet2">JobSheet2</a></li>
  <li><a href="https://github.com/naufalzakky/JobSheet3">JobSheet3</a></li>
  <li><a href="https://github.com/naufalzakky/Nomor1">Nomor 1</a></li>
  <li><a href="https://github.com/naufalzakky/Nomor2">Nomor 2</a></li>
  <li><a href="https://github.com/naufalzakky/Nomor3">Nomor 3</a></li>
  <li><a href="https://github.com/naufalzakky/Nomor4">Nomor 4</a></li>
</ul>


<h2 align="center">Tugas Nomor2</h2>
<h3>Coding</h3>
<p>adafruit</p>
<pre>
<code>
#include <WiFi.h>

#include "Adafruit_MQTT.h"

#include "Adafruit_MQTT_Client.h"

#define WLAN_SSID       "kos putri"

#define WLAN_PASS       "123456788"

#define AIO_SERVER      "io.adafruit.com"

#define AIO_SERVERPORT  1883                  

#define AIO_USERNAME    "rindangoktaviani"

#define AIO_KEY         "aio_Rusa02DaEppjxLMpsN3oxsBKQ1IM"

int output=2;

WiFiClient client;     // Create an ESP8266 WiFiClient class to connect to the MQTT server.

Adafruit_MQTT_Client mqtt(&client, AIO_SERVER, AIO_SERVERPORT, AIO_USERNAME, AIO_KEY);        // Setup the MQTT client class by passing in the WiFi client and MQTT server and login details.

Adafruit_MQTT_Subscribe lampu1 = Adafruit_MQTT_Subscribe(&mqtt, AIO_USERNAME "/feeds/lampu1");

void MQTT_connect();

void setup() {

  Serial.begin(115200);

  delay(10);

pinMode(2,OUTPUT);

 // Connect to WiFi access point.

  Serial.println(); Serial.println();

  Serial.print("Connecting to ");

  Serial.println(WLAN_SSID);

 

  WiFi.begin(WLAN_SSID, WLAN_PASS);

  while (WiFi.status() != WL_CONNECTED) {

    delay(500);

    Serial.print(".");

  }

  Serial.println();

 Serial.println("WiFi connected");

  Serial.println("IP address: "); Serial.println(WiFi.localIP());

  mqtt.subscribe(&lampu1);

}

uint32_t x=0;

void loop() {

   MQTT_connect();

Adafruit_MQTT_Subscribe *subscription;

  while ((subscription = mqtt.readSubscription(5000))) {

    if (subscription == &lampu1) {

      Serial.print(F("Got: "));

      Serial.println((char *)lampu1.lastread);

       if (!strcmp((char*) lampu1.lastread, "ON"))

      {

        digitalWrite(2, HIGH);

      }

      else

      {

        digitalWrite(2, LOW);

      }

    }

  }

 

 

}

void MQTT_connect() {

  int8_t ret;

  // Stop if already connected.

  if (mqtt.connected()) {

    return;

  }

 Serial.print("Connecting to MQTT... ");

uint8_t retries = 3;

  while ((ret = mqtt.connect()) != 0) { // connect will return 0 for connected

       Serial.println(mqtt.connectErrorString(ret));

       Serial.println("Retrying MQTT connection in 5 seconds...");

       mqtt.disconnect();

       delay(5000);  // wait 5 seconds

       retries--;

       if (retries == 0) {

         // basically die and wait for WDT to reset me

         while (1);

       }

  }

  Serial.println("MQTT Connected!");

}

</code>
</pre>

<h3>Hasil</h3>
https://github.com/naufalzakky/Nomor2/blob/main/WhatsApp%20Image%202023-01-03%20at%2018.55.30.jpeg

https://github.com/naufalzakky/Nomor2/blob/main/WhatsApp%20Image%202023-01-03%20at%2018.56.04.jpeg

https://github.com/naufalzakky/Nomor2/blob/main/WhatsApp%20Image%202023-01-03%20at%2021.14.48.jpeg
