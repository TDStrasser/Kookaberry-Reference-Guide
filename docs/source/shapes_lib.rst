***********************************************
:mod:`shapes_lib` --- Graphics helper functions
***********************************************
.. module:: shapes_lib
   :synopsis: Graphics helper functions

.. _shapes_lib:


The Shapes Library (shapes_lib) Module provides some extended graphics for the **Kookaberry** display 
and for any display device using the :ref:`framebuf` functionality.

The display device *dev* is passed as the first argument, and display specific arguments are appended, eg. the colour.

Available functions are:

- Regular polygons at origin (x0,y0) with given vertex radius and angle: 
  
    * polygon / fill_polygon(dev,x0,y0,vert_radius,vert_angle,other_dev_args)
    
- Arcs around origin (x0,yo) with given arc_radius and arc start and stop angles in degrees:
    
    * arc / fill_arc(dev,x0,y0,arc_radius,arc_start,arc_stop,other_dev_args)
    * circle / fill_circle(dev,x0,y0,radius,other_dev_args)

- Triangles of arbitrary shape given vertices

    * triangle / fill_triangle(x0,y0,x1,y1,x2,y2)


.. note::
    
    Filled shapes use the fill_triangle function.
    Any arbitrary shape can be constructed using the foregoing functions.


Example Usage::

    # Draw a filled circle
    from kooka import display as disp # Create the display object
    from shapes_lib import fill_circle
    disp.fill(0) # Clear the display framebuffer of lit pixels
    fill_circle(disp, 63, 31, 16, 1) # draws a filled circle about origin (63,13) with radius 16 and colour of 1 (pixels on)
    disp.show() # Transfer the framebuffer to the physical OLED display


Shapes Library Functions
------------------------

.. function:: shapes_lib.line(dev, x0, y0, radius, lin_angle, *args, **kwargs)

    Draws a radial line at a given angle and radius from the origin (x0,y0) and returns the coordinates of the rotating end of the line.

    *dev* is the display device object, usually kooka.display(), but may be any framebuffer device.

    *x0* and *y0* are the display coordinates of the line's origin

    *radius* is the length of the line

    *lin_angle* is the clockwise angle in degrees of the line about the origin from the vertical position

    *args* and *kwargs* are other arguments passed through to the display device, for example the colour.



.. function:: shapes_lib.draw_poly(dev, x0, y0, vert_radius, sides, vert_angle, fill=False, *args, **kwargs)

    Draws or fills a regular polygon at origin(x0,y0) with vertex radius and vertex angle. 

    *dev* is the display device object, usually kooka.display(), but may be any framebuffer device.

    *x0* and *y0* are the display coordinates of the line's origin

    *vert_radius* is the radius of the vertices from the origin

    *sides* is the number of sides the polygon has

    *vert_angle* is the clockwise angle in degrees of the first vertex about the origin from the vertical position

    *fill* is a boolean which indicates whether the polygon is filled (``True``) or an outline only (``False``). The default is ``False``.

    *args* and *kwargs* are other arguments passed through to the display device, for example the colour.



.. function:: shapes_lib.polygon(dev, x0, y0, vert_radius, sides, vert_angle, *args, **kwargs)

    Draws an outline polygon at origin(x0,y0) with vertex radius and vertex angle. This is a more convenient form of shapes_lib.draw_poly().

    *dev* is the display device object, usually kooka.display(), but may be any framebuffer device.

    *x0* and *y0* are the display coordinates of the line's origin

    *vert_radius* is the radius of the vertices from the origin

    *sides* is the number of sides the polygon has

    *vert_angle* is the clockwise angle in degrees of the first vertex about the origin from the vertical position

    *args* and *kwargs* are other arguments passed through to the display device, for example the colour.


.. function:: shapes_lib.fill_polygon(dev, x0, y0, vert_radius, sides, vert_angle, *args, **kwargs)

    Draws a filled polygon at origin(x0,y0) with vertex radius and vertex angle. This is a more convenient form of shapes_lib.draw_poly().

    *dev* is the display device object, usually kooka.display(), but may be any framebuffer device.

    *x0* and *y0* are the display coordinates of the line's origin

    *vert_radius* is the radius of the vertices from the origin

    *sides* is the number of sides the polygon has

    *vert_angle* is the clockwise angle in degrees of the first vertex about the origin from the vertical position

    *args* and *kwargs* are other arguments passed through to the display device, for example the colour.


.. function:: shapes_lib.draw_arc(dev, x0, y0, arc_radius, arcstart, arcstop, fill=False, *args, **kwargs)

    Draws or fills pie slice around origin (x0,y0) with radius between specified degrees. 

    *dev* is the display device object, usually kooka.display(), but may be any framebuffer device.

    *x0* and *y0* are the display coordinates of the line's origin

    *arc_radius* is the radius of the arc about the origin

    *arcstart* and *arcstop* are the clockwise angles in degrees around the origin from the vertical position of the beginning and end of the arc

    *fill* is a boolean which indicates whether the polygon is filled (``True``) or an outline only (``False``). The default is ``False``.

    *args* and *kwargs* are other arguments passed through to the display device, for example the colour.


.. function:: shapes_lib.arc(dev, x0, y0, arc_radius, arcstart, arcstop, *args, **kwargs)

    Draws an outline pie slice around origin (x0,y0) with radius between specified degrees. This is a more convenient form of shapes_lib.draw_arc().

    *dev* is the display device object, usually kooka.display(), but may be any framebuffer device.

    *x0* and *y0* are the display coordinates of the line's origin

    *vert_radius* is the radius of the vertices from the origin

    *arcstart* and *arcstop* are the clockwise angles in degrees around the origin from the vertical position of the beginning and end of the arc

    *args* and *kwargs* are other arguments passed through to the display device, for example the colour.


.. function:: shapes_lib.fill_arc(dev, x0, y0, arc_radius, arcstart, arcstop, *args, **kwargs)

    Draws a filled pie slice around origin (x0,y0) with radius between specified degrees. This is a more convenient form of shapes_lib.draw_arc().

    *dev* is the display device object, usually kooka.display(), but may be any framebuffer device.

    *x0* and *y0* are the display coordinates of the line's origin

    *arc_radius* is the radius of the arc about the origin

    *arcstart* and *arcstop* are the clockwise angles in degrees around the origin from the vertical position of the beginning and end of the arc

    *args* and *kwargs* are other arguments passed through to the display device, for example the colour.


.. function:: shapes_lib.circle(dev, x0, y0, radius, *args, **kwargs)

    Draws an outline circle around origin (x0,y0) with radius between specified degrees. This is a more convenient form of shapes_lib.draw_arc().

    *dev* is the display device object, usually kooka.display(), but may be any framebuffer device.

    *x0* and *y0* are the display coordinates of the line's origin

    *radius* is the radius of the circle about the origin

    *args* and *kwargs* are other arguments passed through to the display device, for example the colour.


.. function:: shapes_lib.fill_circle(dev, x0, y0, radius, *args, **kwargs)

    Draws a filled circle around origin (x0,y0) with radius between specified degrees. This is a more convenient form of shapes_lib.draw_arc().

    *dev* is the display device object, usually kooka.display(), but may be any framebuffer device.

    *x0* and *y0* are the display coordinates of the line's origin

    *radius* is the radius of the circle about the origin

    *args* and *kwargs* are other arguments passed through to the display device, for example the colour.

.. function:: shapes_lib.triangle(dev, x0, y0, x1, y1, x2, y2, *args, **kwargs)

    Draws an outline triangle with the vertex coordinates (x0,y0), (x1,y1), (x2,y2)

    *dev* is the display device object, usually kooka.display(), but may be any framebuffer device.

    *args* and *kwargs* are other arguments passed through to the display device, for example the colour.


.. function:: shapes_lib.fill_triangle(dev, x0, y0, x1, y1, x2, y2, *args, **kwargs)

    Draws a filled triangle with the vertex coordinates (x0,y0), (x1,y1), (x2,y2)

    *dev* is the display device object, usually kooka.display(), but may be any framebuffer device.

    *args* and *kwargs* are other arguments passed through to the display device, for example the colour.


