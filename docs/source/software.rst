Kookaberry Software
===================

The **Kookaberry** is programmed using the **MicroPython** software language.  
This language is an implementation of the Python 3 language that has been adapted to be suitable for micro-computers.  

More information on **MicroPython** can be found at docs.micropython.org and in particular the **MicroPython** standard library at 
https://docs.micropython.org/en/latest/library/index.html#python-standard-libraries-and-micro-libraries 

A more specific reference to **MicroPython** for the **Kookaberry** can be found at http://docs.micropython.org/en/kookaberry/kookaberry/quickref.html  

MicroPython
-----------

**Micropython** source code is loaded as Python text files (``name.py``) onto the **Kookaberry’s** serial memory by a simple drag and drop operation 
when the **Kookaberry** is mounted as a USB drive on a computer.  

**Kookaberry** scripts should be in the "/app" folder on the **Kookaberry** drive.  

Libraries should be in the   "/lib" folder on the **Kookaberry** drive.  

Data and plain text files should be in the base (root) of the **Kookaberry** drive. 

Files created by **Kookaberry** scripts will also be stored in the base of the drive from where they can be retrieved.  

.. important:: 
    Please note that **Kookaberry**-generated files will not appear in the PC’s USB drive file directory until 
    after the **Kookaberry** has been dismounted and remounted as a USB drive. 
    The computer otherwise cannot detect the file changes that the **Kookaberry** makes.

The source code is converted to machine-readable bytecode by the **Kookaberry’s** on-board **MicroPython** compiler and will perform whatever instructions 
are contained in the source code.  This occurs when the program is run. 

Distinctively, the **Kookaberry** can also contain many **MicroPython** scripts (source code files) which can be selected and run by its on-board Menu.

Scripting Tools
---------------

For users who are more advanced and skilled in writing **MicroPython** scripts, the **KookaIDE** Interactive Development Environment (IDE) is provided 
which runs on Microsoft Windows, Apple’s MacOS, and on RaspberryPi Raspbian OS Personal Computers.  

**KookaIDE** provides an interactive interface with the **Kookaberry** via the REPL interface (Read-Evaluate-Print-Loop).  
This enables a live script coding and de-bugging interface for users.

The **Kookaberry** also has a second development environment intended for school students and coding novices called **KookaBlockly**, 
based on Google's Blockly drag and drop language. **KookaBlockly** automatically generates **MicroPython** code which is run in the usual manner.  

**KookaTW** is a utility that mirrors the **Kookaberry** display on a USB-attached PC or Macintosh.  
The **TW** stands for Twin, or alternatively Teacher's Window.

**KookaTW** is built into the KookaIDE and **KookaBlockly** IDEs.  

This facility allows the physically small Kookaberry display to be clearly observed and shared on a computer and makes it available 
to screen-grabbing software for documentation purposes.

**KookaIDE**, **KookaBlockly** and **KookaTW** are packaged as **KookaSuite** for Windows and Macintosh Personal Computers 
and it is available for download here: 
https://github.com/kookaberry/kooka-releases/tree/master/KookaSuite 

Kookaberry Specific Module Library
----------------------------------

The **Kookaberry**'s software includes modules for using functions that are unique to the **Kookaberry**. These are descibed below:

.. toctree:: 
    :caption: Kookaberry Specific Modules
    

Peripherals Module Library
--------------------------

The **Kookaberry** provides an ever-growing module library to enable on-board devices and a range of external accessories to be used by the software.  
Further modules can be added by users for peripherals and accessories of their choosing.  

There are many Internet forums and repositories that provide module software code and help on getting accessories to work with 
**MicroPython** micro-computers such as the **Kookaberry**. Notable among the repositories is GitHub.

The Kookaberry's built-in software modules are described in the following sections.

.. toctree:: 
    :caption: Built-in Peripheral Modules

    bme280.rst
    dht.rst
    ds18x20.rst
    ds3231.rst
    ina219.rst
    lsm303.rst
    mcp23008.rst
    mlx90614.rst
    neopixel.rst
    nrf5.rst
    onewire.rst
    pcf8574.rst
    sh1106.rst
    uart.rst
    veml7700.rst

