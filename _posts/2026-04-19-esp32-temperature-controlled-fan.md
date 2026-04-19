---
title: "ESP32 Temperature Controlled Fan"
date: 2026-04-19
---

This project builds on my first ESP32 experiment by combining two circuits into one functional system: a DHT22 temperature and humidity sensor, and a DC fan controlled by the ESP32 based on live temperature readings.

## Circuit

![ESP32 Temperature Controlled Fan](/assets/images/esptempfanmain.jpg)

## How It Works

The DHT22 sensor reads the ambient temperature every 2 seconds and sends the value to the ESP32 over a single data line. The ESP32 evaluates the reading in code. If the temperature exceeds 25°C it drives the fan pin HIGH, turning the fan on. Otherwise it drives it LOW, turning the fan off.

The fan and sensor are wired independently. They share the same GND and 3.3V pins on the ESP32, but their data lines go to separate GPIO pins. The connection between them exists only in code.

## What I Learned

Fans don't need current-limiting resistors. When I first tried replacing the LED from my blink project with a fan, nothing happened. After some troubleshooting I suspected the resistor was choking the current, removed it, and the fan started working. Unlike LEDs, fans regulate their own current draw and don't require a resistor in series.

GPIO2 is a strapping pin. When I tried uploading code with something wired to GPIO2 (D2), I kept getting a boot mode error. The ESP32 uses certain pins, GPIO0, GPIO2, and GPIO15, to determine how it boots. Having an external connection on GPIO2 was interfering with the upload. Disconnecting it resolved the issue immediately.

Sensor wiring order matters. My DHT22 was returning `nan` for both temperature and humidity. The root cause was simple, the power and ground wires were swapped. Correcting the wiring fixed the readings instantly.

Serial.print vs Serial.println. All my sensor readings were printing on one continuous line in the Serial Monitor. Replacing the final `Serial.print()` with `Serial.println()` added a newline after each reading, matching the expected output format.

## Code

```cpp
#include <DHT.h>

#define FAN 18

DHT dht(5, DHT22);

void setup() {
  dht.begin();
  delay(2000);
  Serial.begin(115200);
  pinMode(FAN, OUTPUT);
}

void loop() {
  float temp = dht.readTemperature();
  float humidity = dht.readHumidity();

  if (temp > 25) {
    digitalWrite(FAN, HIGH);
  } else {
    digitalWrite(FAN, LOW);
  }

  Serial.print("Temp: ");
  Serial.print(temp);
  Serial.print(" C ");
  Serial.print("Humidity: ");
  Serial.print(humidity);
  Serial.println(" % ");
  delay(2000);
}
```

## Code Walkthrough

- `#include <DHT.h>` — imports the Adafruit DHT library
- `DHT dht(5, DHT22)` — creates a DHT object on GPIO5, configured for the DHT22 sensor
- `dht.begin()` — initializes the sensor on startup
- `Serial.begin(115200)` — opens the serial connection at 115200 baud for monitoring
- `dht.readTemperature()` / `dht.readHumidity()` — reads current values from the sensor
- `if (temp > 25)` — threshold logic that drives the fan pin HIGH or LOW accordingly
- `delay(2000)` — waits 2 seconds between readings, matching the DHT22's minimum sampling interval
