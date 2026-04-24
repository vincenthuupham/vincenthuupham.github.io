---
title: "ESP32 Custom PCB (KiCad) Part 2"
date: 2026-04-24
---

Coming into today I was already in the PCB editor with everything placed, so the first order of business was figuring out spacing. Turns out it absolutely matters. After parsing through the Amazon listing for my ELEGOO ESP32, I found out it's a standard 30-pin board with pins spaced 2.54mm apart and the two rows sitting 25.4mm apart. That 25.4mm is exactly 1 inch, which is not a coincidence. Electronics standardized on 2.54mm pitch because it's exactly 0.1 inches, a legacy from when the US dominated electronics manufacturing and everything was designed in imperial units. So the metric numbers are just imperial in disguise and we're all just stuck with it forever.

[image 2 - positioning J1 relative to J2 with the 25.4mm offset dialog]

While I was poking around the DHT22 footprint I also noticed it had 4 pins in the PCB editor even though the schematic only shows 3. Turns out pin 3 is labeled NC, which stands for Not Connected, and literally does nothing. It's just a placeholder pin to give the sensor a more stable, balanced fit on the board. The name says it all.

[image 1 - DHT22 footprint showing 4 pins in the PCB editor side by side with the schematic showing 3]

Got the connectors spaced correctly, placed the AM2302, drew the board outline using the Edge.Cuts layer, and routed the traces. Turned out I didn't actually need two layers, everything fit on one. The unrouted connections shown as thin diagonal lines are called ratsnest lines, which is genuinely what the industry calls them.

[image 3 - active trace routing in the PCB editor]
Running the DRC after routing came back with one error, which was the NC pin flagged as unconnected. Since it's literally supposed to be unconnected, I just ignored it.

[image 4 - DRC showing the NC pin error]

Then I opened the 3D viewer, which I'd heard was useful for catching things you'd miss in the flat PCB editor. Good thing I did, because my dumbass had placed male pin headers for both J1 and J2. My dev board has male pins. You can't plug male into male.

[image 5 - 3D viewer showing the male pin headers before the fix]

The fix was simple. I just opened properties on each connector in the PCB editor and swapped the footprint from PinHeader to PinSocket.

[image 6 - footprint chooser swapping PinHeader to PinSocket]

Female headers, problem solved.

[image 7 - 3D viewer after the fix showing female sockets]

After that it was just a matter of exporting the Gerber files and generating the drill files, then zipping everything up.

[image 8 - Gerber and drill file export dialog]

Uploaded the zip to JLCPCB, which auto-detected the board dimensions and layer count. Five boards for C$2.73, shipped to Canada for another couple bucks with a coupon applied.

[image 9 - JLCPCB order page with board previewed and price shown]

Order total came out to $3.50. I'm pretty sure they lost money on this.

[image 10 - order confirmation showing $3.50 total]
