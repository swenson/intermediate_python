Threads
=======

Threads and Processes
---------------------

Threads and processes are two fundamental multiprocessing primitives -- they are used to allow a program
to execute, or appear to execute, several instructions simultaneously.  Both are supported in CPython 2.6
and later in various ways.

The essential difference is what threads and processes share and what they don't.
With threads, once a new thread is spun off, it will share *everything* with the thread that created it --
all variables will be accessible (within scope), all objects can be shared and passed between them.

Processes are the exact opposite -- once launched, they share absolutely *nothing* with the original process
that created them, execute for code base.  However, no variables are shared, and they have no shared state.

In CPython, these are implemented in fundamentally different ways.  We will go over them in detail shortly,
but one additional difference is that CPython threads will appear to execute concurrently, although for the
most part, they cannot -- there is a construct called the Global Interpreter Lock (GIL) that prevents
more than on CPython thread from executing Python code at the same time.  With processes, this is not true --
they can execute at the exact same time (assuming there are enough processors to support this), but with the cost
of communication between them being more difficult.


Python Threads
--------------

For most systems using CPython, Python threads will be standard POSIX threads (also called pthreads),
they will be real operating system threads.

Threads are simple.  The easiest way to build a thread is to give it a target function, and tell it to start::

  def go():
    s = sum(i for i in range(100))
    print("Calculated sum" str(s))
  
  import threading
  t = Thread(target=go, args=(,))
  t.start()
  t.join()

There are several things going on here.

#. We define the :func:`go()` function, which simply calculates the sum sum of all integers between 0 and 100.
   (This should be :math:`100 \times 101 / 2 = 5050`.)  We use some fancy syntax here, as well: In the first line,
   we use a *generator expression*, which is a way of building a generator from an inlined for-loop without
   actually creating an array.
#. We import the :mod:`threding` module.
#. We create a new :class:`Thread` object.    
#. We start the thread.
#. We *join* with the thread --- that is, we wait for it to finish before moving on.

This is mostly self-explanatory, and the syntax is fairly easy.  There are a few gotchas, though, when creating and joining threads.

Creating Threads
^^^^^^^^^^^^^^^^

There are two standard ways to create threads.  The first is as above: we create a new ``threading.Thread`` object
with a specified ``target``.  The other way is a more Java-like way: we subclass the ``threading.Thread`` base class.

When creating a new ``threading.Thread``


Locks, Stocks, and Two Smoking Barrels
--------------------------------------


Multiprocessing
---------------

Exercises
---------
