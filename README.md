### ESP32-C3 SuperMini to HU-058 MCU footprint wiring

Based on the clock project done by Adrian from the youtube channel Adrians digital basement https://youtu.be/vgfV_IGmhGA?si=nM0Irj1oQnthL4y8

This project is based on the cheaper of the 2 clocks the one that just uses the rtc chip and STC8G1K17 the code was changed from the original repo using claude so that it would fully rely only on the esp32 super mini and not have to rely on home assistant or anything further and just works stand alone.

![Finished clock](/docs/Z7IUYBY0R0WqkUyBRZgtmQ.jpg?raw=true)

From the kit all you will need is to solder the following items onto the pcb

## Install from the kit

- **SW1, SW2** buttons
- **USB1** micro USB jack (power in)
- **C3** 100 nF, beside the MCU footprint: decouples +5V where the SuperMini lands
- **C4** 100 nF: decoupling on the +5V rail

The display side comes assembled. Nothing else is needed. Solder all those parts first then I recommend some uv cure glue the first step will be to cover the power contacts on the original board we will be powering this clock using the esp32 itself.

![Soldered Parts](/docs/gvIaahmUTU2siD2z8HN_Lg.jpg?raw=true)

![UV Adhisive](/docs/3SfKDDNGSdSF7FNnv2HPgA.jpg?raw=true)

![UV Cure](/docs/ip3M33_eRe6IRE2ybzLQYQ.jpg?raw=true)

Follow the diagram below to hook up the esp32 super mini to the board and change the ssid and password in the firmware to your own and flash it to the board using arduino studio

![UV Cure](/docs/hu058-wiring.svg?raw=true)

## Wire list

| Wire | SuperMini | Clock board | Signal | Notes |
|---|---|---|---|---|
| Blue | `GPIO4` | `DIP pin 16` | CLK, driver 1 (hours) | Not pin 14: that is the DS1302 RST line on this board |
| Green | `GPIO5` | `DIP pin 5` | DATA, driver 1 (hours) | |
| Blue | `GPIO6` | `DIP pin 1` | CLK_1, driver 2 (minutes) | Pin 1 is the notch / square-pad end |
| Green | `GPIO7` | `DIP pin 2` | DATA_1, driver 2 (minutes) | |
| Orange | `GPIO3` | `DIP pin 9` | S1, top button | Button marked SW2 on the board |
| Orange | `GPIO10` | `DIP pin 10` | S2, bottom button | Button marked SW1 on the board |
| Red | `5V` | `DIP pin 6` | +5V (VCC) in | Clock's micro USB feeds the panel and the SuperMini |
| Black | `GND` | `DIP pin 8` | Ground | |

Header P3 is not needed. Its pins, from the schematic, are 1 +5V, 2 GND, 3 S2, 4 S1.

once it is wired in you can position the esp32 super mini in the corner of the board where the power usb would normally go and use uv adhesive again to bond the super mini to the other circuit board.

![UV Cure](/docs/mryWDEfGQfSNnCoLofs8OQ.jpg?raw=true)

After curing the uv adhesive the boards should not move and be locked together and you are almost done you will need to cut away a bit from the case to fit the new height of the port and be careful you cut the right parts you can see in the picture you will need to cut 1 of the screw posts and the plastic wall area if you want to be neat you can spend time with a rotary tool to make it fit or use side cutters like i did.

![UV Cure](/docs/c1F0HmMaQquywnAZ0_dBHA.jpg?raw=true)

Please note this may not do all that the original firmware Adrian made its more of a compact version of it to just tell the time and stuff and control the rgb. To control everything you can go to the clocks ip or use the mdns that is set in the firmware. If you build more then 1 of these clocks i recommend changing the mdns name so they do not clash on the network. Finally once done you can screw the whole thing back together.

## Buttons

| Button | Action | What it does |
|---|---|---|
| **S1** (top, marked SW2) | Tap | Next brightness step: 5, 15, 30, 50, 75, 100 % (shows `b 50` etc.). Also turns the display back on if it was off. |
| **S1** | Hold 1 s | Display on / off |
| **S2** (bottom, marked SW1) | Tap | Next effect: none, colour cycle, flash (shows `EF 0`, `EF 1`, `EF 2`) |
| **S2** | Hold 1 s | Show the IP address one number at a time, or `no nEt` if Wi-Fi is down |
| **Both** | Hold 10 s | Factory reset. A countdown appears after 2 s; let go before it ends to cancel. Clears all web page settings; Wi-Fi is kept because it is set in the sketch. |

Brightness and effect changes made with the buttons are saved about 5 seconds after the last press.

### Issues and notes

I have not tested the ota update feature so not sure if it works and most of the other features in the firmware besides ntp accuracy and changing colors and such.

Note: that i use uv cure adhesive i recommend this over superglue as it works better to bond the 2 pcb boards together but you will need the glue and some sort of uv cure light to cure said adhisive.
Note: For my testing I am using my own local gps based stratum1 ntp server but the code published to the repo uses the nrc public ntp servers instead this was done just to test accuracy while building the firmware and is fully unnecessary for the clock to actually function as by default it will use public ntp servers instead to get its time.


