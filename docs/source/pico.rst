Kookaberry Firmware on a Raspberry Pi Pico
==========================================

The standard `Raspberry Pi Pico <https://www.raspberrypi.com/documentation/microcontrollers/raspberry-pi-pico.html>`_ 
microcomputer may be used with **Kookaberry** firmware V1.10.0 onwards
to provide the software functionality that is available on a standard **Kookaberry** RP2040 microcomputer.

The Pico, however, does not have the physical OLED display, LEDs, buttons, JST-PH GPIO connectors, accelerometer / magnetometer, and digital packet radio
that the **Kookaberry** is equipped with.  

The **Kookaberry** display, buttons and LEDs are available to use in virtual form through the **KookaSuite** programming tools, 
using the *Show Display* options on **KookaBlockly** and **KookaIDE**, or through the **KookaTW** app.

See https://kookablockly-reference-guide.readthedocs.io/en/latest/index.html for instructions on using **KookaBlockly**.

Firmware Installation on the Pico
---------------------------------

.. note::

    **Kookaberry** firmware is compatible only with the Pico, and not with the other models (Pico-W, Pico-H, Pico-WH).

To install the firmware on the Pico:

1.	Download it from the GitHub repository: https://github.com/kookaberry/kooka-releases/releases
2.	Unzip the downloaded file and go to the rp2040 folder to reveal two files: kooka_rp2040.bin and kooka_rp2040_mboot.uf2.
3.	Hold down the BOOTSEL button on the Pico while connecting it to the USB port on the PC that contains the downloaded file.
4.  Load the bootloader (kooka_rp2040_mboot.uf2) onto the Pico by dragging/dropping the file onto the Pico's USB drive.  
5.  The Pico should then mount as a USB drive named "KOOKABERRY."  If not, unplug the Pico and reconnect to the USB port.
6.  Open the KOOKABERRY drive, drag/drop the kooka_rp2040.bin file to the root directory, and unlpug then reconnect the Pico.
7.  The green LED on the Pico will flash during the loading process, and two folders ("app" and "lib") will appear in the 
KOOKABERRY drive after loading is complete.

Raspberry Pico GPIO Pin Limitations
-----------------------------------

The Raspberry Pi Pico board has preset functions for four of the GPIO RP2040 pins, with 26 pins exposed on the Pico board out of 30 possibilities.

Key internal pin assignments on Raspberry Pi Pico include:

•	GPIO 23: Used for Power Control, affecting the onboard regulator.
•	GPIO 24: Monitors the state of VBUS.
•	GPIO 25: Connected to an onboard Green LED.
•	GPIO 29: Monitors VSYS internally and connects to ADC3.

The Kookaberry firmware also reserves some pins for internal functions, and these will not be available for general programming use:

•	GPIO 0, 1, 2, 3: Nordic nRF52 packet radio SPI Bus.
•	GPIO 21, 22: Nordic nRF52 packet radio SWCLK and SWDIO.
•	GPIO 18, 19: LSM303 Accelerometer/Magnetometer I2C SCL/SDA.
•	GPIO 2, 3, 7, 20: Blue OLED 128x64 Display (sh1106 controller) - SPI Bus.
•	GPIO 6: Software RESET.

Using the Kookaberry Pico
-------------------------

The Pico board fitted with pin headers may be used in conjunction with prototyping bread-boards or Pico break-out boards 
to facilitate wiring to peripherals and power.

The Pico boards may also be used in conjunction with purpose-built **Quokka** [#f1]_ Pico Break Out Modules which expose the available GPIO pins 
plus give access to the Kookaberry V2 assigned connectors P1 through P5 as JST-PH connectors. 

See :ref:`machine.Pin`_ for diagrams of available GPIO Pins and how to use them.

.. [#f1] *Quokka* is a trademark of Dinsdale and Associates.


