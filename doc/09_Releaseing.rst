.. highlight:: shell

.. _Releasing:

===============================================
Getting Ready to Release 
===============================================
The easiest way to distribute your python module is by creating a python wheel, and
uploading it to the python package index. The creation of a wheel is controlled by 
the file setup.py. The SciKit-Surgery python template will have created a setup.py file when 
you created the module, but it will need updating. Open it and edit it. 
The most important thing to do is to update the 
"install_requires" field, to include scipy, vtk, and scikit-surgeryvtk
::

  # coding=utf-8
  """
  Setup for scikit-surgery-sphere-fitting
  """

  from setuptools import setup, find_packages
  import versioneer

  # Get the long description
  with open('README.rst') as f:
      long_description = f.read()

  setup(
      name='scikit-surgery-sphere-fitting',
      version=versioneer.get_version(),
      cmdclass=versioneer.get_cmdclass(),
      description='scikit-surgery-sphere-fitting implements a least squares sphere fitting algorithm, to read a vtk poly data file, a config file, and outputs the fitted sphere',
      long_description=long_description,
      long_description_content_type='text/x-rst',
      url='https://github.com/StephenThompson/sksurgeryspherefitting',
      author='Stephen Thompson',
      author_email='s.thompson@ucl.ac.uk',
      license='BSD-3 license',
      classifiers=[
          'Development Status :: 3 - Alpha',

          'Intended Audience :: Developers',
          'Intended Audience :: Healthcare Industry',
          'Intended Audience :: Information Technology',
          'Intended Audience :: Science/Research',


          'License :: OSI Approved :: BSD License',


          'Programming Language :: Python',
          'Programming Language :: Python :: 3',

          'Topic :: Scientific/Engineering :: Information Analysis',
          'Topic :: Scientific/Engineering :: Medical Science Apps.',
      ],

      keywords='medical imaging',

      packages=find_packages(
          exclude=[
              'doc',
              'tests',
              'data'
          ]
      ),

      install_requires=[
          'numpy>=1.11',
          scipy,
          vtk,
          scikit-surgeryvtk
      ],

      entry_points={
          'console_scripts': [
              'sksurgeryspherefitting=sksurgeryspherefitting.__main__:main',
          ],
      },
  )

The readme.rst file will function as the title page of your module. It should provide enough
information to make it clear what the module is intended to do and how to use it. The 
SciKit-Surgery python template will have created a readme.rst file, but this will need updating with 
information on your module. If you're looking for inspiration checkout out the 
readme for `scikit-surgery-sphere-fitting`_. 

Commit and push your changes.
::

   git add setup.py README.rst
   git commit -m "Issue #2 updated setup.py and readme"
   git checkout master
   git merge --no-ff 2-implement-ui
   git push origin master

Wait until the continuous integration tests have finished on WEISSLab. You should now be
able to visit your code on readthedocs or WEISSLab and see three green boxes, showing that 
your code is tested (with 100% coverage) and that the docs are building. To anyone 
considering using your code this would be very encouraging. 

.. _`scikit-surgery-sphere-fitting`: https://scikit-surgery-sphere-fitting.readthedocs.io/en/latest/?badge=latest
