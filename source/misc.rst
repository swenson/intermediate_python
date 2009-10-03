Miscellaneous Topics
====================

Decorators
----------

Decorators are ways to modify functions and classes as you
create them to add certain features, normally limiting the amount
of boiler-plate code you have to write.

Function Decorators
^^^^^^^^^^^^^^^^^^^

One pattern sometimes seen in computationally intensive
functions is caching the result.  This is often called
memoization.

For example, you might have a normal function and a cached version
of it, like, for example, the classical factorial function,
e.g., :math:`4! = 4\times 3\times 2\times 1`.  The standard
factorial function is::

  def factorial(n):
    if n == 0 or n == 1:
      return 1
    else:
      return n * factorial(n - 1)

However, if we previously calculated ``factorial(3)``, then
there should be no reason to recalculate it just to calculate
``factorial(4)``.  So, we might be tempted to cache the value
with, say::

  def factorial_cache(n):
    global cache
    if n in cache:
      return cache[n]
    else:
      f = 1
      if n > 1:
        f = n * factorial_cache(n - 1)
      cache[n] = f

This a bit inelegant and complicated for the simple factorial
function, and it even uses a global, since recursive calls
can't easily share state.  A better way would be if we could
have a ``@memoize`` decorator with the original function,
i.e.::

  @memoize
  def factorial(n):
    if n == 0 or n == 1:
      return 1
    else:
      return n * factorial(n - 1)

We now need to write the `memoize` function::

  def memoize(fun):
    _cache = {}
    def cached_fun(*args):
      if args in _cache: return _cache[args]
      else:
        f = fun(*args)
        _cache[args] = f
        return f
    return cached_fun

Now we can use the ``@memoize`` decorator to remember the return values of function calls to arbitrary functions.

Class Decorators
^^^^^^^^^^^^^^^^


Scoping
-------

Python's scoping rules can be a tad odd at times.  Take, for example,
the following two snippets of code::

  x = 1
  def f():
    print x
  f()

And::

  x = 1
  def f():
    print x 
    x = 2
  f()

The first piece of code does exactly what you think it does: it prints out 
``1`` to the screen.  But the second one will throw the following error in
Python 2.6::

  UnboundLocalError: local variable 'x' referenced before assignment

This happens because the assignment immediately following the ``print``
statement makes the ``x`` variable have local scope in the function,
and you are referencing the local variable ``x`` before it has been
assigned.  This doesn't happen in the first example because, since
you never make an assignment, it searches through all of the scopes
accessible to it for ``x``, eventually finding it in the global scope.

One primary purpose for this seemingly odd behavior is to prevent
you from redefining an import function or class from within your function.
For example, you might write code that does the following::

  def f():
    hex = "1abcdef"
    print hex
  f()
  print hex

If the above code accessed the built-in ``hex`` function, then we could
never again use the original function!  Luckily, this scoping rule is in
place, so that the above code outputs::

  1abcdef
  <built-in function hex>

This way, we can have our built-in functions but be free to use whatever
names we want inside functions and classes without worrying about affecting
code elsewhere.



Optimization Tips
-----------------

Optimizing Python can be tricky.  Different versions of the Python language and different
implementations of it can behave radically different.
An optimization trick that worked well in CPython 2.5 might not work quite as well
in CPython 2.6, and may even hurt performance in Jython 2.5.


String concatenation
^^^^^^^^^^^^^^^^^^^^

The classic optimization problem in Python is string concatenation.
This general problem arises a lot in text generation domains, such as web development, creating markup
language documents (such as XML), and so forth.
It's classic because naive methods often yield poor performance than clever techniques.

The most naive method is to do something like this::

  all = ""
  string_list = ["this", "is", "some", "text"]

  for x in string_list:
    all = all + x

This has poor performance because Python strings are immutable -- once created, they cannot
be changed.
You must make a copy with some new property you are looking for.
Doing in-place edits are emulated in this way, and every concatenation above will require the old string to be garbage collected, and a new string to be allocated.
This is very wasteful.

One of the better ways is to change the for-loop above to be::

  all = ''.join(string_list)

The ``join`` method on a string in Python takes all of its arguments and appends them, using
the string being joined on as a separator.
This defeats the above problem with allocation and garbage collection since the ``join`` method is implemented in a lower-level language, such as C, that has some kind of mutable structure, and since ``join`` knows that the goal of this is to produce a single string.

Disassembly
^^^^^^^^^^^

Sometimes Python code is behaving oddly, or taking much longer than you would think.
One way to figure out what the Python interpreter is actually doing is to disassemble
your code snippet and see exactly what Python is actually executing.

The ``dis`` module does exactly this::

  >>> import dis
  >>> dis.dis(compile('x = 14 + 27', 'none', 'single'))
    1           0 LOAD_CONST               3 (41)
                3 STORE_NAME               0 (x)
                6 LOAD_CONST               2 (None)
                9 RETURN_VALUE        

This command compiled the single statement of ``x = 14 + 27`` into the VM's machine code (in this case, CPython 2.6.2), with associated filename of ``none`` in single-statement mode.

Here, we can see that Python is at least smart enough to add the two constants together for us, so as not to waste time executing ``14 + 27`` every time this code is run.
