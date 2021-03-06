Console API
===========

For complete control over terminal formatting, Rich offers a :class:`~rich.console.Console` class. Most applications will require a single Console instance, so you may want to create one at the module level or as an attribute of your top-level object. For example::

    from rich.console import Console
    console = Console()

The console object handles the mechanics of generating ANSI escape sequences for color and style. It will auto-detect the capabilities of the terminal and convert colors if necessary.


Attributes
----------

The console will auto-detect a number of properties required when rendering.

* :obj:`~rich.console.Console.size` is the current dimensions of the terminal (which may change if you resize the window).
* :obj:`~rich.console.Console.encoding` is the default encoding (typically "utf-8").
* :obj:`~rich.console.Console.is_terminal` is a boolean that indicates if the Console instance is writing to a terminal or not.
* :obj:`~rich.console.Console.color_system` is a string containing "standard", "256" or "truecolor", or ``None`` if not writing to a terminal.


Printing
--------

To write rich content to the terminal use the :meth:`~rich.console.Console.print` method. Rich will convert any object to a string via its (``__str__``) method and perform some simple syntax highlighting. It will also do pretty printing of any containers, such as dicts and lists. If you print a string it will render :ref:`console_markup`. Here are some examples::

    console.print([1, 2, 3])
    console.print("[blue underline]Looks like a link")
    console.print(locals())
    console.print("FOO", style="white on blue")

You can also use :meth:`~rich.console.Console.print` to render objects that support the :ref:`protocol`, which includes Rich's built in objects such as :class:`~rich.text.Text`, :class:`~rich.table.Table`, and :class:`~rich.syntax.Syntax` -- or other custom objects.


Logging
-------

The :meth:`~rich.console.Console.log` methods offers the same capabilities as print, but adds some features useful for debugging a running application. Logging writes the current time in a column to the left, and the file and line where the method was called to a column on the right. Here's an example::

    >>> console.log("Hello, World!")

.. raw:: html

    <pre style="font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #7fbfbf">[16:32:08] </span>Hello, World!                                         <span style="color: #7f7f7f">&lt;stdin&gt;:1</span>
    </pre>

To help with debugging, the log() method has a ``log_locals`` parameter. If you set this to ``True``, Rich will display a table of local variables where the method was called.

Justify / Alignment
-------------------

Both print and log support a ``justify`` argument which if set must be one of "left", "right", "center", or "full".  If "left", any text printed (or logged) will be left aligned, if "right" text will be aligned to the right of the terminal, if "center" the text will be centered, and if "full" the text will be lined up with both the left and right edges of the terminal (like printed text in a book). 

The default for ``justify`` is ``None`` which will generally look the same as ``"left"`` but with a subtle different. Left justify will pad the right of the text with spaces, while a None justify will not. You will only notice the difference if you set a background color with the ``style`` argument. The following example demonstrates the difference::

    from rich.console import Console

    console = Console(width=20)

    style = "bold white on blue"
    console.print("Rich", style=style)
    console.print("Rich", style=style, justify="left")
    console.print("Rich", style=style, justify="center")
    console.print("Rich", style=style, justify="right")


This produces the following output:

.. raw:: html

    <pre style="font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #c0c0c0; background-color: #000080; font-weight: bold">Rich
    Rich               &nbsp;
            Rich       &nbsp; 
                    Rich
    </span></pre>


Exporting
---------

The Console class can export anything written to it as either text or html. To enable exporting, first set ``record=True`` on the constructor. This tells Rich to save a copy of any data you ``print()`` or ``log()``. Here's an example::

    from rich.console import Console
    console = Console(record=True)

After you have written content, you can call :meth:`~rich.console.Console.export_text` or :meth:`~rich.console.Console.export_html` to get the console output as a string. You can also call :meth:`~rich.console.Console.save_text` or :meth:`~rich.console.Console.save_html` to write the contents directly to disk.

For examples of the html output generated by Rich Console, see :ref:`appendix-colors`.
