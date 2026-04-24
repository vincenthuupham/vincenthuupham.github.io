---
title: "ESP32 Custom PCB (KiCad) Part 2"
date: 2026-04-24
---

While reviewing the diagram, I initially questioned why the PCB footprint for the DHT22 included four pins when the physical sensor and schematic representation clearly show only three. Upon further investigation, I learned that the third pin is designated as NC (no connection) and does not serve any electrical function. Instead, it exists purely for mechanical stability. Including a fourth pin allows the component to sit more securely in a breadboard or PCB, providing better balance and physical support.

![](/assets/images/esp32-custom-pcb-part-2-img-1.jpg)

During the transition into the PCB editor, I also realized that spacing is a critical consideration. To better understand the exact dimensions of my ESP32 development board, I used Claude to parse the product description from Amazon. From this, I confirmed that my ELEGOO ESP32 is a standard 30-pin board.

These boards typically use a 2.54 mm pin pitch (0.1 inches), with the two rows spaced 25.4 mm (1 inch) apart edge-to-edge. This standard originates from legacy imperial measurements used during the early dominance of U.S. electronics manufacturing.

![](/assets/images/esp32-custom-pcb-part-2-img-2.jpg)

I also became familiar with “ratsnest lines,” the blue lines shown in the PCB editor that indicate unconnected electrical nets. After completing component placement for the AM3202, defining the board outline using the Edge.Cuts layer, and routing the traces, I found that a two-layer board was unnecessary, as all routing could be completed cleanly on a single layer.

At this stage, I also noticed that 90-degree trace bends are generally avoided. Historically, sharp corners could create “acid traps” during the etching process, potentially leading to manufacturing defects. As a result, PCB design tools typically favor 45-degree trace angles, which I adopted in my layout.

![](/assets/images/esp32-custom-pcb-part-2-img-3.jpg)

When I opened the 3D viewer to validate the design, I identified a critical oversight: I had selected male pin headers for connectors intended to interface with a development board that already uses male pins. This would have prevented proper physical connection. I corrected this by replacing the PinHeader footprints with PinSocket footprints for both J1 and J2, ensuring compatibility.

![](/assets/images/esp32-custom-pcb-part-2-img-4.jpg)
![](/assets/images/esp32-custom-pcb-part-2-img-5.jpg)
![](/assets/images/esp32-custom-pcb-part-2-img-6.jpg)

With these adjustments complete, the design was finalized. I generated the Gerber files, compressed them into a ZIP archive, and submitted the design to JLCPCB for fabrication. The total manufacturing cost came to approximately $3.50 CAD.

![](/assets/images/esp32-custom-pcb-part-2-img-7.jpg)
![](/assets/images/esp32-custom-pcb-part-2-img-8.jpg)
