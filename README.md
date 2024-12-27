# <p align="center">Klipper Expander Board</p>

![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img13.jpeg)

[1. What is this](#1-what-is-this)  
 
[2. A solution](#2-a-solution)

[3. Firmware Compilation](#3-firmware-compilation-)  

[4. Board in DFU mode](#4-board-in-mode)  

[5. Find and add serial number](#5-find-and-add-serial-number)  

[Klipper Expander PIN](#klipper/expander/pin) 

[FAQ](#faq) 

## Flashing and Configuration Guide

### If you are here it is because you have received a board to expand the connection ports of your VORON kit, don't worry, this file will guide you step by step on how to flash and correctly configure your board so that you can install the extra components you want correctly, Thanks for being here!

### Please read in the order presented on this page.

### 1. What is this?

Klipper Expander is a Simple MCU board for use with Klipper 3D Printer firmware. The Expander gives you additional connectivity that complements your main MCU(s) without the cost and size of a full board.

### 2. A solution

It is a solution to add some extras to our VORON printer when we no longer have space on our main board, this board is a perfect tool and a simple installation. It is a solution to add some extras to our VORON printer when we no longer have space on our main board, this board is a perfect tool and a simple installation.

# Setup Guide

### 3. Firmware Compilation

 - Login to your PI via SSH connection (This can be in [PuTTY](https://www.putty.org/) / [MobaXterm](https://mobaxterm.mobatek.net/) programs).

![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img1.png)

 - Enter the following commands to prepare the configuration
    ```bash
    cd klipper
	make menuconfig
	```
	
![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img2.png)

 - Configure the board firmware as follows, we will need to reduce space for the compilation, for this step go to this checkbox to uncheck some options
 
![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img3.png) 
![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img4.png)

- We will only leave these options enabled. Press `Q` on the keyboard to save and exit the settings 

 - Note that when switching between MCU architectures it is important to run `make clean` before `make`. This avoids strange compilation errors.
    ```bash 
    make clean
	make
    ```
![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img20.png)	
![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img5.png)
 
### Generate firmware that matches the current klipper version.

### If the progress bar reaches 100%, it means the burning is complete, please ignore the displayed error

![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img19.png)

### 4. Board in DFU mode

![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/STM32_Klipper_Expander.png)

### You need a jumper to short the BOOT0 pins, connect the usb cable and press the RESET button, the board should remain in DFU mode.

![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img12.jpeg)

### To perform this check, type the command `lsusb` and a device in DFU mode should appear.

    lsusb
![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img6.png)

 - Now flash the firmware with the following command make flash `FLASH_DEVICE=0483:df11`, the firmware was successfully flashed, ignore the error that appears at the end
    ```bash
     make flash FLASH_DEVICE=0483:df11
    ```

![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img7.png)

## Mainsail / Fluidd configuration

### Add this file [KlipperExpander.cfg](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Configure/klipperExpander.cfg) to your configuration by uploading a new file.

![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img8.png)

## Find and add serial number

It is important to know the serial number of our board to be able to include it in the file that we have just included in our configuration, with the command `ls /dev/serial/by-id/*` you can find the serial number of the klipperExpander boar. After completion `ls /dev/serial/by-id/*` should return a device begining with `/dev/serial/by-id/usb-Klipper_stm32f042x6...` 

    ls /dev/serial/by-id/*
![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img10.png)

### Now, copy and paste this serial into the file KlipperExpander in the line `serial`.

![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img9.png)

### Now include the created configuration to your main file `printer.cfg`.
   
    [include klipperExpander.cfg]
![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img11.png)

# <p align="center">`Klipper Expander PIN`</p>

![Image](https://github.com/ekostudio3d/Klipper-Expander/blob/main/Images/img14.jpg)

## Indicator LEDs

There are 3 status indicator LEDs on the board:
- 5v Status near the USB port indicates that there is 5V present and feeding the board (`D6`)
- Board Ready near the IC indicates that the board is communicating with Klipper (`D1`)
- Vin status indicates that there is power present on the Vin and GND feeding the board through the screw terminals (`D7`)
- 4 indicator LEDs (D2-D5) near each of the MOSFETs and screw terminals, these indicate if the MOSFET is enabled

## Mosfets (PA0, PA1, PA2, PA3)

4 x 4 3A Amp mosfets for controlling LED's, Heater, Fans, and other accessories
Connected to pins PA0, PA1, PA2 and PA3

## Thermistors (PA5, PA6)

2 thermistor inputs that use a 4.7K pullup resistor (Klipper default)
Connected to pins PA5 and PA6

# Neopixel header (PB1)

Header for using neopixels. There is a single power input pin (NPV) that you can supply with the voltage your strip needs (5V/12V) and it passes it to the three pin header (Vin, Data, Ground)
Connected to J1, which has a NPV supplied voltage, GND and PB1

## FAQ

### Join our Discord and WeChat groups to receive after-sales support.

 <br/><img src=https://github.com/Lzhikai/SIBOOR-Voron-2.4-AUG/blob/main/Images/DISCORD.jpg width="1080"/><br/> 
 
 ## Links
- SIBOOR Official Discord: [https://discord.gg/tDe55eYx)
- VORON Official Website: [https://vorondesign.com/](https://vorondesign.com/)
- Klipper Official: [https://www.klipper3d.org/](https://www.klipper3d.org/)
- BTT Official: [https://github.com/bigtreetech](https://github.com/bigtreetech)