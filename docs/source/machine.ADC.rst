.. currentmodule:: machine
.. _machine.ADC:

class ADC -- analog to digital conversion
=========================================

Analogue to Digital Converters (ADC) measure the magnitude of an analogue signal and represent it as a digital number for use by a computer program.

See https://simple.wikipedia.org/wiki/Analog-to-digital_converter and `these online articles<https://www.sciencedirect.com/topics/engineering/analog-to-digital-converter#:~:text=An%20analog%2Dto%2Ddigital%20converter,to%20any%20type%20of%20microcontroller>`_.

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

.. method:: ADC.read_u16()

   Read the value on the analog pin and return it as a 16-bit integer.  
   The returned value will be between 0 and 65535.

.. method:: ADC.read_timed(buf, timer)

   .. note:: 

      Temporarily unavailable

   Read analog values into ``buf`` at a rate set by the ``timer`` object.

   ``buf`` can be bytearray or array.array for example.  The ADC values have
   12-bit resolution and are stored directly into ``buf`` if its element size is
   16 bits or greater.  If ``buf`` has only 8-bit elements (eg a bytearray) then
   the sample resolution will be reduced to 8 bits.

   ``timer`` should be a Timer object, and a sample is read each time the timer
   triggers.  The timer must already be initialised and running at the desired
   sampling frequency.

   To support previous behaviour of this function, ``timer`` can also be an
   integer which specifies the frequency (in Hz) to sample at.  In this case
   Timer(6) will be automatically configured to run at the given frequency.

   Example using a Timer object (preferred way)::

       adc = machine.ADC(machine.Pin('P1'))    # create an ADC on pin P1
       tim = machine.Timer(6, freq=10)         # create a timer running at 10Hz
       buf = bytearray(100)                # creat a buffer to store the samples
       adc.read_timed(buf, tim)            # sample 100 values, taking 10s

   Example using an integer for the frequency::

       adc = machine.ADC(machine.Pin('P1'))    # create an ADC on pin P1
       buf = bytearray(100)                # create a buffer of 100 bytes
       adc.read_timed(buf, 10)             # read analog values into buf at 10Hz
                                           #   this will take 10 seconds to finish
       for val in buf:                     # loop over all values
           print(val)                      # print the value out

   This function does not allocate any heap memory. It has blocking behaviour:
   it does not return to the calling program until the buffer is full.

.. method:: ADC.read_timed_multi((adcx, adcy, ...), (bufx, bufy, ...), timer)

      .. note:: 

      Temporarily unavailable


   
   This is a static method. It can be used to extract relative timing or
   phase data from multiple ADC's.

   It reads analog values from multiple ADC's into buffers at a rate set by
   the *timer* object. Each time the timer triggers a sample is rapidly
   read from each ADC in turn.

   ADC and buffer instances are passed in tuples with each ADC having an
   associated buffer. All buffers must be of the same type and length and
   the number of buffers must equal the number of ADC's.

   Buffers can be ``bytearray`` or ``array.array`` for example. The ADC values
   have 12-bit resolution and are stored directly into the buffer if its element
   size is 16 bits or greater.  If buffers have only 8-bit elements (eg a
   ``bytearray``) then the sample resolution will be reduced to 8 bits.

   *timer* must be a Timer object. The timer must already be initialised
   and running at the desired sampling frequency.

   Example reading 3 ADC's::

       adc0 = machine.ADC(machine.Pin('P1'))    # Create ADC's
       adc1 = machine.ADC(machine.Pin('P2'))
       adc2 = machine.ADC(machine.Pin('P4'))
       tim = machine.Timer(1, freq=100)        # Create timer
       rx0 = array.array('H', (0 for i in range(100))) # ADC buffers of
       rx1 = array.array('H', (0 for i in range(100))) # 100 16-bit words
       rx2 = array.array('H', (0 for i in range(100)))
       # read analog values into buffers at 100Hz (takes one second)
       machine.ADC.read_timed_multi((adc0, adc1, adc2), (rx0, rx1, rx2), tim)
       for n in range(len(rx0)):
           print(rx0[n], rx1[n], rx2[n])

   This function does not allocate any heap memory. It has blocking behaviour:
   it does not return to the calling program until the buffers are full.

   The function returns ``True`` if all samples were acquired with correct
   timing. At high sample rates the time taken to acquire a set of samples
   can exceed the timer period. In this case the function returns ``False``,
   indicating a loss of precision in the sample interval. In extreme cases
   samples may be missed.

   The maximum rate depends on factors including the data width and the
   number of ADC's being read. In testing two ADC's were sampled at a timer
   rate of 210kHz without overrun. Samples were missed at 215kHz.  For three
   ADC's the limit is around 140kHz, and for four it is around 110kHz.
   At high sample rates disabling interrupts for the duration can reduce the
   risk of sporadic data loss.
