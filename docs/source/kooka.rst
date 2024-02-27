**********************************************************
:mod:`kooka` --- access to Kookaberry specific peripherals
**********************************************************

.. module:: kooka
   :synopsis: access to Kookaberry specific peripherals

This module provides access to the Kookaberry specific peripherals like the
display, buttons and accelerometer.  Objects for these internal peripherals
are created at start-up and available via the instance names given below.

Instances
=========

.. data::
    led_red
    led_orange
    led_green

    These objects give access to the three LEDs on the board.  They are
    instances of :ref:`kooka.LED`.

.. data::
    button_a
    button_b
    button_c
    button_d

    These objects give access to the four buttons on the board.  They are
    instances of :ref:`kooka.Button`.

.. data::
    display

    This gives acces to the display on the board, an instance of
    :ref:`sh1106.SH1106_SPI`.

.. data::
    accel

    This is the on-board accelerometer, an instance of :ref:`lsm303.LSM303C_Accel`.

.. data::
    compass

    This is the on-board magnetometer, an instance of :ref:`lsm303.LSM303C_Mag`.

.. data::
    radio

    This handles the interface to the 2.4GHz radio (nRF51822), an instance of
    :ref:`nrf51.Radio`.

Functions
=========

.. function:: neopixel_write(pin, timing_ns, buf)

    Write the bytes in *buf* to a NeoPixel-like device on the given *pin*.  Any pin
    can be used and interrupts will be disabled during the entire write to get accurate
    timing.  The *timing_ns* argument should be a 4-tuple of integers which specify
    the 0-high, 0-low, 1-high, 1-low timing values in nanoseconds to use when generating
    the output bit stream.  Typical timing values are ``(450, 800, 850, 400)`` and should
    be tuned for the specific LED hardware.  The bytes from *buf* are output big endian.

.. _kooka.LED:

class LED
=========

This class allows you to turn the built-in LEDs on and off.

Methods
-------

.. method:: on()

    Turn the LED on.

.. method:: off()

    Turn the LED off.

.. method:: toggle()

    Toggle the LED between on and off.

.. _kooka.Button:

class Button
============

This class allows you to read the state of one of the built-in buttons and
includes automatic debouncing.

Methods
-------

.. method:: value()

    Return the current state of the debounced button: 0 for released and 1 for
    pressed (debouncing has been performed on the return value).

.. method:: Pin.__call__()

    Button objects are callable, providing a fast shortcut to get the value of
    the button.  It is equivalent to ``Button.value()``.

.. method:: is_pressed()

    Returns ``True`` if the debounced button is held down, ``False`` otherwise.

    This method is mainly provided for compatibility with the micro:bit.

.. method:: was_pressed()

    This method gives access to the history of the button, allowing you to test
    if the button was pressed down since the last call to this method.  It
    returns ``True`` if it was pressed down, ``False`` otherwise.

    In detail: when the debouncing algorithm (which runs in the background)
    determines that the button went from being not held to held, the button
    remembers that it was pressed.  It will remember this until
    ``was_pressed()`` is called at which point it will return ``True`` and clear
    this state.  It will then return ``False`` if called again, until the button
    is pressed down again.

.. _kooka.Servo:

class Servo -- 3-wire hobby servo driver
========================================

This class allows you to control standard hobby servo motors with 3-wires (ground, power,
signal).  There are 4 available servo ports on P2, P3, P4 and P5.

Example usage::

    from kooka import Servo

    s1 = Servo('P2')   # create a servo object on connector P2
    s2 = Servo('P5')   # create a servo object on connector P5

    s1.angle(45)        # move servo 1 to 45 degrees
    s2.angle(0)         # move servo 2 to 0 degrees

    # move servo1 and servo2 synchronously, taking 1500ms
    s1.angle(-60, 1500)
    s2.angle(30, 1500)

Constructors
------------

.. class:: kooka.Servo(id)

   Create a servo object.  The parameter *id* can be a string naming the
   connector, like ``"P2"``, or a :ref:`machine.Pin` object representing the
   pin on the connector.

Methods
-------

.. method:: Servo.freq([freq])

   If no arguments are given, this function returns the current servo PWM
   frequency in Hz.

   If an argument is given then it sets the servo PWM frequency in Hz.

.. method:: Servo.angle([angle, time=0])

   If no arguments are given, this function returns the current angle of the servo.

   If arguments are given, this function sets the angle of the servo:

     - *angle* is the angle to move to in degrees.
     - *time* is the number of milliseconds to take to get to the specified
       angle.  If omitted, then the servo moves as quickly as possible to its
       new position.

.. method:: Servo.speed([speed, time=0])

   If no arguments are given, this function returns the current speed.

   If arguments are given, this function sets the speed of the servo:

     - *speed* is the speed to change to, between -100 and 100.
     - *time* is the number of milliseconds to take to get to the specified
       speed.  If omitted, then the servo accelerates as quickly as possible.

.. method:: Servo.pulse_width([value])

   If no arguments are given, this function returns the current raw pulse-width
   value in microseconds.

   If an argument is given, this function sets the raw pulse-width value in microseconds.

.. method:: Servo.calibration([pulse_min, pulse_max, pulse_centre, [pulse_angle_90, pulse_speed_100]])

   If no arguments are given, this function returns the current calibration
   data, as a 5-tuple.

   If arguments are given, this function sets the timing calibration:

     - *pulse_min* is the minimum allowed pulse width.
     - *pulse_max* is the maximum allowed pulse width.
     - *pulse_centre* is the pulse width corresponding to the centre/zero position.
     - *pulse_angle_90* is the pulse width corresponding to 90 degrees.
     - *pulse_speed_100* is the pulse width corresponding to a speed of 100.
