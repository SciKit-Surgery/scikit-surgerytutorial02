.. highlight:: shell

.. _Start_Coding:

===============================================
Start Coding
===============================================

The python template is structured with a parent directory that contains various readme, licence files, together with 
three Python scripts. For the moment we don't want to modify any of these. The code we will begin with is to be found
in the subdirectory sksurgeryspherefitting
::
  cd sksurgeryspherefitting/

Within this directory are three more python scripts, __init__.py, __main__.py, and _version.py. None of these
need to be modified. The actual working code is within two subdirectories, algorithms and ui(user interface). More
complex libraries may contain more subdirectories, or less if they have no user interface. Despite the temptation 
to get the user interface working quickly, it is always good practice to start with the algorithms (`See page 44 of this`_).
::
   cd algorithms/

The SNAPPY Python template has already populated this with a couple of example algorithms, addition and multiplication.
These are nice examples, but a bit too simple for our tutorial. So lets delete them and create our own file, 
sphere_fitting.py. I chose sphere fitting as it was useful for me at the time, feel free to insert your own 
algorithm at this point.
The following code uses vim to edit the files, you can use whatever editor you like.
::
   rm addition.py mutliplication.py
   vi sphere_fitting.py


Copy and paste the following into your editor.
::
  # coding=utf-8
  """ Module for fitting a sphere to a list of 3D points """

  #scipy has a nice least squares optimisor
  from scipy.optimize import leastsq
  import numpy

  def fit_sphere_least_squares(x_values, y_values, z_values, initial_parameters):
      """
      Uses scipy's least squares optimisor to fit a sphere to a set
      of 3D Points

      :return: x: an array containing the four fitted parameters
      :return: ier: int An integer flag. If it is equal to 1, 2, 3 or 4, the
               solution was found.
      :param: (x,y,z) three arrays of equal length containing the x, y, and z
              coordinates.
      :param: an array containing four initial values (centre, and radius)
      """
      return leastsq(_calculate_residual_sphere, initial_parameters,
                     args=(x_values, y_values, z_values))

  def _calculate_residual_sphere(parameters, x_values, y_values , z_values):
      """
      Calculates the residual error for an x,y,z coordinates, fitted
      to a sphere with centre and radius defined by the parameters tuple

      :return: The residual error
      :param: A tuple of the parameters to be optimised, should contain
              [x_centre, y_centre, z_centre, radius]
      :param: arrays containing the x,y, and z coordinates.

      """
      #extract the parameters
      x_centre, y_centre, z_centre, radius = parameters

      #use numpy's sqrt function here, which works by element on arrays
      distance_from_centre = numpy.sqrt((x_values - x_centre)**2 + 
                                        (y_values - y_centre)**2 +
                                        (z_values - z_centre)**2)
      return distance_from_centre - radius

Note that there are two functions, the first fit_sphere_least_squares is what we expect the user to call.
Once we publish this module, anyone should be able to download your library and and fit a sphere to their points 
by calling this
function with the appropriate parameters. We have added a "docstring" under the function definition to tell the user
what parameters are required and what the function's return value will be. If you want people to use your code this 
is important. 

The second function, _calculate_residual_sphere is used by SciPy's least square optimiser to fit the model, in this 
case a sphere, though you could rewrite it to fit any geometry you like. This function is prefixed with an underscore (_)
which tells the user that it's not meant to be called directly. It's not strictly necessary to document this function, 
but is good practice. 

That's it, you've written a sphere fitting algorithm using the Python template. Commit your changes;
::
   git rm addition.py mutliplication.py
   git add sphere_fiting.py
   git commit -m "Issue #1 implemted the sphere fitting algorithm"
   git push origin 1-get-working

Make sure you include the hashtag #1 in your commit message, so that WEISSlab can link the change to 
the issue you created earlier.

Now anyone with access to your git repository can download and use your algorithm. However they're a lot more 
likely to do that if they can see that your algorithm does what it's supposed to do. This is where the Python
template starts being really helpful. 

.. _`See page 44 of this`: https://magazines-static.raspberrypi.org/issues/full_pdfs/000/000/030/original/HelloWorld07.pdf#page=44
