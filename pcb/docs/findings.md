# Glowie PCB findings

## mainboard v0 rev A

### [6] Button short press
*rev A2: Decreased R23 to 10k, increased C17 to 1uF. Used 1uF as that's what was available. Could probably be tuned down in the future*

The device does not latch on a short button press.
The design already features a 0.1uF capacitor (C17) at the LDO enable net to keep the LDO enabled for longer on a short button press, but it does not provide enough time for the MCU to boot and latch.
--> Increase the value of C17 (1uF works fine, but could probably be tuned down)

**Note:** C17 currently charges through a 100k resistor (R23). Therefore, a cap too large may not have time to charge on a short button press, and/or introduce a delay before the LDO gets enabled, especially at low battery voltage. If this turns out to be an issue, tune the cap value, and/or move it to the other side of the 100k resistor or decrease the resistor to 10k.

### [5] ~~Force shutdown delay~~
*rev A2: Increased R25 to 1M*

Using R25=510k, C18=22uF, force shutdown delay is too short (~4s at vbatt=3.7v).

--> Increase value of R25. Using R25=1M gives a delay of ~8s at vbatt=3.7v

### [4] ~~Power path mosfet pulldown~~
*fixed in rev A2*

There is 0.5-1v at the USB 5V net with USB disconnected, due to leakage current in the powerpath diode.
This causes 0.3-0.7mV at LDO enable, which gets too close to LDO en low threshold.
--> Decrease powerpath pulldown to 100k. Leakage current should still be low enough for shutdown current requirements.

Experimental findings for pulldown values:
100k -> 180mV at 5V net -> 1.8uA draw
200k -> 320mV at 5V net -> 1.6uA draw

### [3] led_en_gpio short
- With jumper "keep_led_on", ldo_en is shorted to led_en. ldo_en can get to 5V when usb is conencted. Since led_en is connected directly to GPIO, the ESP32 gets connected to 5V, which is not good.
- Mod: cut trace on back side from led_en gpio to keep_led_on jumper. led_en gpio is still accessible through rx pin
- Future solution: Make connection from led_en on gpio side and boost side cuttable. Also add soldering point to led_en_gpio so it can be reused (e.g. for an external mosfet)

### [2] power on too sensitive
- Power on circuit very sensitive to button input, very small currents can turn it on.
- Probably not a problem when the pcb is inside the ball
- Better use a mosfet in next PCB revision to have more control and use stronger pulls.

## panel v0 rev A

### [1] ~~Bad routing~~
*fixed in rev B*

- The USB protection IC routing is wrong. I/O pins are mixed up.
