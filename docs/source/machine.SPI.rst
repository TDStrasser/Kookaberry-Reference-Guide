.. currentmodule:: machine
.. _machine.SPI:
.. _machine.SoftSPI:

class SoftSPI -- a Serial Peripheral Interface bus protocol (master side)
=========================================================================

.. note::

   The SPI class which gave access to hardware SPI ports has been deprecated for general use on the **Kookaberry**.
   It is still available as it is used for the **Kookaberry**'s internal peripherals, specifically the display and the front-end and radio co-processors.
   
   For general programming, it is now necessary to use the SoftSPI class as described herein.

   Software SPI is more flexible and less hardware dependant.  
   It is implemented by bit-banging and can be used on any pin but is not as efficient as hardware SPI was.

   The only difference in usage between the SPI and SoftSPI classes is in construction. 
   Otherwise their usage and methods are identical.
   
The Serial Peripheral Bus (SPI) is a synchronous serial protocol that is driven by a master. At the
physical level, a bus consists of 3 lines: SCK, MOSI, MISO. Multiple devices can share the same bus. 

Each device on the SPI bus should have a separate, 4th signal,
SS (Slave Select), to select a particular device on a bus with which
communication takes place. Management of an SS signal should happen in
user code (via :class:`machine.Pin`).

For a more fulsome description see https://en.wikipedia.org/wiki/Serial_Peripheral_Interface

SoftSPI Constructors
--------------------

.. class:: SoftSPI(baudrate=500000, *, polarity=0, phase=0, bits=8, firstbit=MSB, sck=None, mosi=None, miso=None)

   Construct a new software SPI object. Additional parameters must be given, 
   usually at least sck, mosi and miso, as these are used to initialise the bus. 
   See :method:`SoftSPI.init`` for a description of the parameters.


SoftSPI Methods
---------------

.. method:: SoftSPI.init(baudrate=500000, \*, polarity=0, phase=0, bits=8, firstbit=SPI.MSB, sck=None, mosi=None, miso=None)

   Initialise the SPI bus with the given parameters:

     - ``baudrate`` is the SCK clock rate.
     - ``polarity`` can be ``0`` or ``1``, and is the level the idle clock line sits at.
     - ``phase`` can be ``0`` or ``1`` to sample data on the first or second clock edge respectively.
     - ``bits`` is the width in bits of each transfer. Only 8 is guaranteed to be supported by all hardware.
     - ``firstbit`` can be ``SPI.MSB`` or ``SPI.LSB``.
     - ``sck``, ``mosi``, ``miso`` are pins (:class:`machine.Pin``) objects to use for bus signals.


.. method:: SoftSPI.deinit()

   Turn off the SPI bus.

.. method:: SoftSPI.read(nbytes, write=0x00)

    Read a number of bytes specified by ``nbytes`` while continuously writing
    the single byte given by ``write``.
    Returns a ``bytes`` object with the data that was read.

.. method:: SoftSPI.readinto(buf, write=0x00)

    Read into the buffer specified by ``buf`` while continuously writing the
    single byte given by ``write``.
    Returns ``None``.

.. method:: SoftSPI.write(buf)

    Write the bytes contained in ``buf``.
    Returns ``None``.


.. method:: SoftSPI.write_readinto(write_buf, read_buf)

    Write the bytes from ``write_buf`` while reading into ``read_buf``.  The
    buffers can be the same or different, but both buffers must have the
    same length.
    Returns ``None``.

SoftSPI Constants
-----------------

.. data:: SoftSPI.MSB

   set the first bit to be the most significant bit

.. data:: SoftSPI.LSB

   set the first bit to be the least significant bit
