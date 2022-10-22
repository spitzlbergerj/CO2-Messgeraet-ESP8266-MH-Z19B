Dieses Repository dient als Sammlung von Dateien und Links für den Bau eines CO2 Sensors für den Innenbereich auf Basis von ESP8266, ESPEasy, MH-Z19B, LEDs. Enthalten ist ebenfalls ein Gehäuse als 3D Druck.
This repository serves as a collection of files and links for building an indoor CO2 sensor based on ESP8266, ESPEasy, MH-Z19B, LEDs. Also included is a housing as 3D print.

# ESP8266

## D1 mini - Layout

https://i0.wp.com/randomnerdtutorials.com/wp-content/uploads/2019/05/ESP8266-WeMos-D1-Mini-pinout-gpio-pin.png?w=715&quality=100&strip=all&ssl=1

## GPIO Pins, wwelche wofür nutzen


Label | GPIO | Input | Output | Notes
----- | ----- | ----- | ----- | ----- 
D0 | GPIO16 | no interrupt | no PWM or I2C support | HIGH at boot; used to wake up from deep sleep
D1 | GPIO5 | OK | OK | often used as SCL (I2C)
D2 | GPIO4 | OK | OK | often | usedas SDA (I2C)
D3	| GPIO0	| pulled up	| OK	| connected to FLASH button, boot fails if pulled LOW
D4	| GPIO2	| pulled up	| OK	| HIGH at boot; connected to on-board LED, boot fails if pulled LOW
D5	| GPIO14	| OK	| OK	| SPI (SCLK)
D6	| GPIO12	| OK	| OK	| SPI (MISO)
D7	| GPIO13	| OK	| OK	| SPI (MOSI)
D8	| GPIO15	| pulled to GND	| OK	| SPI (CS); Boot fails if pulled HIGH
RX	| GPIO3	| OK	| RX pin	| HIGH at boot
TX	| GPIO1	| TX pin	| OK	| HIGH at boot; debug output at boot, boot fails if pulled LOW
A0	| ADC0	| Analog | Input	| X	


Quellen: 
https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/
