# Midea HVAC WiFi Dongle

This repository contains the hardware design of a WiFi dongle for Midea-like airconditioners and humidifiers.
Midea provides a WiFi dongle (SK102/SK103), either as optional feature or built-in, to control the appliances via a mobile application. This dongle uses and is dependent on the Midea cloud which does not expose a publically available API. The protocol used by the dongle has been reverse engineered, but newer version still require the cloud to obtain an encryption key or require an addition server simulating the Midea cloud. This means integration in an existing home automation system is not straight forward.

Besides the protocol the dongle uses to commumicate with the mobile app and the cloud, also the protocol that is used between the dongle and the appliances has been reverse engineered (at least the most important parts). This means the original dongle can be replaced by a custom dongle making it independent on any cloud.

Besides Midea itself other vendors of these type of appliances are also using the Midea cloud and dongle to support control via a mobile app. Examples of these vendors are:

* Qlima
* Artel
* Carrier
* Toshiba
* Idema

This design of a custom dongle can be used to replace the orignal dongle with a type A USB connector. The version with with a JST-XH connector needs a slight modification.

## Technical design

The design of the dongle is based on an ESP-12 which provides the WiFi interface and microcontroller to communicate with the appliance. Because the interface of the appliance is a 5V TTL serial port (UART) and the ESP-12 uses 3.3V and is not 5V tolerant, we need level shifters for the communication and a power regulator in order to power the dongle from the appliance.

The design has been made in KiCad and all files can be found in the KiCad directory.

<p align="center">
  <img src="./images/pcb.jpeg" /><br/>
  <em>The KiCad PCB design</em>
</p>

Note: The resulting PCB including the ESP-12 turned out to be 1 mm to long to fit into the location of the original dongle (in my Artel airconditioner). It was easy to locate it somewhere else, but pay attention whether the PCB would fit in your appliance. The dimensions of the dongle are: 55mmx20mm (l*w) without casing.

## Ordering and manufacturing

The PCB can be ordered by the manufacturer of your choosing. I had it manufactured and partly assembled by [JLCPCB](https://jlcpcb.com/). The files required to order the PCB including the SMT assembly of all components except the ESP-12 and USB connector can be found in the JLCPCB directory.

<p align="center">
  <img src="./images/top_assembled.jpeg" /><br/>
  <em>The assembled PCB</em>
</p>

The JLCPCB directory contains the following files:

* Gerber-Midea WiFi Dongle v1.zip, this zip contains all gerber files and the drill file.
* BOM-Midea WiFi Dongle.csv, this is the Bill Of Material file.
* CPL-Midea WiFi Dongle.csv, this is the Pick and Place file

Note: The ESP-12 and USB connector are not part of the BOM and need to be ordered somewhere else, JLCPCB could not deliver them. This also means you have to solder them yourself.

## Case

I also designed a case that can printed on a 3d printer. The .STL file is part of this repository.

<p align="center">
  <img src="./images/case.jpeg" /><br/>
  <em>The 3D printed case</em>
</p>

## Firmware

At this moment there are several options
* Sören Beye created firmware to integrate dehumidifiers ith Home Assistant: https://github.com/Hypfer/esp8266-midea-dehumidifier
* Sergey V. Dudanov create an extension for ESPHome integrating aircondititioners with Home Assistant: https://github.com/dudanov/esphome
* I created a node.js module to integrate airconditioners with aything you like, but you need to program: https://github.com/reneklootwijk/node-mideahvac. The firmware required on the dongle is a esplink, a serial bridge: https://github.com/dudanov/esphome
* To reverse engineer the protocol the original dongle is using to communicate with your appliance, you can use this custom dongle to capture this communication. See https://github.com/reneklootwijk/midea-uartsniffer.