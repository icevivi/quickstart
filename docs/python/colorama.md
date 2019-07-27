colorama 0.4.1
===============

License
=======

Copyright Jonathan Hartley 2013. BSD 3-Clause license; see LICENSE file.

version
=======
0.4.1


Download and docs:
    https://pypi.org/project/colorama/
Source code & Development:
    https://github.com/tartley/colorama

Usage
=====

Initialisation
--------------

Applications should initialise Colorama using:

.. code-block:: python

    from colorama import init
    init()

On Windows, calling ``init()`` will filter ANSI escape sequences out of any
text sent to ``stdout`` or ``stderr``, and replace them with equivalent Win32
calls.

On other platforms, calling ``init()`` has no effect (unless you request other
optional functionality; see "Init Keyword Args", below). By design, this permits
applications to call ``init()`` unconditionally on all platforms, after which
ANSI output should just work.

To stop using colorama before your program exits, simply call ``deinit()``.
This will restore ``stdout`` and ``stderr`` to their original values, so that
Colorama is disabled. To resume using Colorama again, call ``reinit()``; it is
cheaper to calling ``init()`` again (but does the same thing).


Colored Output
--------------

Cross-platform printing of colored text can then be done using Colorama's
constant shorthand for ANSI escape sequences:

.. code-block:: python

    from colorama import Fore, Back, Style
    print(Fore.RED + 'some red text')
    print(Back.GREEN + 'and with a green background')
    print(Style.DIM + 'and in dim text')
    print(Style.RESET_ALL)
    print('back to normal now')

...or simply by manually printing ANSI sequences from your own code:

.. code-block:: python

    print('\033[31m' + 'some red text')
    print('\033[30m') # and reset to default color

...or, Colorama can be used happily in conjunction with existing ANSI libraries
such as Termcolor:

.. code-block:: python

    from colorama import init
    from termcolor import colored

    # use Colorama to make Termcolor work on Windows too
    init()

    # then use Termcolor for all colored text output
    print(colored('Hello, World!', 'green', 'on_red'))

Available formatting constants are::

    Fore: BLACK, RED, GREEN, YELLOW, BLUE, MAGENTA, CYAN, WHITE, RESET.
    Back: BLACK, RED, GREEN, YELLOW, BLUE, MAGENTA, CYAN, WHITE, RESET.
    Style: DIM, NORMAL, BRIGHT, RESET_ALL

``Style.RESET_ALL`` resets foreground, background, and brightness. Colorama will
perform this reset automatically on program exit.


Cursor Positioning
------------------

ANSI codes to reposition the cursor are supported. See ``demos/demo06.py`` for
an example of how to generate them.


Init Keyword Args
-----------------

``init()`` accepts some ``**kwargs`` to override default behaviour.

init(autoreset=False):
    If you find yourself repeatedly sending reset sequences to turn off color
    changes at the end of every print, then ``init(autoreset=True)`` will
    automate that:

    .. code-block:: python

        from colorama import init
        init(autoreset=True)
        print(Fore.RED + 'some red text')
        print('automatically back to default color again')

init(strip=None):
    Pass ``True`` or ``False`` to override whether ansi codes should be
    stripped from the output. The default behaviour is to strip if on Windows
    or if output is redirected (not a tty).

init(convert=None):
    Pass ``True`` or ``False`` to override whether to convert ANSI codes in the
    output into win32 calls. The default behaviour is to convert if on Windows
    and output is to a tty (terminal).

init(wrap=True):
    On Windows, colorama works by replacing ``sys.stdout`` and ``sys.stderr``
    with proxy objects, which override the ``.write()`` method to do their work.
    If this wrapping causes you problems, then this can be disabled by passing
    ``init(wrap=False)``. The default behaviour is to wrap if ``autoreset`` or
    ``strip`` or ``convert`` are True.

    When wrapping is disabled, colored printing on non-Windows platforms will
    continue to work as normal. To do cross-platform colored output, you can
    use Colorama's ``AnsiToWin32`` proxy directly:

    .. code-block:: python

        import sys
        from colorama import init, AnsiToWin32
        init(wrap=False)
        stream = AnsiToWin32(sys.stderr).stream

        # Python 3
        print(Fore.BLUE + 'blue text on stderr', file=stream)


    
