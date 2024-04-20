*************************************************
:mod:`doomsday` --- RTC datetime helper functions
*************************************************
.. module:: doomsday
   :synopsis: Real Time Clock datetime helper


.. _doomsday:


The doomsday module contains utility functions that convert a date to the day of the week.

It is an implementation of John Conway's Doomsday algorithm.  See https://en.wikipedia.org/wiki/Doomsday_rule

Example Usage::
    
    # To obtain and print the day of the week
    from kooka import RTC
    import doomsday

    rtc = RTC() # Create a Kookaberry clock object

    current_time = rtc.datetime() # Retrives the current datetime tuple (YYYY,MM,DD,WD,HH,MM,SS,SUBS)
    day_of_week = doomsday.calc_dow(current_time[2], current_time[1], current_time[0]) # Returns a day string e.g. "Monday"
    print("Today is ", day_of_week)


Doomsday Functions
------------------

 .. method:: doomsday.calc_dow(day,  month,  year)

    Returns a character string representing the day of the week given a date. 
    One of (``Sunday``,``Monday``, ``Tuesday``, ``Wednesday``, ``Thursday``, ``Friday``, ``Saturday``)
    
    To abbreviate the day of the week to the first 3 letters, use: day_of_week = doomsday.calc_dow(day,  month,  year)[0:3]


    - *day* is an integer representing the day of the month
    - *month* is an integer (1 to 12 inclusive) representing the calendar month
    - *year* is an integer representing the calendar year

.. method:: doomsday.dow_index(day,  month,  year)

    Returns an integer representing the day of the week given a date. 
    One of ``0`` to ``7`` inclusive corresponding to (Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday)

    - *day* is an integer representing the day of the month
    - *month* is an integer (1 to 12 inclusive) representing the calendar month
    - *year* is an integer representing the calendar year

