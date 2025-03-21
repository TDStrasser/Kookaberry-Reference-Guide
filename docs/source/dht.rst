**********************************************************
:mod:`dht` --- Atmospheric temperature and humidity sensor
**********************************************************
.. module:: dht
   :synopsis: Digital atmospheric multi-sensor


.. _dht:


The digital humidity and temperature (dht) module provides the interfaces to use the DHT11 (blue) and the DHT22 (white) temperature and humidity sensors. 

DHT sensors are low-cost digital sensors with capacitive humidity sensors and thermistors to measure the surrounding air. 
They feature a chip that handles analogue to digital conversion and communicates using a 1-wire interface.  
The interface is built into the DHT class and does not need to be created separately.

The DHT22 returns floating-point results with 1 decimal place resolution for both humidity and temperature readings. 

The DHT11 returns integer values for both temperature and humidity.

.. important:: 

    The DHT11 can be sampled no more than once per second and the DHT22 once every two seconds for most accurate results.


The data-sheets for the sensors are available here:

- DHT11: https://components101.com/sites/default/files/component_datasheet/DHT11-Temperature-Sensor.pdf 
- DHT22: https://components101.com/sites/default/files/component_datasheet/DHT22%20Sensor%20Datasheet.pdf 


Class DHT
=========

Example Usage::

    import dht
    from machine import Pin
    import time

    dht = dht.DHT11(Pin('P4')) # for the DHT11 (blue) sensors
    # or dht = dht.DHT22(Pin('P4')) # for the DHT22 (white) sensor

    # Then measure and read their values with:
    while True:  # loop runs forever
      dht.measure() # initiates the sensor reading
      dht.temperature() # returns the temperature in degrees Celsius
      dht.humidity() # returns the relative humidity as a percentage
      
      # Then add code to do something with the measurements, e.g.
      print(temperature, "C", humidity, "%")
      # Then sleep for 2 seconds to allow the DHT to take the next reading
      time.sleep(2)


DHT Constructors
----------------

.. class:: dht.DHT11(pin)

.. class:: dht.DHT22(pin)

    Creates the DHT11/22 sensor object.

    The parameter *pin* can be a string naming the connector, like ``"P2"``, or a :class:`machine.Pin` object representing the pin on the connector.    

DHT Methods
-----------

The generic term *dht* is used to represent either *DHT11* or *DHT22* as appropriate.

.. method:: dht.measure()

    Instructs the DHT11/22 sensor to initiate a measurement.
    
.. method:: dht.temperature()

    Returns the temperature reading, in degrees Celsius, from the DHT11/22 sensor.

.. method:: dht.humidity()

    Returns the relative humidity reading, in percent, from the DHT11/22 sensor.

