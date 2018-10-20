# Python

PDB commands:

l list -> it keeps moving ahead
ll long list -> it captures all data shown by l

n next

s step

self._x, self._y  to see all variables

self._x

w to get call stack where program will be

c for continue till next breakpoint

b to set new breakpoint, b amaze/runner:58    or     b filename:123    or         b:123    or    b  filename.methodname

r return > step out of this level

p is for printing value of expression or variables.  e.g. 
(Pdb) p self.filename == 'logs'
True
(Pdb) p [m for m in self.filename]
['l', 'o', 'g', 's']

--------------------------
Starting in Python 3.7, there’s another way to enter the debugger. PEP 553 describes the built-in function breakpoint(), which makes entering the debugger easy and consistent:

breakpoint()
By default, breakpoint() will import pdb and call pdb.set_trace(), as shown above. However, using breakpoint() is more flexible and allows you to control debugging behavior via its API and use of the environment variable PYTHONBREAKPOINT. For example, setting PYTHONBREAKPOINT=0 in your environment will completely disable breakpoint(), thus disabling debugging. If you’re using Python 3.7 or later, I encourage you to use breakpoint() instead of pdb.set_trace().

You can also break into the debugger, without modifying the source and using pdb.set_trace() or breakpoint(), by running Python directly from the command-line and passing the option -m pdb. If your application accepts command-line arguments, pass them as you normally would after the filename. For example:

$ python3 -m pdb app.py arg1 arg2

---------------------------


# Documentation using Sphinx
from restructuredText to any format
