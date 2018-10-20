# Python
Ref: https://realpython.com/python-debugging-pdb/

PDB commands:

l list -> it keeps moving ahead. Pass the argument . to always list 11 lines around the current line: l .
ll long list -> it captures all data shown by l

n next

s step

self._x, self._y  to see all variables

self._x

w to get call stack where program will be

c for continue till next breakpoint

b to set new breakpoint, b(reak) [ ([filename:]lineno | function) [, condition] ]
b amaze/runner:58    or     b filename:123    or         b:123    or    b  filename.methodname
Enter b with no arguments to see a list of all breakpoints

r return > step out of this level

p is for printing value of expression or variables.  e.g. 
(Pdb) p self.filename == 'logs'
True
(Pdb) p [m for m in self.filename]
['l', 'o', 'g', 's']


pp You can also use the command pp (pretty-print) to pretty-print expressions. This is helpful if you want to print a variable or expression with a large amount of output, e.g. lists and dictionaries. Pretty-printing keeps objects on a single line if it can or breaks them onto multiple lines if they don’t fit within the allowed width.

--------------------
You can disable and re-enable breakpoints using the command disable bpnumber and enable bpnumber. bpnumber is the breakpoint number from the breakpoints list’s 1st column Num. Notice the Enb column’s value change:

(Pdb) disable 1
Disabled breakpoint 1 at /code/util.py:1
(Pdb) b
Num Type         Disp Enb   Where
1   breakpoint   keep no    at /code/util.py:1
(Pdb) enable 1
Enabled breakpoint 1 at /code/util.py:1
(Pdb) b
Num Type         Disp Enb   Where
1   breakpoint   keep yes   at /code/util.py:1
(Pdb) 
To delete a breakpoint, use the command cl (clear):

cl(ear) filename:lineno
cl(ear) [bpnumber [bpnumber...]]

--------------------

Note the lines --Call-- and --Return--. This is pdb letting you know why execution was stopped. n (next) and s (step) will stop before a function returns. That’s why you see the --Return-- lines above.

Also note ->'.' at the end of the line after the first --Return-- above:

--Return--
> /code/example3.py(9)get_path()->'.'
-> return head
(Pdb) 
When pdb stops at the end of a function before it returns, it also prints the return value for you. In this example it’s '.'.


--------------------------
Starting in Python 3.7, there’s another way to enter the debugger. PEP 553 describes the built-in function breakpoint(), which makes entering the debugger easy and consistent:

breakpoint()
By default, breakpoint() will import pdb and call pdb.set_trace(), as shown above. However, using breakpoint() is more flexible and allows you to control debugging behavior via its API and use of the environment variable PYTHONBREAKPOINT. For example, setting PYTHONBREAKPOINT=0 in your environment will completely disable breakpoint(), thus disabling debugging. If you’re using Python 3.7 or later, I encourage you to use breakpoint() instead of pdb.set_trace().

You can also break into the debugger, without modifying the source and using pdb.set_trace() or breakpoint(), by running Python directly from the command-line and passing the option -m pdb. If your application accepts command-line arguments, pass them as you normally would after the filename. For example:

$ python3 -m pdb app.py arg1 arg2

---------------------------


# Documentation using Sphinx
from restructuredText to any format
