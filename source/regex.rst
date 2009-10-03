Regular Expressions
===================

Regular expressions are supported in Python through the ``re`` package.

There are four basic operations done with regular expression in
Python:

1. Compiling using ``re.compile``.  This creates a reusable regular
   expression pattern that can be used with other functions.
   If the same expression is used multiple times, use
   of compiled expressions can speed up performance and simplify
   code.
2. Matching using ``re.match``.  This tells you whether or not a
   string matches a given pattern.
3. Searching using ``re.search``.  This tells you whether or not
   a given string contains the given pattern anywhere in it,
   and tells you where it is.
4. Replacing using ``re.sub``.  This allows you to find a pattern
   in a string and replace the first occurrence with 
   replacement text.

Using these functions, you can perform the majority of standard
regular expression functionality with a cleaner syntax, if a bit
less compact than many other scripting languages (also, a bit less
confusing at times).



