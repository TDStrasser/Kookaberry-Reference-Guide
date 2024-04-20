*****************************************************************
:mod:`fonts` --- Pixel arrays of fonts for the Kookaberry display
*****************************************************************
.. _fonts:

.. module:: fonts
   :synopsis: Pixel arrays of alphanumeric text fonts for use with the Kookaberry's display

Example Usage::

    import fonts
    from kooka import display as disp # Create the Kookaberry display object

    disp.fill(0) # clear the display
    disp.setfont(fonts.mono8x8) # set the font
    disp.print('Hello World!') # print a message using the font

Class fonts
===========

Instances
---------

.. data::
    mono5x5
    mono6x7
    mono8x13
    mono8x8
    sans12

    These objects give access to pixel maps of font sets for use on the **Kookaberry**'s display, or any other :ref:`framebuf` display.

    The font sizes are as shown in the names of the objects:

    - ``mono5x5`` font size is 5 pixels wide by 5 pixels high
    - ``mono6x7`` font size is 6 pixels wide by 7 pixels high
    - ``mono8x13`` font size is 8 pixels wide by 13 pixels high
    - ``mono8x8`` font size is 8 pixels wide by 8 pixels high
    - ``sans12`` font size is sans serif 12 pixels wide by 12 pixels high
  