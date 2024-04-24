*********************************************************
:mod:`machine` --- access to the Kookaberry microcomputer
*********************************************************
.. _machine:

.. module:: machine
   :synopsis: access to Kookaberry specific microcomputer

This module provides access to the **Kookaberry** microcomputer facilities like the
A/D and D/A Converters, serial interfaces, Real Time Clock, and timers.

The focus in this guide is on the core **Kookaberry** functionality to ensure that scripts are portable between **Kookaberry** models.
There are two variants of the **Kookaberry** using STM32 and RP2040 microcomputers.  
These each have features and functions particular to their microcomputers that are beyond those covered in this guide.

The additional functionality can be examined by using these commands in the **KookaIDE** REPL::

   import machine
   # Loads the machine module

   dir(machine)
   # Lists the classes available within the machine module

   dir(machine.Pin)
   # Lists the methods and data items in the machine.Pin class
   help(machine.Pin)
   # A more expansive listing of what is in the machine.Pin class

   # Do the same for other machine classes

   # Also search the MicroPython documentation for further explanations


.. toctree::
  :caption: Kookaberry MicroComputer Facilities

  machine.ADC.rst
  machine.DAC.rst
  machine.I2C.rst
  machine.Pin.rst
  machine.RTC.rst
  machine.SPI.rst
  machine.Timer.rst
  machine.UART.rst
  

