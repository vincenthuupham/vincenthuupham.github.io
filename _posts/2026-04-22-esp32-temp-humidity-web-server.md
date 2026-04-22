---
title: "ESP32 Temperature & Humidity Web Server"
date: 2026-04-22
---

This project builds off my previous ESP32 temperature controlled fan by stripping it down and focusing on one thing: serving live sensor data over WiFi through a webpage.

## What I Changed and Why

I removed the fan entirely to keep the focus clean. The previous project already proved the sensor and threshold logic worked. What I wanted to test here was whether I could serve that data over a network in real time.

## Circuit

Same DHT22 wiring as before, just without the fan circuit. Data line to GPIO15, power to the 3.3V pin, ground to GND.

## How It Works

The ESP32 connects to WiFi on boot and starts an HTTP server on port 80. When a browser connects to the ESP32's local IP address, it reads the temperature and humidity from the DHT22 and sends back a minimal HTML page with the values. The page refreshes automatically every 3 seconds.

There's no JavaScript, no external hosting, no frameworks. The entire website is a few `client.print()` lines running on the ESP32 itself.

## Troubleshooting

The site would stop loading after a while. The ESP32 was getting stuck in its client loop if a browser connected but didn't finish sending its request cleanly. The fix was adding a 2 second timeout so if the request doesn't complete in time, the ESP32 bails out and moves on instead of hanging forever.

Readings were showing `nan`. I had moved the DHT22 from GPIO5 to GPIO15 on the physical circuit for organizational reasons but forgot to update the pin number in the code. Updating `DHT dht(5, DHT22)` to `DHT dht(15, DHT22)` fixed it immediately.

## AI Usage

I used Claude throughout this project as a development tool. The base code structure, the timeout fix, and the debugging approach were all done with Claude in the loop. That said, the actual troubleshooting decisions were mine. Figuring out that the site was hanging, suspecting the client loop, catching the GPIO mismatch. Claude helped me move faster but the problem solving was hands on.

I think that's worth being transparent about. Using AI to accelerate a project is a real skill and pretending otherwise doesn't help anyone.

## Code

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include <DHT.h>

const char *ssid = "yourssid";
const char *password = "yourpassword";

DHT dht(15, DHT22);
NetworkServer server(80);

void setup() {
  Serial.begin(115200);
  dht.begin();
  delay(2000);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) delay(500);
  Serial.println(WiFi.localIP());
  server.begin();
}

void loop() {
  NetworkClient client = server.accept();
  if (!client) return;

  unsigned long timeout = millis();
  String req = "";

  while (client.connected()) {
    if (millis() - timeout > 2000) break;
    if (!client.available()) continue;
    char c = client.read();
    req += c;
    if (req.endsWith("\r\n\r\n")) {
      float temp = dht.readTemperature();
      float humidity = dht.readHumidity();
      client.println("HTTP/1.1 200 OK");
      client.println("Content-type:text/html");
      client.println();
      client.println("<meta http-equiv='refresh' content='3'>");
      client.print("Temp: ");
      client.print(temp);
      client.println(" C<br>");
      client.print("Humidity: ");
      client.print(humidity);
      client.println(" %");
      break;
    }
  }
  client.stop();
}
\```

## What I Learned

How HTTP actually works at a low level. Writing raw `client.println("HTTP/1.1 200 OK")` makes it click in a way that using a framework never would. The browser is just waiting for that exact string and the ESP32 is the one sending it.

How servers can hang. The timeout issue was a good lesson in what happens when a server doesn't handle bad or incomplete connections. Without it the ESP32 froze and ignored everything else.

That barebones is fine. The whole site is a handful of lines and it works. Not everything needs a library or a framework.
```
