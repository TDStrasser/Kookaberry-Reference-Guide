Peripherals Module Library
--------------------------

The **Kookaberry** provides an ever-growing module library to enable on-board devices and a range of external accessories to be used by the software.  
Further modules can be added by users for peripherals and accessories of their choosing.  

There are many Internet forums and repositories that provide module software code and help on getting accessories to work with 
**MicroPython** micro-computers such as the **Kookaberry**. Notable among the repositories is GitHub.

The Kookaberry's built-in software modules are described in the following sections. Supported peripherals include:

- buttons, other digital switches, and electrically compatible digital signals 
- analogue signals from potentiometers, and from analogue sensors for distance, light, sound, proximity, heart-rate etc.
- RGB colour LED arrays known as NeoPixels 
- servos and electrical motors
- digital sensors for acceleration, magnetism, temperature, humidity, air pressure, light, infra-red, electric voltage and current
- battery time sources and clockwise
- digital serial communications with radio transceivers and other computers

.. important::

    All peripherals used with the **Kookaberry** must be electrically compatible with it.
    
    The allowable Pin input voltage range for the **Kookaberry** is 0 volts to +3.3 volts DC. 
    Applying voltages outside that range may irreparably damage the **Kookaberry**.


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
    nrf51.rst
    onewire.rst
    veml7700.rst

