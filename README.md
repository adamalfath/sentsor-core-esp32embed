# SENTSOR Core ESP32-EMBED
## Introduction
<img src="https://github.com/adamalfath/sentsor-core-esp32embed/blob/main/media/esp32embed-render2.png" width="600">  

Introducing a new SENTSOR Core Board ESP32-EMBED: all-in-one, super low power development board based on latest revision of ESP32 chip. As usual, SENTSOR Core Board comes with extremely accurate RTC and MicroSD slot, addressable RGB LED, with additional features like fully packed battery PMIC solution such as charger, protection and monitoring, also dual low Iq LDO design providing up to 800mA with overall sleep mode power consumption at only XuW, making this board suitable for low power embedded application.

This board is a revision from previous [SENTSOR Core Board WROOM-32U](https://github.com/adamalfath/sentsor-core-wroom32u) (R1.0) board. This revision also comes with two flavours, named ESP32-EMBED (this one) aimed for super low power and battery powered development board for embedded application, and [ESP32-DEV](https://github.com/adamalfath/sentsor-core-esp32dev) aimed for general purpose development board.

## Features
<img src="https://github.com/adamalfath/sentsor-core-esp32embed/blob/main/media/esp32embed-pinoutposter.png" width="600">  

- **Compact & powerful**, measured only 58.42x40.64mm (2300x1600mil) packing all the onboard features.
- **2x18P pinout layout**, standard pitch 2.54mm (100mil) breadboard friendly, with one side dedicated for peripheral (power, programming, communication) and the other side for GPIO, simplifying wire/trace management.
- **Castellated holes & pin header**, choose inter-board connection method according to your application requirement.
- **ESP32-WROOM-32UE**, comes with latest [ECO V3](https://www.espressif.com/sites/default/files/documentation/ESP32_ECO_V3_User_Guide__EN.pdfhttps://www.espressif.com/sites/default/files/documentation/ESP32_ECO_V3_User_Guide__EN.pdf) Dual Core 32-bit LX6 microprocessor with clock up to 240MHz, ULP co-processor, 520kB SRAM, 4MB flash.
- **802.11b/g/n WiFi Connectivity**, support mode STA/AP/STA+AP mode with external antenna using U.FL/IPEX connector allowing the board to be installed inside the enclosure without worrying transceiver signal quality. 
- **Bluetooth 4.2 Connectivity**, support classic bluetooth and Low Energy (LE) protocol.
- **DS3231M RTC**, extremely accurate ±5ppm RTC with alarm trigger capability and replaceable CR1220 battery. Connected via I2C at address 0x68.
- **Auto-reset Trigger**, ease boot mode changing during firmware upload process using DTR & RTS control line.
- **Battery Powered**, run SENTSOR board everywhere without power supply issue with 1-cell Lithium Ion/Polymer battery via JST-PH connector.
- **CN3165 Battery Charger**, solar-powered and USB-compatible Li-Ion/Polymer charger with on-chip adaptive charging current for wide range of input power scenario.
- **DW01A Battery Protection**, battery usage protection against over-charge, over-discharge, over-current, and short circuit.
- **LC709203F Battery Fuel Gauge**, accurately monitor your battery Relative State of Charge (RSOC).
- **RT9078 & RT9013 LDO**, dual low quiescent current LDO design providing configurable +3.3V power rail up to 800mA.
- **Super Low Power**, measured sleep mode power consumption down to XuW (XuA XV at VBAT pin, LDO2 off). 
- **Built-in LED/RGB**, single color active-low LED connected to pin IO2, and SK6812 addressable RGB LED connected to pin IO4. SK6812 data out is exposed so you can chain it up with the rest of the LED circuit.
- **MicroSD socket**, connected via SPI with slave select (SS) at pin IO5.
- **26 pin GPIO** @3V3 level, 3xUART, 2xSPI, 2xI2C, 2xI2S, 12bit ADC, 8bit DAC, hall-effect sensing, capacitive sensing, JTAG debug, etc. Please see pinout poster/schematic file for more information.

## How to Use
### Installing ESP32 Hardware Package
Chip from Espressif doesn't supported out-of-the-box by Arduino IDE so we need 3rd-party hardware package called ESP32 Arduino Core. The package installed using board manager, for detailed instruction on how to install ESP32 Arduino Core on Arduino IDE you can follow the step here
https://github.com/espressif/arduino-esp32/blob/master/docs/arduino-ide/boards_manager.md.

Once it's done, ESP32 should be listed on the board selection menu. Choose **ESP32 Dev Module** on board option, **4MB (32Mb)** on the flash size option and set the PSRAM option to disabled. For the partition scheme (application memory & file system) you can choose the allocation combination as needed, and the other option can left as default.  
<img src="https://github.com/adamalfath/sentsor-core-esp32embed/blob/main/media/esp32embed-arduinoideinfo.png" width="600">  
Now your SENTSOR Core ESP32-EMBED is ready to be use! Again, you can skip this step if you already install the ESP32 hardware package.

### Programming & Uploading Firmware
This board need an external programmer (USB to UART bridge) to upload the firmware, like FT232, CH340, CP2102, PL2303, etc. Connect the external programmer to the board according to wiring configuration below then do the upload process as usual.

|Programmer|SENTSOR board|
|-|-|
|+3V30 (or +5V)|+3V3 (or VIN)|
|GND|GND|
|TX|RX|
|RX|TX|
|DTR|DTR|
|RTS|RTS|

DTR & RTS connection is optional for auto-reset signal. You can left it unconnected and enter the boot mode manually by pressing BOOT then RESET button.

### Dependency
To access on-board peripheral like RTC, MicroSD & RGB LED you may need to install related library below:
- RTCLib https://github.com/adafruit/RTClib
- SdFat https://github.com/greiman/SdFat
- NeoPixel https://github.com/adafruit/Adafruit_NeoPixel  

or any other library of your choice that have same functionality.

### Battery Powered
SENTSOR Core Board ESP32-EMBED can be powered using 1-cell (3.7V-4.2V) Lithium-Ion/Polymer battery using JST-PH connector. The charging process can be done by supplying external power to VIN pin, charging current is set via ISET resistor (please see schematic file for more information).

> **:warning: Caution!**  
> Please pay attention to the battery connector polarity, there's +/- sign on the board silkscreen.

### Power Configuration
This board have 3 exposed jumper pad (labeled PWRCFG, see poster above). Each pad correspond with the configuration for board power system.

|VIN_CUT|LDO_IO|LDO_AON|Result|
|-|-|-|-|
|X|0|0|LDO_2 disabled|
|X|0|1|LDO_2 always on|
|0|1|0|LDO_2 enabled by GPIO (IO27)|
|1|1|0|LDO_2 active & VIN_0 cut|
|X|1|1|Invalid|

X = don't care. 1 mean the pad is connected, whether using 0Ω resistor or solder bridge. 0 mean the pad is left open (unconnected).

## Bill of Materials
|Designator|Qty|Name/Value|Footprint|
|-|-|-|-|
|U1|1|RT9078-33GJ5|TSOT-23-5_L2.9-W1.6-P0.95|
|U2|1|RT9013-33GB|TSOT-23-5_L2.9-W1.6-P0.95|
|U3|1|CN3165|DFN-8_L3.0-W3.0-P0.50-TL-EP|
|U4|1|DW01A|SOT-23-6_L2.9-W1.6-P0.95|
|U5|1|LC709203FQH-01TWG|WDFN-8_L4.0-W3.0-P0.50-BL-EP|
|U6|1|ESP32-WROOM-32UE(4MB)|WIFIM-32UE-SMD_38P|
|U7|1|DS3231MZ+|SOIC-8_L4.9-W3.9-P1.27|
|R1,R2|2|100k|R0402|
|R10,R11,R12,R13,R14, R15,R16,R17|8|10k|R0402|
|R19,R21|2|0 (NC)|R0402|
|R20|1|0|R0402|
|R3,R4,R5,R8,R9,R18|6|1k|R0402|
|R6|1|2.2k|R0402|
|R7|1|100|R0402|
|C1,C2,C4,C7|4|1u|C0402|
|C3|1|220u, 6.3V|CASE-B_3528|
|C5,C8|2|10u|C0402|
|C6,C9,C10,C11,C12,C13|6|100n|C0402|
|D1|1|B5819WS|SOD-323_L1.8-W1.3|
|LED1|1|Blue|LED0402|
|LED2|1|Green|LED0402|
|LED3|1|Red|LED0402|
|LED4|1|White|LED0402|
|LED5|1|EC20-SK6812|LED2020|
|Q1|1|HSSK8811|SOT-363_L2.0-W1.3-P0.65|
|Q2|1|BSS138PW,115|SOT-323-3_L2.2-W1.3-P1.30|
|Q3|1|SL8205S|SOT-23-6_L2.9-W1.6-P0.95|
|Q4|1|UMH3N|SC-70-6_L2.2-W1.3-P0.65|
|BTN1,BTN2|2|3.9x3x4.45|SW-SMD_L3.9-W3.0-P4.45|
|CN1|1|JST-PH_2P|CONN-SMD_2P-P2.00|
|CARD1|1|HYC80-TF08-375|HYC80-TF08-XXX|
|BAT1|1|CR1220|BAT_CR1220_SMD|


## Design 
<img src="https://github.com/adamalfath/sentsor-core-esp32embed/blob/main/media/esp32embed-pcb-ss.png" width="600">  

SENTSOR Core ESP32-EMBED is an open source hardware, please use it wisely. The project files is hosted on online based EDA called EasyEDA, you can access it via link below:  

>Link: https://easyeda.com/sentsor-project/sentsor-core-esp32-embed

## Gallery
_Render View:_  
<img src="https://github.com/adamalfath/sentsor-esp32embed/blob/main/media/esp32embed-render0.png" width="400"> <img src="https://github.com/adamalfath/sentsor-core-esp32embed/blob/main/media/esp32embed-render1.png" width="400">  
<img src="https://github.com/adamalfath/sentsor-core-esp32embed/blob/main/media/esp32embed-render2.png" width="400"> <img src="https://github.com/adamalfath/sentsor-core-esp32embed/blob/main/media/esp32embed-render3.png" width="400">

## Support Open Source Hardware & SENTSOR!
If you are interested in our product, you can buy them in the marketplace or by giving donation using this link below:

[![Store](https://img.shields.io/badge/marketplace-Tokopedia-brightgreen.svg)](https://www.tokopedia.com/gerai-sagalarupa/etalase/sentsor-product)  [![Store](https://img.shields.io/badge/marketplace-Tindie-lightgrey.svg)](https://www.tindie.com/)  [![Donate](https://img.shields.io/badge/donate-PayPal-blue.svg)](https://www.paypal.me/adamalfath)  

Your support will be very helpful for the next development of open source hardware from SENTSOR.

## Information
SENTSOR  
Author: Adam Alfath  
Contact: adam.alfath23@gmail.com  
Web: [sentsor.net](http://www.sentsor.net)  
Repo: [SENTSOR Main Repo](http://github.com/adamalfath/sentsor)

<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png"/></a><br/>
SENTSOR Core ESP32-EMBED is licensed under <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.