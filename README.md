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

(Pdb) b util.get_path, not filename.startswith('/')

r return > step out of this level

p is for printing value of expression or variables.  e.g. 

(Pdb) p self.filename == 'logs'
True
(Pdb) p [m for m in self.filename]
['l', 'o', 'g', 's']


pp You can also use the command pp (pretty-print) to pretty-print expressions. This is helpful if you want to print a variable or expression with a large amount of output, e.g. lists and dictionaries. Pretty-printing keeps objects on a single line if it can or breaks them onto multiple lines if they don’t fit within the allowed width.

unt	unt(il) [lineno]	Without lineno, continue execution until the line with a number greater than the current one is reached. With lineno, continue execution until a line with a number greater or equal to that is reached. In both cases, also stop when the current frame returns.

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

-------------------------
Displaying Expressions
Similar to printing expressions with p and pp, you can use the command display [expression] to tell pdb to automatically display the value of an expression, if it changed, when execution stops. Use the command undisplay [expression] to clear a display expression.

Here’s the syntax and description for both commands:

Command	Syntax	Description
display	display [expression]	Display the value of expression if it changed, each time execution stops in the current frame. Without expression, list all display expressions for the current frame.
undisplay	undisplay [expression]	Do not display expression any more in the current frame. Without expression, clear all display expressions for the current frame.
Below is an example, example4display.py, demonstrating its use with a loop:

$ ./example4display.py 
> /code/example4display.py(9)get_path()
-> head, tail = os.path.split(fname)
(Pdb) ll
  6     def get_path(fname):
  7         """Return file's path or empty string if no path."""
  8         import pdb; pdb.set_trace()
  9  ->     head, tail = os.path.split(fname)
 10         for char in tail:
 11             pass  # Check filename char
 12         return head
(Pdb) b 11
Breakpoint 1 at /code/example4display.py:11
(Pdb) c
> /code/example4display.py(11)get_path()
-> pass  # Check filename char
(Pdb) display char
display char: 'e'
(Pdb) c
> /code/example4display.py(11)get_path()
-> pass  # Check filename char
display char: 'x'  [old: 'e']
(Pdb) 
> /code/example4display.py(11)get_path()
-> pass  # Check filename char
display char: 'a'  [old: 'x']
(Pdb) 
> /code/example4display.py(11)get_path()
-> pass  # Check filename char
display char: 'm'  [old: 'a']
In the output above, pdb automatically displayed the value of the char variable because each time the breakpoint was hit its value had changed. Sometimes this is helpful and exactly what you want, but there’s another way to use display.

You can enter display multiple times to build a watch list of expressions. This can be easier to use than p. After adding all of the expressions you’re interested in, simply enter display to see the current values:

$ ./example4display.py 
> /code/example4display.py(9)get_path()
-> head, tail = os.path.split(fname)
(Pdb) ll
  6     def get_path(fname):
  7         """Return file's path or empty string if no path."""
  8         import pdb; pdb.set_trace()
  9  ->     head, tail = os.path.split(fname)
 10         for char in tail:
 11             pass  # Check filename char
 12         return head
(Pdb) b 11
Breakpoint 1 at /code/example4display.py:11
(Pdb) c
> /code/example4display.py(11)get_path()
-> pass  # Check filename char
(Pdb) display char
display char: 'e'
(Pdb) display fname
display fname: './example4display.py'
(Pdb) display head
display head: '.'
(Pdb) display tail
display tail: 'example4display.py'
(Pdb) c
> /code/example4display.py(11)get_path()
-> pass  # Check filename char
display char: 'x'  [old: 'e']
(Pdb) display
Currently displaying:
char: 'x'
fname: './example4display.py'
head: '.'
tail: 'example4display.py'

--------------------------
Starting in Python 3.7, there’s another way to enter the debugger. PEP 553 describes the built-in function breakpoint(), which makes entering the debugger easy and consistent:

breakpoint()
By default, breakpoint() will import pdb and call pdb.set_trace(), as shown above. However, using breakpoint() is more flexible and allows you to control debugging behavior via its API and use of the environment variable PYTHONBREAKPOINT. For example, setting PYTHONBREAKPOINT=0 in your environment will completely disable breakpoint(), thus disabling debugging. If you’re using Python 3.7 or later, I encourage you to use breakpoint() instead of pdb.set_trace().

You can also break into the debugger, without modifying the source and using pdb.set_trace() or breakpoint(), by running Python directly from the command-line and passing the option -m pdb. If your application accepts command-line arguments, pass them as you normally would after the filename. For example:

$ python3 -m pdb app.py arg1 arg2

---------------------------


# Documentation using Sphinx
from restructuredText to any format
