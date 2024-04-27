**********************************************************
:mod:`kooka` --- access to Kookaberry specific peripherals
**********************************************************
.. _kooka:

.. module:: kooka
   :synopsis: access to Kookaberry specific peripherals

This module provides access to the **Kookaberry** specific peripherals like the
display, buttons and accelerometer.  Objects for these internal peripherals
are created at start-up and are available via the instance names given below.

Instances
=========

.. data::
    led_red
    led_orange
    led_green
    led_power

    These objects give access to the three LEDs on the the front of the **Kookaberry** and the green power LED on the rear.  They are
    instances of :ref:`kooka.LED`.

.. data::
    button_a
    button_b
    button_c
    button_d

    These objects give access to the four buttons (A, B, C, and D) on the **Kookaberry**.  They are
    instances of :ref:`kooka.Button`.

.. data::
    display

    This gives access to the **Kookaberry**'s display, an instance of
    :ref:`sh1106.SH1106_SPI` combined with :ref:`framebuf`.

    See :ref:`kooka.display` for a detailed description.

.. data::
    accel

    This is the on-board accelerometer, an instance of :ref:`lsm303.LSM303C_Accel`.

.. data::
    compass

    This is the on-board magnetometer, an instance of :ref:`lsm303.LSM303C_Mag`.

.. data::
    radio

    This handles the interface to the 2.4GHz digital packet radio (nRF51822), an instance of
    :ref:`nrf51.Radio`.


.. _kooka.LED:

class LED
=========

This class allows you to turn the built-in LEDs on and off.

This class exists only for the pre-defined instances (``led_red``, ``led_orange``, ``led_green``, and ``led_power``) 
and cannot be used on GPIO pins.
This is because the LEDs are connected to one of the **Kookaberry** co-processors and not the programmable microcomputer.

Example Usage::

    import kooka

    kooka.led_red.on() # Turn the red LED on

    kooka.led_orange.toggle()  # Change the state of the orange LED

    kooka.led_green.off()  # Turn the green LED off



LED Methods
-----------

.. method:: on()

    Turn the LED on.

.. method:: off()

    Turn the LED off.

.. method:: toggle()

    Toggle the LED between on and off.

.. _kooka.Button:

class Button
============

This class allows you to read the state of one of the built-in buttons and includes automatic debouncing.

This class exists only for the pre-defined instances (``button_a``, ``button_b``, ``button_c``, and ``button_d``) 
and cannot be used on GPIO pins.
This is because the buttons are connected to one of the **Kookaberry** co-processors and not the programmable microcomputer.

Example Usage::

    # Blinks the orange LED and controls the green LED
    import kooka, time

    while not kooka.buttan_a.was_pressed(): # Loop until button A was pressed
        kooka.led_orange.toggle() # Change the state of the orange LED

        if kooka.button_b.is_pressed(): # If button B is being pressed
            kooka.led_green.on()  # Turn on the green LED
        else:
            kooka.led_green.off() # Otherwise turn the green LED off

        time.sleep(1) # Delay for 1 second to slow blinking



Button Methods
--------------

.. method:: value()

    Return the current state of the debounced button: 0 for released and 1 for
    pressed (debouncing has been performed on the return value).

.. method:: is_pressed()

    Returns ``True`` if the debounced button is held down, ``False`` otherwise.

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

class Servo - 3-wire hobby servo driver
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

Servo Constructors
------------------

.. class:: kooka.Servo(id)

   Create a servo object.  The parameter *id* can be a string naming the
   connector, like ``"P2"``, or a :class:`machine.Pin` object representing the
   pin on the connector.

Servo Methods
-------------

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


.. _kooka.display:

class Display - access to the Kookaberry's OLED display and underlying framebuffer
==================================================================================

The display class inherits the methods of the :ref:`framebuf` class and the physical :ref:`sh1106` OLED display.

- The SH1106 OLED display is monochrome with dimensions 128 pixels wide x 64 pixels high.
- The top-left corner of the display is coordinate (0,0) and the bottom-right is coordinate (127,63)
- Being a monochromatic display, only two colours are supported being: ``0`` black (pixel off); and ``1`` white (pixel on)

Example Usage::

    from kooka import display       # Create the display object

    display.print('hello', 123)     # print objects to the display directly in the default 8x8 font

    import fonts                    # Import the text font sets
    disp.setfont(fonts.mono6x7)     # set the font to 6x7
    disp.print('Hello World!')      # print a message using the 6x7 font

    display.contrast(255)           # set maximum contrast
    display.invert(True)            # invert all pixels

    display.fill(0)                 # clear the display
    display.line(4, 5, 60, 40, 1)   # draw a line
    display.show()                  # show the frame buffer to the display

Display Constructor
-------------------

.. class:: kooka.display()

   Create a display object comprising a correctly dimensioned framebuffer and the SH1106_SPI physical OLED display object.  

Drawing Basic Shapes on the Display
-----------------------------------

The following methods draw basic shapes onto the display.

See also :ref:`shapeslib` for a helper module that draws more advanced geometric shapes 
by combining the following methods in easy to use ways.

.. method:: display.fill(c)

    Fill the entire FrameBuffer with the specified color, being: `0`` pixels are off; and ``1`` pixels are on.

.. method:: display.pixel(x, y[, c])

    If *c* is not given, get the color value of the specified pixel.
    If *c* is given, set the specified pixel to the given color, being: `0`` pixel is off; and ``1`` pixels is on.

.. method:: display.hline(x, y, w, c)
.. method:: display.vline(x, y, h, c)
.. method:: display.line(x1, y1, x2, y2, c)

    Draw a line from a set of coordinates using the given color and
    a thickness of 1 pixel. The `line` method draws the line up to
    a second set of coordinates whereas the `hline` and `vline`
    methods draw horizontal and vertical lines respectively up to
    a given length.

.. method:: display.rect(x, y, w, h, c)
.. method:: display.fill_rect(x, y, w, h, c)

    Draw a rectangle at the given location, size and color. The `rect`
    method draws only a 1 pixel outline whereas the `fill_rect` method
    draws both the outline and interior.

Drawing Text on the Display
---------------------------

.. method:: display.text(s, x, y[, c])

    Write text to the FrameBuffer using the the coordinates as the lower-left
    corner of the text. The color of the text can be defined by the optional
    argument but is otherwise a default value of 1. All characters have
    default dimensions of 8x8 pixels.

.. method:: display.setfont(font)

    Change the font to the one given by the object *font* which is an instance 
    from the :ref:`fonts` class.  Available fonts are:
    
    -  ``fonts.mono5x5``
    -  ``fonts.mono6x7``
    -  ``fonts.mono8x13``
    -  ``fonts.mono8x8`` and
    -  ``fonts.sans12``


Pixel Map Display Methods
-------------------------

.. method:: display.scroll(xstep, ystep)

    Shift the contents of the display by the given vector. This may
    leave a footprint of the previously lit pixels on the display.

.. method:: display.blit(fbuf, x, y[, key])

    Draw another FrameBuffer on top of the current one at the given display coordinates.
    If *key* is specified then it should be a color integer and the
    corresponding color will be considered transparent: all pixels with that
    color value will not be drawn.

    
Display Control Methods
-----------------------

These methods control the physical appearance of the **Kookaberry** display.

.. method:: display.init_display()

    Initialises and clears the display.

.. method:: display.poweroff()

    Turns off the display.

.. method:: display.poweron()

    Turns on the display.

.. method:: display.contrast(value)

    Sets the contrast of the display to the given *value*, between 0 and 255
    inclusive.

.. method:: display.invert(value)

    If *value* is true then the display colours are inverted, otherwise they
    are normal.  That is, black pixels change to white, and white pixels change to black.

.. method:: display.show()

    Draw the internal display framebuffer onto the physical display.

.. method:: display.print(\*args, sep= ' ')

    This method is intended to be equivalent to the built-in ``print()`` method,
    but it prints to the display instead of the serial prompt.

    The given arguments are converted to strings and then drawn to the display
    using the currently selected font (defaults to 8x8 font).  The display is scrolled up
    when text is drawn to the last line of the display.
   

.. toctree::
    :caption: Display-related classes
    :maxdepth: 1
    
    sh1106.rst
    framebuf.rst
    fonts.rst