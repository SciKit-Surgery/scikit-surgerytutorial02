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
      description='scikit-surgery-sphere-fitting implements a 
                    least squares sphere fitting algorithm, 
                    to read a vtk poly data file, a config file, and 
                    outputs the fitted sphere',
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
          'spicy',
          'vtk>=9.2.6',
          'scikit-surgeryvtk>=2.0.1'
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
`README.rst`_ for `scikit-surgerybard`_.

Commit, push your changes and delete local merged branches.
Please referee to this `git cheat sheet`_ for other git commands.
::

   git add setup.py README.rst
   git commit -m "updated setup.py and readme (#2)"
   git checkout main
   git push origin main
   git branch --delete 1-testing-tox-lint-pep8
   git branch --delete 2-implement-ui

Wait until the continuous integration tests have finished on GitHub. You should now be
able to visit your code on readthedocs and see three green boxes, showing that
your code is tested (with 100% coverage) and that the docs are building. To anyone 
considering using your code this would be very encouraging. 

.. _`README.rst`: https://github.com/SciKit-Surgery/scikit-surgerybard/blob/master/README.rst
.. _`scikit-surgerybard`: https://scikit-surgerybard.readthedocs.io/en/latest/
.. _`git cheat sheet`: https://education.github.com/git-cheat-sheet-education.pdf