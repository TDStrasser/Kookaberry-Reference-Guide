.. currentmodule:: machine
.. _machine.Timer:

class Timer -- control internal timers
======================================

Timers can be used for a great variety of tasks.  At the moment, only
the simplest case of calling a function periodically is available for programming use and is implemented as a software Timer class.

The Timer consists of a counter that counts up at a certain rate.  The rate
at which it counts is in milliseconds.  When the counter reaches the timer period it triggers an
event, and the counter resets back to zero.  By using the callback method,
the timer event can call a MicroPython function.

Example usage to toggle a LED at a fixed frequency::

   from machine import Timer
   import kooka
   tim = Timer(-1, freq=2,callback= (lambda t:kooka.led_red.toggle())) # create a software timer object with callback

Example using named function for the callback::

   from machine import Timer
   counter = 0           # Initialise a counter
   def tick(tim):        # The callback function
     global counter      # Reference the counter
     counter += 1        # Increment the counter on each callback
     print(counter)      # Print the counter's value
   tim = Timer(-1, freq=1, callback=tick)  # create a software timer object - trigger at 1Hz - with callback

*Note:* Memory can't be allocated during a callback (an interrupt) and so
exceptions raised within a callback don't give much information.  
See the `core MicroPython documentation <https://docs.micropython.org/en/latest/library/micropython.html#micropython.alloc_emergency_exception_buf>'_'
for how to get around this limitation.


Timer Constructors
------------------

.. class:: machine.Timer(-1, ...)

   Construct a new software timer object.  If additional
   arguments are given, then the timer is initialised by ``init(...)``.

   Because of the differences between **Kookaberry** microcomputer models, 
   and hardware timers being pre-allocated to internal functions,
   only software timers (*id*=-1) are available for user scripts.

Timer Methods
-------------

.. method:: Timer.init(\*, freq=1, period=1000, callback=None, mode=Timer.PERIODIC)

   Initialise the timer.  Initialisation must be either by frequency (in Hz)
   or by prescaler and period::

       tim.init(freq=100)                  # set the timer to trigger at 100Hz
       tim.init(prescaler=83, period=999)  # set the prescaler and period directly

   Keyword arguments:

     - ``freq`` --- (default is 1Hz) specifies the periodic frequency of the timer. You might also
       view this as the frequency with which the timer goes through one complete cycle.

     - ``period`` --- (default is 1000msecs) period in milliseconds of the timer (= 1/freq).
       This determines the period of the timer (i.e. when the counter cycles). 
       The timer counter will roll-over after ``period + 1`` timer clock cycles.
       If specified with ``freq``, the ``freq`` value will override ``period``.

     - ``mode`` can be one of:

       - ``Timer.PERIODIC`` - configures the timer to repeat (default)
       - ``Timer.ONE_SHOT`` - configures the timer to operate just once.

     - ``callback`` - Sets the function to be called when the timer triggers.
         If the function is ``None`` then the callback will be disabled.

     
