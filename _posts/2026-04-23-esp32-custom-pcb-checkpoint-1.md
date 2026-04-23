---
title: "ESP32 Custom PCB (KiCad)"
date: 2026-04-23
---

Building off my ESP32 Temperature & Humidity Web Server project, I decided to take the next step and design a custom PCB.

## Getting Started

First thing I did was download KiCad, and immediately ran into my first issue — figuring out which ESP32 board I actually had. I was in the schematic editor, assumed I should recreate my circuit in the software, opened up the symbol search and looked up "ESP32," and got hit with an entire list. Checked my Amazon listing (the exact ESP32 I bought and tried to read the description and specs) and got nowhere.
That's when I learned there are actual layers to this thing. There's the chip itself, then the module sitting between the chip and the dev board, and then the dev board I'd been using the whole time. Instead of digging through the product description trying to figure out which one I had, I wrote a quick Arduino sketch to just ask the board directly:

```cpp
cppvoid setup() {
  Serial.begin(115200);
  delay(3000);
  Serial.println(ESP.getChipModel());
  Serial.println(ESP.getChipRevision());
}
void loop() {}

Serial Monitor output confirmed it:
ets Jul 29 2019 12:21:46
rst:0x1 (POWERON_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:1
load:0x3fff0030,len:4876
ho 0 tail 12 room 4
load:0x40078000,len:16560
load:0x40080400,len:3500
entry 0x400805b4
ESP32-D0WD-V3
301
```

Then came another realization — if I'm going to order a custom PCB, shouldn't I order the ESP32 module first and build around that? Turns out these companies don't just give you an ESP32 chip when you order a custom PCB. And trying to find the bare chip or module on Amazon was a dead end anyway since everyone's just selling dev boards. With all of this piling up, being completely new to all of it, I just said screw it. A custom PCB is essentially my previous breadboard setup flattened into one board — that's literally all it is. So the plan became to just build the PCB around the dev board I already have, keep it simple, and get familiar with the tools and the process. The goal isn't to become a full-blown electrical engineer. More complex stuff with the module or bare chip can come later.
With that settled, the schematic really only needed two things: the DHT22 sensor and two Conn_01x15 headers. The DHT22 showed up in KiCad as the AM2302, which threw me off at first. The headers were originally suggested as Conn_01x19, but I looked at the schematic, counted 19 nodes, then looked at my actual dev board sitting on my desk and counted 15 pins. Changed them to Conn_01x15. From there I went on Amazon to cross-reference the pin diagram and also just looked at my physical board to copy the connections. Hit a snag where some wires were overlapping, which wasn't an issue on the breadboard since jumper wires aren't constrained to 2D. That's when I found out the PCB can actually be two layers, which solved that.
Got the components placed, wired everything up in the schematic editor, ran the electrical rules check and it came back clean. Then pushed it to the PCB editor and that's where things got weird — the two Conn_01x15 headers were just missing. Turned out I needed to assign footprints to them. For standard pin headers the right one is:
Connector_PinHeader_2.54mm:PinHeader_1x15_P2.54mm_Vertical
Fixed that, re-updated the PCB, and everything showed up. That's where I'm leaving off.
