code128
==========

code128 is a simple library to create Code-128 barcodes.

License: GNU Lesser General Public License v2 or later (LGPLv2+) (LGPLv2+)

https://pypi.python.org/pypi/code128/

Author: Felix Knopf

version: 0.3

Features
---------
* optimal codes (use code128C to encode long sequences of digits; lazy switch between Code128A and B)
* full latin-1 charset is supported
* no additional libraries needed for svg output
* output as PIL Image objects (PIL requiered)
* command line tool and gui

Dependencies
~~~~~~~~~~~~
* **Python3** (Tested with 3.3 and 3.4, other versions should work, too)
* **setuptools** to use the setup script or pip, usually preinstalled
* *optional*: **PIL**, or compatible fork (**Pillow** is recommended) to save barcodes as raster graphics


Usage
-----

with Python
~~~~~~~~~~~

.. code:: python

	import code128
	
	code128.image("Hello World").save("Hello World.png")  # with PIL present
	
	with open("Hello World.svg", "w") as f:
		f.write(code128.svg("Hello World"))

		

