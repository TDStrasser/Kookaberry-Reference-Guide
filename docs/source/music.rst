******************************
:mod:`music` --- Playing Music
******************************
.. module:: music
   :synopsis: Module for playing music


.. _music:

This module supports the playing of music stored as notation files.  This module is fully compatible with the **micro:bit**.

Module music
============

Example Usage::

    # Plays a series of notes using a loudspeaker module connected to Pin P1
    import music
    from machine import Pin

    notes_list = ['e4', 'f#', 'g', 'a', 'b', 'c5', 'd', 'e'] # The notes to be played
    music.play(notes_list, Pin('P1')) # Play the notes on the loudspeaker

Music Functions
---------------

.. function:: music.play(notes, pin=None, wait=True, loop=False)

    Plays the music given by *notes* on the *pin* using pulse-width-modulation (PWM).

    *notes* can be a string, such as ``"c1:4"``, or a list of notes as strings, such as ["c","d","e"]
    The duration and octave values are reset to their defaults (of 4 each) before the music is played.

    *pin* is the ouput pin created by :ref:`machine.Pin`, or can be a string, one of ('P1', 'P2', 'P3A', 'P3B', 'P4', or 'P5').
    The output pin can be used to override the default pin=None which prevents sounds from being played.

    If wait is set to ``True``, playing is blocking, and the music will be played to the end.

    If loop is set to ``True``, the music repeats until the function *music.stop()* is called. Set wait to ``False`` to use this.

    An individual note is specified as: NOTE[octave][:duration].

    - Notes are written as a string with quotes: e.g. ``"c4:8"``. This is a c note in octave 4 with a duration of 8 ticks (a minim or 2 crotchet beats).
    - Notes are the letters a to g with or without an accidental (# for a sharp, b for a flat). 
    - Lower case or upper case notes are the same. eg. A and a are the same. Ab is A-flat and C# is C-sharp.
    - Use r or R for a rest (silence).
    - If the octave is left out it defaults to 4 (containing middle C).
    - If the duration is left out it defaults to 4 (a crotchet).
    - For example, a2:4 refers to the note “A” in octave 2 that lasts for four ticks (a tick is an arbitrary length of time defined by a tempo setting function).
    - The octave and duration parameters are states that carry over to subsequent notes until re-specified. e.g. [‘c4:1’, ‘e’, ‘g:8’] The e is octave 4 for 1 tick. The g is octave 4 for 8 ticks.

.. function:: music.set_tempo(ticks=4, bpm=120)

    Sets the tempo for playback.

    The number of *ticks*, expressed as an integer, make a beat. The default is 4 ticks per beat.

    *bpm* is the tempo for each beat, in beats per minute, expressed as an integer. The default is 120 bpm.

.. function:: music.get_tempo()

    Gets the current tempo as a tuple of integers: (bpm, ticks).

    To display the tuple from get_tempo it can be converted to a string::

        display.print(str(music.get_tempo()))
        # Will print the default values as '(120,4)'

    Tuple unpacking can be used instead of indices:: 
        
        bpm, ticks = music.get_tempo() 
        # Assigns default values to variable bpm and ticks respectively
        
.. function:: music.stop(pin=None)

    Stops all music playback on the built-in speaker and any pin outputting sound.

    This is used in conjunction with the functions *music.play()* or *music.pitch()* when the ``wait=False`` argument is used so that the music plays in the background,
    and particularly must be used to stop music playing when the ``loop=True`` argument is used in which case the music loops forever.

    An optional argument *pin* can be provided to specify a particular pin::
        
        music.stop(pin='P1').

.. function:: music.reset()

    Resets the state of the following attributes to: ticks = 4; bpm = 120; duration = 4; octave = 4

.. function:: music.pitch(frequency, duration=-1, pin=pin0, wait=True)

    Plays a pitch at the integer *frequency* given for the *duration* specified in milliseconds.

    Only one pitch can be played on one pin at any one time.

    If *duration* is negative the pitch is played continuously until either the blocking call is interrupted or, 
    in the case of a background call, a new frequency is set or *music.stop()* is called.

    An optional argument to specify the output *pin* can be used to override the default of ``None``which causes no sound to play.

    If *wait* is set to ``True``, this function is blocking and the script will wait for the sound to complete.  
    If *wait* is set to ``False`` the script will continue with the sound playing in the background until completion or until *music.stop()* is called.

Some Music Tunes
----------------

The **Kookaberry** is usally shipped with a library of tunes *musictunes* in the ``/lib`` folder.

This script will access that library and play any particular tune selected::

    # Demonstration of the Kookaberry Music functionality
    # A loudspeaker is attached to plug P2
    # Begin code
    import machine, kooka, musictunes, music, fonts
    p = musictunes.tunes.keys() # A dictionary of tunes
    names = sorted(list(p)) # Sort the tunes by name in alphabetic order
    ptr = 0 # Initialises the tune pointer
    disp = kooka.display # Creates the display object
    spkrpin = 'P2' # The loudspeaker pin - attach a loudspeaker module
    # The main loop begins here
    while not kooka.button_a.was_pressed():
        # Display the static text
        disp.fill(0)
        disp.setfont(fonts.mono8x8)
        disp.text('Music Demo', 0, 6)
        disp.setfont(fonts.mono5x5)
        disp.text('Plug Speaker into %s' % spkrpin, 0, 16)
        disp.setfont(fonts.mono6x7)
        disp.text('X', 0, 60) # button A exits the script
        disp.text('Prev', 20, 60) # button C navigates to the previous tune
        disp.text('Next', 55, 60) # button D navigates to the next tune
        disp.text('Play', 95, 60) # button B plays the current tune
        disp.setfont(fonts.mono8x8)
        disp.text('%s' % names[ptr], 0, 30) # Display the name of the current tune
        disp.text('%d of %d' % (ptr+1, len(names)), 20, 50)  # Display the tune number and total tunes
        
        # Navigate using the C and D buttons
        if kooka.button_c.was_pressed(): ptr = max(0, ptr-1)
        if kooka.button_d.was_pressed(): ptr = min(len(names)-1, ptr+1)
        # Play the current tune using button B
        if kooka.button_b.was_pressed(): music.play(musictunes.tunes[names[ptr]], pin=machine.Pin(spkrpin))

        disp.show() # Update the physical Display

This script is the contents of the *musictunes* library module::

    tunes ={
           'BA_DING' : ('b5:1', 'e6:3'),
           'BADDY' : ('c3:3', 'r', 'd:2', 'd#', 'r', 'c', 'r', 'f#:8'),
           'BIRTHDAY' : ('c4:3', 'c:1', 'd:4', 'c:4', 'f', 'e:8', 'c:3', 'c:1', 'd:4', 'c:4', 'g', 'f:8', 'c:3', 'c:1', 'c5:4', 'a4', 'f', 'e', 'd', 'a#:3', 'a#:1', 'a:4', 'f', 'g', 'f:8'),
           'BLUES' : ('c2:2', 'e', 'g', 'a', 'a#', 'a', 'g', 'e', 'c2:2', 'e', 'g', 'a', 'a#', 'a', 'g', 'e', 'f', 'a', 'c3', 'd', 'd#', 'd', 'c', 'a2', 'c2:2', 'e', 'g', 'a', 'a#', 'a', 'g', 'e', 'g', 'b', 'd3', 'f', 'f2', 'a', 'c3', 'd#', 'c2:2', 'e', 'g', 'e', 'g', 'f', 'e', 'd'),
           'CHASE' : ('a4:1', 'b', 'c5', 'b4', 'a:2', 'r', 'a:1', 'b', 'c5', 'b4', 'a:2', 'r', 'a:2', 'e5', 'd#', 'e', 'f', 'e', 'd#', 'e', 'b4:1', 'c5', 'd', 'c', 'b4:2', 'r', 'b:1', 'c5', 'd', 'c', 'b4:2', 'r', 'b:2', 'e5', 'd#', 'e', 'f', 'e', 'd#', 'e'),
           'DADADADUM' : ('r4:2', 'g', 'g', 'g', 'eb:8', 'r:2', 'f', 'f', 'f', 'd:8'),
           'ENTERTAINER' : ('d4:1', 'd#', 'e', 'c5:2', 'e4:1', 'c5:2', 'e4:1', 'c5:3', 'c:1', 'd', 'd#', 'e', 'c', 'd', 'e:2', 'b4:1', 'd5:2', 'c:4'),
           'FUNERAL' : ('c3:4', 'c:3', 'c:1', 'c:4', 'd#:3', 'd:1', 'd:3', 'c:1', 'c:3', 'b2:1', 'c3:4'),
           'FUNK' : ('c2:2', 'c', 'd#', 'c:1', 'f:2', 'c:1', 'f:2', 'f#', 'g', 'c', 'c', 'g', 'c:1', 'f#:2', 'c:1', 'f#:2', 'f', 'd#'),
           'JUMP_DOWN' : ('g5:1', 'f', 'e', 'd', 'c'),
           'JUMP_UP' : ('c5:1', 'd', 'e', 'f', 'g'),
           'NYAN' : ('f#5:2', 'g#', 'c#:1', 'd#:2', 'b4:1', 'd5:1', 'c#', 'b4:2', 'b', 'c#5', 'd', 'd:1', 'c#', 'b4:1', 'c#5:1', 'd#', 'f#', 'g#', 'd#', 'f#', 'c#', 'd', 'b4', 'c#5', 'b4', 'd#5:2', 'f#', 'g#:1', 'd#', 'f#', 'c#', 'd#', 'b4', 'd5', 'd#', 'd', 'c#', 'b4', 'c#5', 'd:2', 'b4:1', 'c#5', 'd#', 'f#', 'c#', 'd', 'c#', 'b4', 'c#5:2', 'b4', 'c#5', 'b4', 'f#:1', 'g#', 'b:2', 'f#:1', 'g#', 'b', 'c#5', 'd#', 'b4', 'e5', 'd#', 'e', 'f#', 'b4:2', 'b', 'f#:1', 'g#', 'b', 'f#', 'e5', 'd#', 'c#', 'b4', 'f#', 'd#', 'e', 'f#', 'b:2', 'f#:1', 'g#', 'b:2', 'f#:1', 'g#', 'b', 'b', 'c#5', 'd#', 'b4', 'f#', 'g#', 'f#', 'b:2', 'b:1', 'a#', 'b', 'f#', 'g#', 'b', 'e5', 'd#', 'e', 'f#', 'b4:2', 'c#5'),
           'ODE' : ('e4', 'e', 'f', 'g', 'g', 'f', 'e', 'd', 'c', 'c', 'd', 'e', 'e:6', 'd:2', 'd:8', 'e:4', 'e', 'f', 'g', 'g', 'f', 'e', 'd', 'c', 'c', 'd', 'e', 'd:6', 'c:2', 'c:8'),
           'POWER_DOWN' : ('g5:1', 'd#', 'c', 'g4:2', 'b:1', 'c5:3'),
           'POWER_UP' : ('g4:1', 'c5', 'e', 'g:2', 'e:1', 'g:3'),
           'PRELUDE' : ('c4:1', 'e', 'g', 'c5', 'e', 'g4', 'c5', 'e', 'c4', 'e', 'g', 'c5', 'e', 'g4', 'c5', 'e', 'c4', 'd', 'g', 'd5', 'f', 'g4', 'd5', 'f', 'c4', 'd', 'g', 'd5', 'f', 'g4', 'd5', 'f', 'b3', 'd4', 'g', 'd5', 'f', 'g4', 'd5', 'f', 'b3', 'd4', 'g', 'd5', 'f', 'g4', 'd5', 'f', 'c4', 'e', 'g', 'c5', 'e', 'g4', 'c5', 'e', 'c4', 'e', 'g', 'c5', 'e', 'g4', 'c5', 'e'),
           'PUNCHLINE' : ('c4:3', 'g3:1', 'f#', 'g', 'g#:3', 'g', 'r', 'b', 'c4'),
           'PYTHON' : ('d5:1', 'b4', 'r', 'b', 'b', 'a#', 'b', 'g5', 'r', 'd', 'd', 'r', 'b4', 'c5', 'r', 'c', 'c', 'r', 'd', 'e:5', 'c:1', 'a4', 'r', 'a', 'a', 'g#', 'a', 'f#5', 'r', 'e', 'e', 'r', 'c', 'b4', 'r', 'b', 'b', 'r', 'c5', 'd:5', 'd:1', 'b4', 'r', 'b', 'b', 'a#', 'b', 'b5', 'r', 'g', 'g', 'r', 'd', 'c#', 'r', 'a', 'a', 'r', 'a', 'a:5', 'g:1', 'f#:2', 'a:1', 'a', 'g#', 'a', 'e:2', 'a:1', 'a', 'g#', 'a', 'd', 'r', 'c#', 'd', 'r', 'c#', 'd:2', 'r:3'),
           'RINGTONE' : ('c4:1', 'd', 'e:2', 'g', 'd:1', 'e', 'f:2', 'a', 'e:1', 'f', 'g:2', 'b', 'c5:4'),
           'WAWAWAWAA' : ('e3:3', 'r:1', 'd#:3', 'r:1', 'd:4', 'r:1', 'c#:8'),
           'WEDDING' : ('c4:4', 'f:3', 'f:1', 'f:8', 'c:4', 'g:3', 'e:1', 'f:8', 'c:4', 'f:3', 'a:1', 'c5:4', 'a4:3', 'f:1', 'f:4', 'e:3', 'f:1', 'g:8')
           }





