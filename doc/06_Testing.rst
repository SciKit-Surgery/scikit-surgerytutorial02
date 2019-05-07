.. highlight:: shell

.. _Testing:

===============================================
Start Testing with Tox
===============================================
The SNAPPY Python template comes preconfigured to perform functional tests on your code, to test that 
your code conforms to Python's coding style (`PEP 8`_), and produce documentation for your code. This is all
done with the helpful tox package, which is configured with the tox.ini file.

Navigate back to the project's parent directory.
::
   cd ../../

And try running tox
::
   tox

At this stage you may realise you don't have tox installed, if not try
::
   pip install tox


If tox runs, you should get lots of test failures, and a nice big chunk of output to the console,
explaining what these errors are. Let's start at the top.
::
   ImportError while importing test module '/home/thompson/src/scikit-surgery-sphere-fitting/tests/test_sksurgeryspherefitting.py'.
   Hint: make sure your test modules/packages have valid Python names.
   Traceback:
   .tox/py27/lib/python2.7/site-packages/six.py:709: in exec_
    exec("""exec _code_ in _globs_, _locs_""")
   tests/test_sksurgeryspherefitting.py:5: in <module>
      from sksurgeryspherefitting.ui.sksurgeryspherefitting_demo import run_demo
   sksurgeryspherefitting/ui/sksurgeryspherefitting_demo.py:4: in <module>
      from sksurgeryspherefitting.algorithms import addition, multiplication
   E   ImportError: cannot import name addition

The first and last two lines are the most helpful, it all started in a file in the tests directory, and ended when 
it couldn't import the name addition. That's because we deleted it and replaced it with sphere fitting. Let's 
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
we removed them and replaced them with sphere_fitting.py, so let update the import statement
::
   from sksurgeryspherefitting.algorithms import sphere_fitting

Now scroll down and delete the two unit tests for addition and subtraction, replacing them 
with a test for FitSphere_LeastSquares

::
   def test_FitSphere_LeastSquares():

    x_centre = 1.0
    y_centre = 167.0
    z_centre = 200.0

    radius = 7.5

    #some arrays to fit data to
    xs=numpy.ndarray(shape=(1000,),dtype=float )
    ys=numpy.ndarray(shape=(1000,),dtype=float )
    zs=numpy.ndarray(shape=(1000,),dtype=float )

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

        xs[i] = x*factor + x_centre
        ys[i] = y*factor + y_centre
        zs[i] = z*factor + z_centre
       
    parameter = [0.0, 0.0, 0.0, 0.0]
    result = sphere_fitting.FitSphere_LeastSquares (xs, ys, zs, parameters)
    assert result[0][0] == x_centre

We've used some functions from numpy, so don't forget to add import numpy at the top of the file;
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
module python needs for testing, so edit requirements and add scipy to numpy, which should already be 
there
::
   numpy
   scipy

now try running tox again, you should see a bunch of output ending something like ...
::
   ______________________________________________________ summary ______________________________________________________
   py27: commands succeeded
   py36: commands succeeded
   ERROR:   lint: commands failed

Which tells that the functional unit tests worked, but that "lint" failed. 

What is Lint

~~~~~~~~~~~~
The SNAPPY python template used lint to check that the code is well written, according to Python's PEP 8 
coding standard. At times this may seem unnecessary, as long as the code runs who cares whether it's 
tidily written? However, the aim of the SNAPPY Python template is to help create code that not only works for 
you, but will be downloaded by others, modified and spread about. That's a lot more likely to happen if your code
can be easily understood by others, and that's what lint helps you do. 

If we scroll back up through the test output we'll start finding some linting errors. The first of them
looks like this;
::
   lint runtests: commands[0] | pylint --rcfile=tests/pylintrc sksurgeryspherefitting
   ************* Module sksurgeryspherefitting.ui.sksurgeryspherefitting_command_line
   sksurgeryspherefitting/ui/sksurgeryspherefitting_command_line.py:14:0: C0301: Line too long (81/80) (line-too-long)

It's telling us that one of the lines is tool long in a file in the ui directory. We haven't started on the user 
interface yet, so let's just tell lint to ignore that directory for the moment. 
Edit the file pylintrc, which is in the tests directory. Near the top of the file is an entry called 
ignore, add "ui" to the list of things to ignore;
::
   # Add files or directories to the blacklist. They should be base names, not
   # paths.
   ignore=CVS, ui

Now try running tox again. If you've cut and pasted the code from earlier, you should get 
one linting error;
::
   sksurgeryspherefitting/algorithms/sphere_fitting.py:23:62: C0326: No space allowed before comma
   def _calculate_residual_sphere(parameters, x_values, y_values , z_values):
                                                                ^ (bad-whitespace)

As you can see, it's a minor error that effects readability.  edit sphere_fitting.py to 
fix it and rerun tox. You should now get:
::
   ______________________________________________________ summary ______________________________________________________
   py27: commands succeeded
   py36: commands succeeded
   lint: commands succeeded
   congratulations :)

If not, read the output and fix any remaining errors. If so, commit your changes and push to origin;
::
   git add requirments.txt sksurgeryspherefitting/algorithms/sphere_fiting.py tests/pylintrc
   git commit -m "Issue #1 implemted unit test and fixed style errors"
   git push origin 1-get-working

If you wait a few minutes and visit weisslab, you should be able to see your library commit passing 
with a nice green tick. Congratulations, you have mastered testing and continuous integration testing. 

.. image:: passing_weisslab.png
   :height: 400px
   :alt: Your commit passing on weisslab
   :align: center

Your code is working now. So lets draw a line under it. Use git to merge your branch back to master, 
and push it to the origin. Then close the issue on Weisslab.
::
   git checkout master
   git merge --no-ff 1-get-working
   git push origin master
   git branch --delete 1-get-working

Go to the WEISSlab website, and close issue 1.

.. _`PEP 8`: https://www.python.org/dev/peps/pep-0008/
