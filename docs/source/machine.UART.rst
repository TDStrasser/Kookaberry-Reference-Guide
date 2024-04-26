.. currentmodule:: machine
.. _machine.UART:

class UART -- duplex serial communication bus
=============================================

.. warning::

   This section is not yet finalised.

A universal asynchronous receiver-transmitter (UART) is an interface for asynchronous serial communication 
in which the data format and transmission speeds are configurable. 
It sends data bits one by one, from the least significant to the most significant, 
framed by start and stop bits so that precise timing is handled by the communication channel. 

See also https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter

UART implements the standard UART/USART duplex serial communications protocol.  At
the physical level it consists of 2 lines: Rx and Tx.  The unit of communication
is a character (not to be confused with a string character) which can be 8 or 9
bits wide.

UART objects can be created and initialised using::

    from machine import UART

    uart = UART("A", 9600)                       # init on connector P3 with given baudrate
    uart.init(9600, bits=8, parity=None, stop=1) # init with given parameters

On the **Kookaberry** Bits can be 7, 8 or 9. Stop can be 1 or 2. With *parity=None*,
only 8 and 9 bits are supported.  With parity enabled, only 7 and 8 bits
are supported.

A UART object acts like a `stream` object and reading and writing is done
using the standard stream methods::

    uart.read(10)       # read 10 characters, returns a bytes object
    uart.read()         # read all available characters
    uart.readline()     # read a line
    uart.readinto(buf)  # read and store into the given buffer
    uart.write('abc\n')   # write the 3 characters and a newline character

UART Constructors
-----------------

.. class:: UART(id, baudrate=9600, bits=8, parity=None, stop=1, \*, ...)

   Construct a UART object of the given *id*.

   *id* is a string which can be:
   
   - "A" (on connector P3) with the Rx pin on P3A / PA10 / J4, and the Tx pin on P3B / PA9 / J10
   - "B" (only on the edge connector) with the Rx pin on PC1 / K7, and the Tx pin on PC0 / K8


UART Methods
------------

.. method:: UART.init(baudrate=9600, bits=8, parity=None, stop=1, \*, ...)

   Initialise the UART bus with the given parameters:

     - *baudrate* is the clock rate (default is ``9600``).
     - *bits* is the number of bits per character, ``7``, ``8`` (default) or ``9``.
     - *parity* is the parity, ``None`` (default), ``0`` (even) or ``1`` (odd).
     - *stop* is the number of stop bits, ``1`` (default) or ``2``.

   Additional keyword-only parameters that may be supported by a port are:

     - *txbuf* specifies the length in characters of the TX buffer.
     - *rxbuf* specifies the length in characters of the RX buffer.
     - *timeout* specifies the time to wait for the first character (in ms).
     - *timeout_char* specifies the time to wait between characters (in ms).
     - *invert* specifies which lines to invert.

   
.. method:: UART.deinit()

   Turn off the UART bus.

.. method:: UART.any()

   Returns an integer counting the number of characters that can be read without
   blocking.  It will return ``0`` if there are no characters available and a positive
   number if there are characters.  The method may return ``1`` even if there is more
   than one character available for reading.

.. method:: UART.read([nbytes])

   Read characters.  If ``nbytes`` is specified then read at most that many bytes,
   otherwise read as much data as possible. It may return sooner if a timeout
   is reached. The timeout is configurable in the constructor.

   Return value: a bytes object containing the bytes read in.  Returns ``None``
   on timeout.

.. method:: UART.readinto(buf[, nbytes])

   Read bytes into the ``buf``.  If ``nbytes`` is specified then read at most
   that many bytes.  Otherwise, read at most ``len(buf)`` bytes. It may return sooner if a timeout
   is reached. The timeout is configurable in the constructor.

   Return value: number of bytes read and stored into ``buf`` or ``None`` on
   timeout.

.. method:: UART.readline()

   Read a line, ending in a newline character. It may return sooner if a timeout
   is reached. The timeout is configurable in the constructor.

   Return value: the line read or ``None`` on timeout.

.. method:: UART.write(buf)

   Write the buffer of bytes to the bus.

   Return value: number of bytes written or ``None`` on timeout.

.. method:: UART.sendbreak()

   Send a break condition on the bus. This drives the bus low for a duration
   longer than required for a normal transmission of a character.

.. method:: UART.irq(trigger, priority=1, handler=None, wake=machine.IDLE)

   Create a callback to be triggered when data is received on the UART.

       - *trigger* can only be ``UART.RX_ANY``
       - *priority* level of the interrupt. Can take values in the range 1-7.
         Higher values represent higher priorities.
       - *handler* an optional function to be called when new characters arrive.
       - *wake* can only be ``machine.IDLE``.

   .. note::

      The handler will be called whenever any of the following two conditions are met:

          - 8 new characters have been received.
          - At least 1 new character is waiting in the Rx buffer and the Rx line has been
            silent for the duration of 1 complete frame.

      This means that when the handler function is called there will be between 1 to 8
      characters waiting.

   Returns an irq object.


UART Constants
--------------

.. data:: UART.RTS -- 256

.. data:: UART.CTS -- 512

.. data:: UART.IRQ_RXIDLE -- 16
