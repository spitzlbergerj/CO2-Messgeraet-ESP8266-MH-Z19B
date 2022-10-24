Dieses Repository dient als Sammlung für Dateien, Links und Anleitungen für den Bau eines CO2 Sensors für den Innenbereich auf Basis von ESP8266, ESPEasy, MH-Z19B, LEDs. Enthalten sind ebenfalls die stl-Dateien ein Gehäuse als 3D Druck.

This repository serves as a collection of files, links and documentation for building an indoor CO2 sensor based on ESP8266, ESPEasy, MH-Z19B, LEDs. Also included is a housing as 3D print.

# CO2 - Sensor für den Innenbereich

## Komponenten

- ESP8266 D1 mini
- MH-Z19B
- LED 3mm
- ESPEasy

## ESP8266 - genutzte Pins

Ich habe für meinen Aufbau folgnede Pins genutzt:

ESP Label | Funktion | Nutzung | Kabelfarbe (bei mir) | MH-Z19B | LEDs
----- | -------- | ------- | -------------------- | -------| ---
5V | Spannung 5V | Stromversorgung MH-Z19B | rot | Vin | -
G | Ground | Ground für MH-Z19B und alle LEDs | schwarz | GND | Kathoden
D5 | GPIO 14 | TX für MH-Z19 | gelb | TX | -
D7 | GPIO 13 | RX für MH-Z19 | grün | RX | -
D6 | GPIO 12 | LED rot | rot | - | Vorwiderstand - LED rot
D2 | GPIO 4 | LED gelb | gelb | - | Vorwiderstand - LED gelb
D1 | GPIO 5 | LED grün | grün | - | Vorwiderstand - LED grün

## Gehäuseaufbau

siehe https://www.thingiverse.com/thing:5580123

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-Gehäuse-geschlossen-LED.png"  width="200"> | <img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-Gehäuse-geschlossen.png"  width="200"> | <img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-Gehäuse-Unterteil.png"  width="200"> |  <img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-Gehäuse-Deckel.png"  width="200">

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-Gehäuse-real1.png"  width="200"> | <img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-Gehäuse-real2.png"  width="200"> | <img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-Gehäuse-real3.png"  width="200"> | 

## ESPEasy

[ESPEasy](https://www.letscontrolit.com/wiki/index.php/ESPEasy) bringt schon einen Flasher für Windows mit. Damit sollte das Flashen des ESP8266 bzw Wemos D1 mini mit einem USB Kabel am Windows Rechner kein Problem mehr sein. Allerdings kommt es wohl auf das USB Kabel an. Ich habe bestimmt 5 oder 6 Kabel durchprobiert, bis ich eines gefunden habe, mit dem es zuverlässig geht. Natürlich kann auch ein USB TTL Adapter verwendet werden.

Bei diesem Flasher kann insbesondere auch schon das WLAN eingetragen werden. Hier bitte den Hacken bei Post Flash Aufgaben nicht vergessen

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/Flasher-Konfiguration zum Flashen.png"  width="400">

### Hardware

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-ESPeasy-Hardware.png"  width="400">

### Device

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-ESPeasy-Devices.png"  width="400">

### Controllers

Ich sende die Werte an meine Homematic:

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-ESPeasy-Controllers.png"  width="400">

Controller Publish: egal.exe?ret=dom.GetObject(%27ESP8266-CO2-mobil%27).State(%val1%)

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-Homematic_Systemvariable.png"  width="400">

### Rules

<img src="https://github.com/spitzlbergerj/CO2-Messgeraet-ESP8266-MH-Z19B/blob/main/img/github-CO2-Sensor-ESPeasy-Rules.png"  width="400">

```
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
```

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

https://www.letscontrolit.com/wiki/index.php/ESPEasy
https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/
https://www.verdrahtet.info/2021/02/27/co2-sensor-fuer-homematic-und-iobroker-im-eigenbau/

