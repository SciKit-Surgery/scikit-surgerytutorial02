.. highlight:: shell

.. _Planning:

===============================================
Planning
===============================================
Thinking about what the library is supposed to do is a good way to 
start. The Python Template comes with a doc/requirements.rst file 
which is a good place to jot down initial thoughts on what the library
will or won't do. It already has some non specific requirements, 
let's add some more about what a sphere fitting algorithm should do.
Later on you could add links to unit tests to verify that the requirements
are met.

The list doesn't have to be exhaustive and will probably become obsolete, but 
it provides an opportunity to stop and think before coding. There is a 
substantial body of evidence that thinking before coding leads to better 
code.
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

  git add doc/requirements.rst
  git commit -m "Issue #1 added some functional requirements"
