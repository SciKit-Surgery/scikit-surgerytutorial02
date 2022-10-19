.. highlight:: shell

.. _Planning:

===============================================
Planning
===============================================
Read up on good practice for the development of scientific software.
`Ten Simple Rules for the Open Development of Scientific Software`_ provides
some helpful inspiration for new researchers/developers.
Rules 1 to 3 cover thinking carefully
about exactly what you want your software to do and how it will interface with
existing software to create a useful application.

`Test driven development`_ is a useful software development tool to ensure that
your code not only works but demonstrably performs the function(s) you intended.
Test driven development
begins with describing what you intend the software to do, a process known as defining
the
`software requirements`_. You should consider doing this for any software project,
but how thorough you are should depend on the intended end use of your software.
For a piece of quality controlled code to be used in a medical device it is essential.
For some research code to support your paper you could be more relaxed.

The Python Template comes with a doc/requirements.rst file
which is a good place to jot down initial thoughts on what the library
will or won't do. It already has some non specific requirements, 
let's add some more about what a sphere fitting algorithm should do.
Later on you could add links to unit tests to verify that the requirements
are met.

The list doesn't have to be exhaustive and will probably become obsolete, but 
it provides an opportunity to stop and think before coding.
::

  |    0003    |  Provides a function to fit a sphere to a list of      |                                     |
  |            |  3 dimensional points                                  |                                     |
  +------------+--------------------------------------------------------+-------------------------------------+
  |    0004    |  Provides a command line application                   |                                     |
  +------------+--------------------------------------------------------+-------------------------------------+
  |    0005    |  What else ??                                          |                                     |  
  +------------+--------------------------------------------------------+-------------------------------------+

When you've finished editing doc/requirements.rst, don't forget to update it on gitlab.
::

  git add docs/requirements.rst
  git commit -m "added some functional requirements (#1)"

.. _`Ten Simple Rules for the Open Development of Scientific Software`: https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1002802
.. _`software requirements`: http://andtr.com/the-importance-of-software-requirements
.. _`Test driven development`: https://en.wikipedia.org/wiki/Test-driven_development
