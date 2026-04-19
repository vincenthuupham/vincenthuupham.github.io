The fix was to move the component off GPIO2. Other strapping pins to avoid during upload are GPIO0 and GPIO15.

## Problems Encountered

- **DHT library dependency** — installing the DHT sensor library required an additional dependency (Adafruit Unified Sensor), resolved by clicking Install All when prompted
- **Upload failure** — ESP32 not entering boot mode, resolved by holding the BOOT button during upload
- **Strapping pin conflict** — component wired to GPIO2 prevented upload, resolved by moving it to a different pin
- **Sensor reading nan** — power and ground wires on the DHT22 were swapped, resolved by correcting the wiring
- **Serial output on one line** — replaced `Serial.print()` with `Serial.println()` on the last print statement

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

- `#include <DHT.h>` — imports the DHT sensor library
- `DHT dht(5, DHT22)` — creates a DHT22 sensor object on GPIO5
- `#define FAN 18` — assigns GPIO18 the name FAN
- `dht.begin()` — initializes the sensor on startup
- `pinMode(FAN, OUTPUT)` — configures GPIO18 as an output to control the fan
- `dht.readTemperature()` and `dht.readHumidity()` — read current values from the sensor every loop
- `if (temp > 25)` — turns the fan on above 25°C, off otherwise
- `Serial.println()` — prints readings to the serial monitor for debugging
