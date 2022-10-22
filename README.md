Dieses Repository dient als Sammlung von Dateien und Links für den Bau eines CO2 Sensors für den Innenbereich auf Basis von ESP8266, ESPEasy, MH-Z19B, LEDs. Enthalten ist ebenfalls ein Gehäuse als 3D Druck.
This repository serves as a collection of files and links for building an indoor CO2 sensor based on ESP8266, ESPEasy, MH-Z19B, LEDs. Also included is a housing as 3D print.

# CO2 - Sensor für den Innenbereich

## ESP8266 - genutzte Pins

ESP Label | Funktion | Nutzung | Kabelfarbe (bei mir) | MH-Z19B | LEDs
----- | -------- | ------- | -------------------- | -------| ---
5V | Spannung 5V | Stromversorgung MH-Z19B | rot | Vin | -
G | Ground | Ground für MH-Z19B und alle LEDs | schwarz | GND | Kathoden
D5 | GPIO 14 | TX für MH-Z19 | gelb | TX | -
D7 | GPIO 13 | RX für MH-Z19 | grün | RX | -
D3 | GPIO 0 | LED rot | rot | - | Vorwiderstand - LED rot
D4 | GPIO 2 | LED gelb | gelb | - | Vorwiderstand - LED gelb
D6 | GPIO 12 | LED grün | grün | - | Vorwiderstand - LED grün

## Gehäuseaufbau

## ESPeasy

### Device

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-ESPeasy-Devices.png"  width="400">

### Controllers

Ich sende die Werte an meine Homematic:

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-ESPeasy-Controllers.png"  width="400">

Controller Publish: egal.exe?ret=dom.GetObject(%27ESP8266-CO2-mobil%27).State(%val1%)

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-Homematic_Systemvariable.png"  width="400">

### Rules

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-ESPeasy-Rules.png"  width="400">

// Beim Booten

On System#Boot do   
   gpio,5,0          // LEDs aus
   gpio,4,0
   gpio,12,0
   gpio,5,1          // LEDs einzeln blinken
   gpio,5,0
   gpio,4,1
   gpio,4,0
   gpio,12,1
   gpio,12,0
   gpio,5,1          // LEDs ein
   gpio,4,1
   gpio,12,1
endon


// LEDs werteabhängig schalten
// D1, GPIO5 = grün
// D2, GPIO4 = gelb
// D6, GPIO12 = rot

On CO2_Sensor_mobil#PPM do
if [CO2_Sensor_mobil#PPM] <= 800
   gpio,5,1          
   gpio,4,0
   gpio,12,0
else
   if [CO2_Sensor_mobil#PPM] > 800 and [CO2_Sensor_mobil#PPM] <= 1400 
      gpio,5,0          
      gpio,4,1
      gpio,12,0
   else
      gpio,5,0          
      gpio,4,0
      gpio,12,1
   endif
endif
endon


# MH-Z19B

# ESP8266

## D1 mini - Layout

![ESP8266 D1 Mini](https://i0.wp.com/randomnerdtutorials.com/wp-content/uploads/2019/05/ESP8266-WeMos-D1-Mini-pinout-gpio-pin.png?w=715&quality=100&strip=all&ssl=1)

## GPIO Pins, wwelche wofür nutzen


Label | GPIO | Input | Output | Notes
----- | ----- | ----- | ----- | ----- 
D0 | GPIO16 | no interrupt | no PWM or I2C support | HIGH at boot; used to wake up from deep sleep
D1 | GPIO5 | OK | OK | often used as SCL (I2C)
D2 | GPIO4 | OK | OK | often used as SDA (I2C)
D3	| GPIO0	| pulled up	| OK	| connected to FLASH button, boot fails if pulled LOW
D4	| GPIO2	| pulled up	| OK	| HIGH at boot; connected to on-board LED, boot fails if pulled LOW
D5	| GPIO14	| OK	| OK	| SPI (SCLK)
D6	| GPIO12	| OK	| OK	| SPI (MISO)
D7	| GPIO13	| OK	| OK	| SPI (MOSI)
D8	| GPIO15	| pulled to GND	| OK	| SPI (CS); Boot fails if pulled HIGH
RX	| GPIO3	| OK	| RX pin	| HIGH at boot
TX	| GPIO1	| TX pin	| OK	| HIGH at boot; debug output at boot, boot fails if pulled LOW
A0	| ADC0	| Analog | Input	| X	


# Quellen

https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/
