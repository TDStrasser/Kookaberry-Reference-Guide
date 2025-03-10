.. currentmodule:: machine
.. _machine.ADC:

class ADC -- analog to digital conversion
=========================================

Analogue to Digital Converters (ADC) measure the magnitude of an analogue signal and represent it as a digital number for use by a computer program.

See https://simple.wikipedia.org/wiki/Analog-to-digital_converter and `these online articles <https://www.sciencedirect.com/topics/engineering/analog-to-digital-converter#:~:text=An%20analog%2Dto%2Ddigital%20converter,to%20any%20type%20of%20microcontroller>`_.

Usage::

    import machine

    pin = "P1"                          # specify the pin to use

    adc = machine.ADC(pin)              # create an analog object from a pin
    val = adc.read()                    # read an analog value


ADC Constructors
----------------

.. class:: machine.ADC(pin)

   Create an ADC object associated with the given pin.
   This allows you to then read analog values on that pin.

   *pin* should be a :class:`machine.Pin` object.


ADC Methods
-----------

.. method:: ADC.read()

   Read the value on the analog pin and return it as a 12-bit integer.  
   The returned value will be between 0 and 4095.

   .. note::

      This method is not available on the RP2040 or RP2350 **Kookaberry**

.. method:: ADC.read_u16()

   Read the value on the analog pin and return it as a 16-bit integer.  
   The returned value will be between 0 and 65535.

