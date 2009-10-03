Objects and Typing
==================

Duck typing
-----------

Duck typing is the principle that if a variable acts like a duck, looks like a duck, and sounds like a duck, then maybe it's as good as being a duck.

This simplifies problems present in many other languages.
For example, in a language like Java, you often have to check an object's class, then cast it to be the class you are interested in, and then finally use it.

In Python, object accesses are powerful and generic enough for explicit type checking and casting-like operations to be only rarely used.

For example, in Python, you might have some code that looks like the following::

  a = b[0] + b[1]

In this code snippet, you may not care what the underlying structure for ``b`` is really:
It could be a ``tuple``, a ``list``, or something more exotic.
It doesn't really matter, as we are only interested in operating on the first two of the
objects it "contains", for whateve that means.
In this instance, ``b`` is duck typed to be a list.


Exercises
---------

1. Python optimization is a bit less aggressive than many compiled languages.
   For example, in C, you might have an expression like::

  x = x + y;

   C might optimize this into a single CPU instruction, the equivalent of ``x += y``.
   However, Python will make no such assumptions.

   Use the ``dis`` module to explain the difference between ``x = x + y`` and ``x += y``,
   and explain how this might be affect duck typing.
