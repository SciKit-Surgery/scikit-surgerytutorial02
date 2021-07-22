.. highlight:: shell

.. _Testing:

===============================================
Start Testing with Tox
===============================================
The SciKit-Surgery Python template comes preconfigured to perform functional tests on your code, to test that 
your code conforms to Python's coding style (`PEP 8`_), and produce documentation for your code. This is all
done with the helpful tox package, which is configured with the tox.ini file.

Navigate back to the project's parent directory.
::

   cd ../../

And try running tox
::

   tox

At this stage you may realise you don't have tox installed, if not, try
::

   pip install tox


If tox runs, you should get some test failures, accompanied by informative output to the console,
explaining what these errors are. Let's start at the top.
::

   ImportError while importing test module '/home/thompson/src/scikit-surgery-sphere-fitting/tests/test_sksurgeryspherefitting.py'.
   Hint: make sure your test modules/packages have valid Python names.
   Traceback:
   .tox/py36/lib/python3.6/site-packages/six.py:709: in exec_
    exec("""exec _code_ in _globs_, _locs_""")
   tests/test_sksurgeryspherefitting.py:5: in <module>
      from sksurgeryspherefitting.ui.sksurgeryspherefitting_demo import run_demo
   sksurgeryspherefitting/ui/sksurgeryspherefitting_demo.py:4: in <module>
      from sksurgeryspherefitting.algorithms import addition, multiplication
   E   ImportError: cannot import name addition

The first and last two lines are the most helpful, it all started in a file in the tests directory, and ended when 
it couldn't import the name "addition". That's because we deleted it and replaced it with sphere fitting. Let's 
go into the test directory,
::

   cd tests

And edit the test file 
::

   vi test_sksurgeryspherefitting.py

The first line imports from the user interface (ui). We'll cover this later in the tutorial, for now let's 
comment it out 
::

   #from sksurgeryspherefitting.ui.sksurgeryspherefitting_demo import run_demo

And comment out the first test, which is for the ui.
::

  #def test_using_pytest_sksurgeryspherefitting():
  #    x = 1
  #    y = 2
  #    verbose = False
  #    multiply = False
  #
  #    expected_answer = 3
  #    assert run_demo(x, y, multiply, verbose) == expected_answer

Then look at the second import statement, it asks to import addition and multiply from algorithms, but 
we removed them and replaced them with sphere_fitting.py, so let's update the import statement
::

   from sksurgeryspherefitting.algorithms import sphere_fitting

Also add an import numpy, so we can use it's approximately equal function.
::

  import numpy

Now scroll down and delete the two unit tests for addition and subtraction, replacing them 
with a test for fit_sphere_least_squares
::

  def test_fit_sphere_least_squares():
    x_centre = 1.0
    y_centre = 167.0
    z_centre = 200.0

    radius = 7.5

    #some arrays to fit data to
    x_values=numpy.ndarray(shape=(1000,),dtype=float )
    y_values=numpy.ndarray(shape=(1000,),dtype=float )
    z_values=numpy.ndarray(shape=(1000,),dtype=float )

    #fill the arrays with points uniformly spread on 
    #a sphere centred at x,y,z with radius radius
    for i in range(1000):
        #make a random vector
        x=numpy.random.uniform(-1.0, 1.0)
        y=numpy.random.uniform(-1.0, 1.0)
        z=numpy.random.uniform(-1.0, 1.0)

        #scale it to length radius
        length=numpy.sqrt( (x)**2 + (y)**2 + (z)**2 )
        factor = radius / length

        x_values[i] = x*factor + x_centre
        y_values[i] = y*factor + y_centre
        z_values[i] = z*factor + z_centre
       
    parameters = [0.0, 0.0, 0.0, 0.0]
    result = sphere_fitting.fit_sphere_least_squares (x_values, 
                                                      y_values, 
                                                      z_values, 
                                                      parameters)

    numpy.testing.assert_approx_equal(result[0][0], x_centre, significant=10)

We've used some functions from numpy, so don't forget to add import numpy at the top of the test file;
::

   import numpy
   
now try running tox again
::

   cd ../
   tox

you'll see that it fails, with 
::

   E   ImportError: No module named scipy.optimize

We need to tell tox that we need scipy to for this module. The file requirements.txt tells tox what 
module python needs for testing, so edit requirements and add scipy. numpy should already be there.
::

   numpy
   scipy

After changing requirements.txt you will need to rebuild tox's virtual environments, using
::

  tox -r

now try running tox again, you should see a bunch of output ending something like ...
::

   ______________________________________________________ summary ______________________________________________________
   py36: commands succeeded
   ERROR:   lint: commands failed

Which tells us that the functional unit tests worked, but that "lint" failed. 

.. _`PEP 8`: https://www.python.org/dev/peps/pep-0008/

