---
title: "ESP32 Custom PCB (KiCad) Part 2"
date: 2026-04-24
---

PCB layout is where the schematic stops being abstract. Coming into today I was already in the PCB editor with everything placed, so the first order of business was figuring out spacing. Unlike the schematic editor, physical dimensions actually matter here.

After using Claude to parse through the Amazon listing for my ELEGOO ESP32, I found out it's a standard 30-pin board with pins spaced 2.54mm apart and the two rows sitting 25.4mm apart.

![](/assets/images/img2env.jpg)

While I was poking around the DHT22 footprint I also noticed it had 4 pins in the PCB editor even though the schematic only shows 3. Turns out pin 3 is labeled NC, which stands for Not Connected, and serves no electrical purpose. It's just a placeholder pin to give the sensor a more stable, balanced fit on the board.

![](/assets/images/img6env.jpg)

Got the connectors spaced correctly, placed the AM2302, drew the board outline using the Edge.Cuts layer, and routed the traces. Everything fit on a single layer. The unrouted connections shown as thin diagonal lines are called ratsnest lines, which is the actual industry term for them.

![](/assets/images/img3env.jpg)

Running the DRC after routing came back with one error, which was the NC pin flagged as unconnected. Since it's intended to be unconnected, I ignored it.

![](/assets/images/img4env.jpg)

Then I opened the 3D viewer, which is useful for catching physical issues that aren't visible in the flat PCB editor. As it turned out, I had placed male pin headers for both J1 and J2, which won't work since the dev board itself has male pins.

![](/assets/images/img5env.jpg)

The fix was straightforward. I opened properties on each connector in the PCB editor and swapped the footprint from PinHeader to PinSocket, changing them to female headers.

![](/assets/images/img1env.jpg)

Female headers, problem solved.

![](/assets/images/img7env.jpg)

After that it was just a matter of exporting the Gerber files, generating the drill files, and zipping everything up.

![](/assets/images/img8env.jpg)

Uploaded the zip to JLCPCB, which auto-detected the board dimensions and layer count. Five boards for C$2.73, shipped to Canada for another couple dollars with a coupon applied.

![](/assets/images/img9env.jpg)

Order total came out to $3.50. They're probably losing money on this order.

![](/assets/images/img10env.jpg)
