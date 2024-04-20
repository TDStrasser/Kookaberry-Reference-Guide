*******************************************************
:mod:`onewire` --- interface to the nRF51 BLE coprocessor
*******************************************************

.. module:: onewire
   :synopsis: interface to the nRF51 BLE coprocessor

.. currentmodule:: onewire
.. _onewire:

The 1-wire bus is a serial bus that uses just a single wire for communication (in addition to wires for ground and power). 

This module provides low level access to the 1-wire serial interface.  Other modules dht, ds18x20 and neopixel make use of this module.

Read about the 1-wire communications protocol at https://en.wikipedia.org/wiki/1-Wire

Class OneWire
=============



Example Usage::

    # For the following code to work you need to have at least one DS18B20 temperature sensor with its data line connected to Pin P1. 
    import onewire, time, ds18x20
    from machine import Pin
    import onewire, ds18x20

    # create the onewire object
    ds = ds18x20.DS18X20(onewire.OneWire(Pin('P1')))

    # scan for devices on the bus
    roms = ds.scan()
    print('found devices:', roms)

    # loop 10 times and print all temperatures
    for i in range(10):
        print('temperatures:', end=' ')
        ds.convert_temp()
        time.sleep_ms(750)
        for rom in roms:
            print(ds.read_temp(rom), end=' ')
        print()


OneWire Constructor
-------------------

.. class:: onewire.OneWire(pin)

    *pin* is a Pin object using one of the Kookaberry's input pins (e.g. ``P1``, ``P2``, ``P3A``, ``P3B``, ``P4``, ``P5``)
    

OneWire Methods
---------------

.. method:: OneWire.reset(required=False)

    Resets the 1-wire bus to its initial conditions and returns the boolean success status. 
    
    Returns true if a device presence pulse was detected, otherwise false.

    *required* defaults to ``False``, but if set to ``True`` will raise a ``OneWireError`` if the reset is unsuccessful.


.. method:: OneWire.readbit()

    Reads and returns a single bit from 1-wire bus, assuming the 1-wire protocol.

.. method:: OneWire.readbyte()

    Reads 8 bits from the 1-wire bus using the 1-wire protocol, then returns them as an integer. (Assumes a low-bit-first model.)

.. method:: OneWire.readinto(buffer)

    Reads bytes from the 1-wire bus using the 1-wire protocol, into a predefined buffer, until the buffer is filled.

    *buffer* is a byte array of program-defined length.

.. method:: OneWire.writebit(bitvalue)

    Writes a single bit with value *bitvalue* to the 1-wire bus, using the 1-wire protocol.

.. method:: OneWire.writebyte(bytevalue)

    From byte *bytevalue*, writes 8 bits to the 1-wire bus using the 1-wire protocol. (Assumes a low-bit-first model.)

.. method:: OneWire.write(bytearray)

    Writes the contents of the *bytearray* to the 1-wire bus using the 1-wire protocol.

.. method:: OneWire.select_rom(romcode)

    Selects a specific 1-wire device by specifying its unique *romcode*.

.. method:: OneWire.scan()

    Scans the 1-wire bus and returns an array of unique *romcode*s corresponding to the devices on the 1-wire bus.

.. method:: OneWire.crc8(bytearray)

    Computes the 8-bit CRC-remainder of the given bytearray (or other buffered object) using the CRC-8/MAXIM version.


    

