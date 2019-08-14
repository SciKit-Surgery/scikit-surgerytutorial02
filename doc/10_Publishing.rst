.. highlight:: shell

.. _Publishing:

===============================================
Publishing 
===============================================

Finally, if you're mostly happy with your project you can add to the Python package index, 
where it will be easy for anyone to find and use your library. The Python 
Template provides code at the end of .gitlab-ci.yml to deploy your library when 
you create a tag with git. 
::
   deploy pip to PyPI:
    stage: deploy
    when: manual
    only:
      - tags

    environment:
      name: PyPI
      url: https://pypi.python.org/pypi/scikit-surgery-sphere-fitting

    tags:
      - pip-production

    artifacts:
      paths:
        - dist/

    script:
      # Install packages required to build/publish
      # remove any previous distribution files
      - pip install wheel twine setuptools
      - rm -rf dist

      # bundle installer
      - python setup.py bdist_wheel

      # Upload to pypi
      - twine upload --repository pypi dist/* --username $PYPI_USER --password $PYPI_PASS

You should probably change this to the test.pypi index before you try this for the first time, so change it to 
::
   deploy pip to PyPI:
    stage: deploy
    when: manual
    only:
      - tags

    environment:
      name: PyPI
      url: https://test.pypi.python.org/pypi/scikit-surgery-sphere-fitting

    tags:
      - pip-production

    artifacts:
      paths:
        - dist/

    script:
      # Install packages required to build/publish
      # remove any previous distribution files
      - pip install wheel twine setuptools
      - rm -rf dist

      # bundle installer
      - python setup.py bdist_wheel

      # Upload to pypi
      - twine upload --repository test.pypi dist/* --username $PYPI_USER --password $PYPI_PASS

Now tag a release:
::
   git tag -a v0.0.1 -m "First release"
   git push origin v0.0.1

When you visit WEISSLab there should now be a manual build stage called "deploy pip to PyPi". You can
trigger this manually and deploy your code to PyPi. To do this you will need an account on PyPi and to add
$PYPI_USER and $PYPI_PAS as variables in your WEISSLab project. 


.. _`scikit-surgery-sphere-fitting`: https://scikit-surgery-sphere-fitting.readthedocs.io/en/latest/?badge=latest
