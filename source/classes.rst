Classes
=======

New-style versus old-style
--------------------------

Before Python 2.2, the standard classes acted slightly differently.
Sometimes, people refer to these classes as "old-style" classes.
To remain compatible with older code, if you simply declare a class
with no inheritence, you will still get an old-style class.
For example::

  class OldClass:
    pass

This declares :class:`OldClass` to be an old-style class.

What does this mean? Simply that the :func:`type()` of an instance
will not match the :func:`__class__()` method of the instance.
In code

.. testsetup:: *

  class OldClass:
    pass
  x = OldClass()

.. doctest::

  >>> x = OldClass()
  >>> type(x)
  <type 'instance'>
  >>> x.__class__  # doctest: +SKIP
  <class __main__.OldClass at 0x1228a80>
  >>> type(x) == x.__class__
  False

All instances of old-style classes have type *instance*.
Compare this with a new-style class::

  class NewClass(object):
    pass

Checking the type and class

.. testsetup:: *

  class NewClass(object):
    pass
  x = NewClass()

.. doctest::

  >>> x = NewClass()
  >>> type(x) # doctest: +SKIP
  <class '__main__.NewClass'>
  >>> x.__class__ # doctest: +SKIP
  <class '__main__.NewClass'>
  >>> type(x) == x.__class__
  True

There are some other, more obscure differences between old- and new-style classes, such as the way
they behave when using the :func:`__getattr__` and :func:`__getattribute__` methods.


Metaclasses
-----------

When a class is created, there is a built-in Python function that takes the class's code and creates
an actual class object that operates in the way we expect.  This built-in Python function is called
a metaclass, since it creates classes.

You can actually create your own metaclass.  This is primarily used to add additional functinoality
to certain classes before they are created, similar to the way that class decorators work.


Exercises
---------

#. Construct a new-style class that sublcasses :class:`list`.
   Add a method, :func:`double`, that creates another :class:`list`
   that contains two back-to-back copies of a list, i.e., ::
    
     >>> l = mylist(1,2,3)
     >>> l.double()
     [1, 2, 3, 1, 2, 3]


