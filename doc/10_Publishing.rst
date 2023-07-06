.. highlight:: shell

.. _Publishing:

===============================================
Publishing 
===============================================

Finally, if you're mostly happy with your project you can add to the Python package index, 
where it will be easy for anyone to find and use your library. The Python 
Template provides code at the end of ci.yml to deploy your library when you create a tag with git.

::
  deploy:
    runs-on: ubuntu-22.04
    needs: test
    steps:
      - uses: actions/checkout@v3
      - uses: actions/checkout@master
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: 3.8

      - name: Install dependencies
        run: python -m pip install wheel twine setuptools

      - name: Build wheel
        run: |
          python setup.py sdist bdist_wheel

      - name: Publish package if tagged release
        if: github.event_name == 'push' && startsWith(github.event.ref, 'refs/tags')
        uses: pypa/gh-action-pypi-publish@master
        with:
          user: __token__
          password: ${{ secrets.PYPI_API_TOKEN }}

Now tag a release:
::

   git tag -a v0.0.1 -m "First release"
   git push origin v0.0.1

You might like to add some release cycle versions (schedules for alpha, beta, RC and full bugfix support).
See how `python versions`_ are done.

When you visit GitHub there should now be a manual build stage called "deploy". You can
trigger this manually and deploy your code to PyPi. To do this you will need an account on PyPi and to add
PYPI_API_TOKEN and $PYPI_PAS as variables in your GitHub project `TEST_PYPI_API_TOKEN`_.

.. _`scikit-surgery-sphere-fitting`: https://scikit-surgery-sphere-fitting.readthedocs.io/en/latest/?badge=latest
.. _`python versions`: https://peps.python.org/pep-0602
.. _`TEST_PYPI_API_TOKEN`: https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/#saving-credentials-on-github