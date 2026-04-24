---
title: "ESP32 Custom PCB (KiCad) Part 2"
date: 2026-04-24
---

Looking at this diagram I wondered why the fuck there are 4 pins on the PCB Editor footprint of the DHT22. I look at my DHT22 IRL and as u can see on the schematic editor that shit only has 3 pins the fuck Lol. As it turns out Pin 3 (NC) literally does nothing — it's just a placeholder to make the sensor physically stable in a breadboard or PCB. Having 4 pins instead of 3 gives it a more balanced, secure fit:

![](/assets/images/esp32-custom-pcb-part-2-img-1.jpg)

Also ran into a problem in which I wondered shit - now that im in the PCB editor - shouldn’t spacing matters? As it turns out yes the fuck it does. Used Claude to help parse the entire Amazon description of the ESP32 dev board I ordered and I learned that For spacing, my ELEGOO ESP32 is a standard 30-pin board. Those typically have pins spaced 2.54mm apart (standard) and the two rows are 25.4mm (1 inch) apart edge-to-edge because electronics standardized on 2.54mm pitch, which is exactly 0.1 inches. It's a legacy from when the US dominated electronics manufacturing and everything was designed in imperial units.

![](/assets/images/esp32-custom-pcb-part-2-img-2.jpg)

I also learned that the blue lines are called ratsnest lines. But anyways yeah I finished placement of the AM3202, used edge.cuts to make the board, and finished tracing. Turns out I didn’t actually need two layers, the tracing was fine. IIt was also at this stage that I Noticed that 90 degree bends aren’t common. Turns out From what I understand, historically 90 degree traces were avoided since they could become acid traps during etching, which is why the PCB Editor automatically makes these lines 45 degrees:

![](/assets/images/esp32-custom-pcb-part-2-img-3.jpg)

Problem - finally opened the 3D editor because apparently you’d catch things here you don’t see on the PCB / schematic editor. What do you know my fucking dumbass implemented male rails which won’t connect to my dev board full of male pins. Quick fix I just changed the footprint from PinHeader to PinSocket. Doing the same for J2:

![](/assets/images/esp32-custom-pcb-part-2-img-4.jpg)
![](/assets/images/esp32-custom-pcb-part-2-img-5.jpg)
![](/assets/images/esp32-custom-pcb-part-2-img-6.jpg)

So yeah. Everything was finished after. Just needed to create the gerber files, zip it, and upload it to JLCPCB. Everything turned out to be $3.50 CAD.

![](/assets/images/esp32-custom-pcb-part-2-img-7.jpg)
![](/assets/images/esp32-custom-pcb-part-2-img-8.jpg)
