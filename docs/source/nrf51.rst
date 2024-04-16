*******************************************************
:mod:`nrf51` --- interface to the nRF51 BLE coprocessor
*******************************************************
.. _nrf51:

.. module:: nrf51
   :synopsis: interface to the nRF51 BLE coprocessor

This module provides a ``Radio`` class to interface with the nRF51 BLE coprocessor.
Note that an instance of this class is created at start-up and available in the
:mod:`kooka` module as ``kooka.radio``.

Example usage::

    from kooka import radio

    radio.enable()
    radio.config(power=7)
    radio.send('message')
    radio.send_bytes(b'data')
    print(radio.receive())
    radio.disable()

.. _nrf51.Radio:

class Radio
===========

This class represents a connection to the nRF51 coprocessor.

Methods
-------

.. method:: Radio.enable()

    Enable the radio.  Raises a :class:`RadioError` if it fails.

.. method:: Radio.disable()

    Disable the radio.  Raises a :class:`RadioError` if it fails.

.. method:: Radio.config(param=value)

    Configure various settings relating to the radio. The parameters are:

    - ``length`` (default=32): defines the maximum length, in bytes, of a
      message sent via the radio. It can be between 1 and 251 bytes long.

    - ``queue`` (default=3): specifies the number of messages that can be
      stored on the incoming message queue.  If there is no space left on the
      queue then additional incoming messages are dropped.
      Can be between 1 and 254.

    - ``channel`` (default=7): an integer value between 0 and 83 inclusive
      that defines the channel (actually frequency) to which the
      radio is tuned. Messages will be sent via this channel and only messages
      received via this channel will be put onto the incoming message queue.
      Each step is 1MHz wide, starting at 2400MHz.

    - ``power`` (default=6): an integer value between 0 and 7 inclusive which
      indicates the strength of signal used when sending a message. The
      higher the value the stronger the signal, but the more power is consumed
      by the device. The numbering translates to positions in the following list
      of dBm (decibel milliwatt) values: -30, -20, -16, -12, -8, -4, 0, 4.

    - ``address`` (default=0x75626974): an arbitrary name, expressed as a
      32-bit address, that's used to filter incoming packets at the hardware
      level, keeping only those that match the address you set.
      The default matches that used on the micro:bit.

    - ``group`` (default=0): an 8-bit value (0-255) used in conjunction with
      ``address`` to filter incoming messages.  This effectively makes the full
      address 40 bits long.

    - ``data_rate`` (default=1): indicates the speed at which data transfer
      (send and receive) takes place.  It can be 0, 1 or 2, for 250kbit/sec,
      1Mbit/sec, or 2Mbit/sec respectively.

    - ``timestamp`` (default=TIMESTAMP_MS): indicates the units used in the
      timestamp entry returned by ``receive_full()``.  Possible values are
      ``radio.TIMESTAMP_MS`` and ``radio.TIMESTAMP_US``.

.. py:method:: send(message)

    Send a string message.  The parameter *message* should be a string object.
    This method is equivalent to ``send_bytes(bytes(message, 'utf8'))`` but with
    ``b'\x01\x00\x01'`` prepended to the front, which makes it compatible with
    code running on a micro:bit.

.. py:method:: send_bytes(message)

    Send a raw message.  The parameter *message* should be a bytes object.

.. py:method:: receive()

    Retrieve and return the next incoming message on the message queue.
    Returns ``None`` if there are no pending messages.
    Messages are returned as string objects.

    This method is equivalent to ``str(receive_bytes(), 'utf8')`` but with a
    check that the the first three bytes are ``b'\x01\x00\x01'``, which makes it
    compatible with code running on a micro:bit.  The method strips the prepended
    bytes before converting to a string (and raises a ``ValueError`` if the
    prefix is not correct).

.. py:method:: receive_bytes()

    Retrieve and return the next incoming message on the message queue.
    Returns ``None`` if there are no pending messages.
    Messages are returned as bytes objects.

.. py:method:: receive_full()

    Retrieve and return the next incoming message on the message queue, with
    additional information.
    Returns ``None`` if there are no pending messages.

    If there is a pending message the return value is a 3-tuple with elements:

    * the message as a bytes object.
    * the RSSI (signal strength) between -255 (weakest) and 0 (strongest) as measured in dBm.
    * a timestamp, either milliseconds or microseconds, being the value returned by
      ``time.ticks_ms()``or ``time.ticks_us()`` when the message was received.

    For example::

        details = radio.receive_full()
        if details:
            msg, rssi, timestamp = details

class RadioError
================

This class is derived from ``Exception`` and is used to indicate various low-level
errors with the radio hardware.
