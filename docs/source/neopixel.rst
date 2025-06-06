****************************************
:mod:`neopixel` --- RGB colour LED array
****************************************
.. module:: neopixel
   :synopsis: RGB colour LED array

.. _neopixel:


This class enables the control of an arbitrarily long string of RGB coloured LEDs.  
Each LED in the string can be individually set to any colour by setting values for the R (red), G (green), and B (blue) components of the LED.

NeoPixels (als known as WS2812 LEDs) are available as LED strips, and as 2-dimensional arrays of LEDs which can function as displays.

.. important:
   NeoPixels can draw up to 20 milliamps each when fully lit, and so it is recommended that no more than 8 NeoPixels be powered directly from the **Kookaberry**.
   If more NeoPixels are required then an intermediate power injection accessory circuit board should be used.  
   The **Kookaberry** would otherwise be overloaded and would shut down.


Class NeoPixel
==============

Example usage::

    # NeoPixel Demo - Cycle Colours Along The NeoPixel String
    pixels_in_array = 8 # Change this number to suit the number of NeoPixels
    
    import neopixel, machine, time

    from kooka import display # Construct the Display object
    display.fill(0)

    np = neopixel.NeoPixel(machine.Pin('P1'), pixels_in_array) # Construct the NeoPixel object
    
    # Define RGB colours
    red = (63,0,0)
    green = (0,63,0)
    blue = (0,0,63)
    yellow = (31,31,0)
    cyan = (0,31,31)
    violet = (31,0,31)
    white = (21,21,21)
    black = (0,0,0)
    colours =[red,green,blue,yellow,cyan,violet,white]

    n = np.n # Read back the number of pixels
    display.print("No of pixels = ",n)
    display.print("Cycle individual pixels")
    # cycle
    for colour in colours:
        for i in range(n):
            np.fill(black) # Set all pixels to black
            np[i % n] = colour # Change the current neopixel's data from black
            np.write()  # Transfer all neopixel data
            time.sleep_ms(100) # Pause sufficient for observation
    display.print('Finished')


NeoPixel Constructors
---------------------

.. class:: neopixel.NeoPixel(pin, pixels_in_array)

   Creates a *neopixel* object.  
   
   The parameter *pin* can be a string naming the connector, like ``"P2"``, or a :class:`machine.Pin` object representing the
   pin on the connector.

   The parameter *pixels_in_array* is an integer specifying how many LEDs are in the NeoPixel string.  
   This value can be interrogated after the *neopixel* object is created using the object property *NeoPixel.n*.

   The *neopixel* object appears as a buffer of length *pixels_in_array*. 
   Each element of the *neopixel* buffer can be set by an array *[r,g,b]* representing the intensity of each constituent colour in the range 0 to 255 inclusive.
   
   Examples of primary colours are:
   
     - *[0,0,0]* represents the colour black (all LEDs off)
     - *[255,0,0]* is 100% intensity red
     - *[127,0,0]* is 50% intensity red
     - *[0,127,0]* is 50% intensity green
     - *[0,0,127]* is 50% intensity blue 
     - *[63,63,63]* is 25% intensity white (red + green + blue)
     - *[63,63,0]* is 25% intensity yellow (red + green)
     - *[0,63,63]* is 25% intensity cyan (green + blue)
     - *[63,0,63]* is 25% intensity violet (red + blue)


    To set the first LED in the *neopixel* array to 25% white, use *NeoPixel[0] = [63,63,63]*.

NeoPixel Methods
----------------

.. method:: NeoPixel.write()

    Write the bytes in *neopixel* buffer to a NeoPixel-like device on the given *pin* when the *neopixel* object was created.  
    Interrupts will be disabled during the entire write to get accurate timing.
    The physical NeoPixel LED string will then be lit in accordance with the pattern set in the *neopixel* object buffer.

.. method:: NeoPixel.fill(colour)

    Fill the *neopixel* object buffer array with the specified *colour*.  
    An immediately subsequent *NeoPixel.write()* will set all the NeoPixels to the specified *colour*.
