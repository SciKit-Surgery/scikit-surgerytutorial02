.. highlight:: shell

.. _User_Interface:


Adding a User Interface
===============================================
Your library is sufficient as is, you have an implementation of an algorithm, which 
an interested user could download and use within their own Python application. However, 
it's nice to include a sample application, or some sort of user interface.  
A basic UI allows people to download and use your code directly and also see how 
your algorithm is meant to be used. The SciKit-Surgery Python template makes this
easy. 

Start by creating a new issue on GitHub, something like "Implement UI". And a new
git branch to match
::

   git checkout -b 2-implement-ui


We'll be modifying the code in the sksurgeryspherefitting/ui directory. 
Before we start, edit tests/pylintrc back to how it was, so our code gets properly tested.
::

   # Add files or directories to the blacklist. They should be base names, not
   # paths.
   ignore=CVS

Now edit sksurgeryspherefitting/ui/sksurgeryspherefitting_demo.py, so that 
it looks like:
::

  # coding=utf-8

  """Uses sphere fitting to fit to vtk model"""
  import vtk
  from sksurgeryvtk.models.vtk_surface_model import VTKSurfaceModel
  from sksurgeryspherefitting.algorithms import sphere_fitting

  def run_demo(model_file_name, output=""):
      """ Run the application """
      model = VTKSurfaceModel(model_file_name, [1., 0., 0.])
      x_values = model.get_points_as_numpy()[:, 0]
      y_values = model.get_points_as_numpy()[:, 1]
      z_values = model.get_points_as_numpy()[:, 2]

      initial_parameters = [0.0, 0.0, 0.0, 0.0]
      result = sphere_fitting.fit_sphere_least_squares(x_values,
                                                       y_values,
                                                       z_values,
                                                       initial_parameters)

      print(f"Result is {result}")

      if output != "":

          sphere = vtk.vtkSphereSource()
          sphere.SetCenter(result[0][0], result[0][1], result[0][2])
          sphere.SetRadius(result[0][3])
          sphere.SetThetaResolution(60)
          sphere.SetPhiResolution(60)

          writer = vtk.vtkXMLPolyDataWriter()
          writer.SetFileName(output)
          writer.SetInputData(sphere.GetOutput())
          sphere.Update()
          writer.Write()

And edit sksurgeryspherefitting/ui/sksurgeryspherefitting_command_line.py:
::

  # coding=utf-8

  """Command line processing"""


  import argparse
  from sksurgeryspherefitting import __version__
  from sksurgeryspherefitting.ui.sksurgeryspherefitting_demo import run_demo


  def main(args=None):
      """Entry point for scikit-surgery-sphere-fitting application"""

      parser = argparse.ArgumentParser(
          description='scikit-surgery-sphere-fitting')

      ## ADD POSITIONAL ARGUMENTS
      parser.add_argument("model",
                          type=str,
                          help="Filename for vtk surface model")

      # ADD OPTINAL ARGUMENTS
      parser.add_argument("-o", "--output",
                          required=False,
                          type=str,
                          default="",
                          help="Write the fitted sphere to file"
                          )

      version_string = __version__
      friendly_version_string = version_string if version_string else 'unknown'
      parser.add_argument(
          "--version",
          action='version',
          version='scikit-surgery-sphere-fitting version '
          + friendly_version_string
          )

      args = parser.parse_args(args)

      run_demo(args.model, args.output)

We should also add a unit test to make sure that the demo program works, so create a file 
tests/test_sksurgeryspherefitting_demo.py and cut and paste this:
::

  # coding=utf-8

  """scikit-surgery-sphere-fitting tests"""

  from sksurgeryspherefitting.ui.sksurgeryspherefitting_demo import run_demo

  def test_fit_sphere_least_sqs_demo():
    """
    test the run demo entry point
    """
    model_name = 'data/CT_Level_1.vtp'
    output_name = 'out_temp.vtp'

    run_demo(model_name, output_name)


Note that we need some testing data here. If you have a vtk surface file that you'd like to 
try fitting a sphere to you can subsitute it above. Other wise you can get one from `here`_
::

   mkdir data
   cd data
   wget https://github.com/thompson318/scikit-surgery-sphere-fitting/raw/master/data/CT_Level_1.vtp

Before you run again (e.g. `tox -r`), we need to tell tox about the extra dependencies we've just added
(`vtk`_, and `scikit-surgeryvtk`_)  so edit requirements.txt, which should now look like:
::

   numpy>=1.11
   scipy
   vtk<9.0.0
   scikit-surgeryvtk

You will need to add `vtk`_, and `scikit-surgeryvtk`_ in setup.py for the `install_requires`:
::
    install_requires=[
        'numpy>=1.11',
        'spicy',
        'vtk<9.0.0',
        'scikit-surgeryvtk'
    ],


Next we need to edit tests/pylintrc to help lint deal with python modules that use compiled libraries. 
Pylint can't see inside compiled libraries, so it needs help with "import vtk". So we add vtk to the 
"extension-pkg-whitelist" in pylintrc (line 35):
::

   extension-pkg-whitelist=numpy, vtk

If you run tox now (e.g. `tox -r`), you should get all unit tests passing, and 100% test coverage.
And if you're in the project parent directory you should be able to run
(using your roject's virtual environment `source .tox/py36/bin/activate`):
::

   python sksurgeryspherefitting data/CT_Level_1.vtp -o sphere.vtp

You'll see some output on the console
(e.g., `Result is (array([136.571217  , 151.97335771, -95.51789211,   8.11853981]), 2)`.
If you have a vtk viewer you can load both models and see what you've done.
Alternatively, you can use the online `vtk_GeometryViewer`_ where you will need to drop vtp files in the viewer.
Here's an example of a sphere fitted to a 3D ultrasound image of a fiducial sphere.

The original US data:

.. figure:: https://github.com/SciKit-Surgery/scikit-surgerytutorial02/raw/master/doc/sphere.gif

and with a fitted sphere

.. figure:: https://github.com/SciKit-Surgery/scikit-surgerytutorial02/raw/master/doc/fitted_sphere.gif

For github actions, you will need to amend `/.github/workflows/ci.yml` using only python version 3.7
::
    jobs:
      test:
        strategy:
          matrix:
            os: [ubuntu-latest, macos-latest, windows-latest]
            python-ver: [3.7]

Commit your changes and push to origin
::

   git add data/CT_level_1.vtp
   git add tests/pylintrc tests/test_sksurgeryspherefitting_demo.py
   git add sksurgeryspherefitting/ui/sk*.py
   git commit -m "added user interface (#2)"
   git push origin  2-implement-ui


.. _`here`: https://gihub.com/thompson318/scikit-surgery-sphere-fitting/blob/master/data/CT_Level_1.vtp
.. _`vtk`: https://pypi.org/project/vtk/
.. _`scikit-surgeryvtk`: https://pypi.org/project/scikit-surgeryvtk/
.. _`vtk_GeometryViewer`: https://kitware.github.io/vtk-js/examples/GeometryViewer/index.html
