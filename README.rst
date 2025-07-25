Decorators for Humans
=====================

The goal of the decorator module is to make it easy to define
signature-preserving function decorators and decorator factories.
It also includes an implementation of multiple dispatch and other niceties
(please check the docs). It is released under a two-clauses
BSD license, i.e. basically you can do whatever you want with it but I am not
responsible.

Installation
-------------

If you are lazy, just perform

 ``$ pip install decorator``

which will install just the module on your system.

If you prefer to install the full distribution from source, including
the documentation, clone the `GitHub repo`_ or download the tarball_, unpack it and run

 ``$ pip install .``

in the main directory, possibly as superuser.

.. _tarball: https://pypi.org/project/decorator/#files
.. _GitHub repo: https://github.com/micheles/decorator

Testing
--------

If you have the source code installation you can run the tests with

 `$ python tests/test.py -v`

or (if you have setuptools installed)

 `$ python setup.py test`

Notice that you may run into trouble if in your system there
is an older version of the decorator module; in such a case remove the
old version. It is safe even to copy the module `decorator.py` over
an existing one, since we kept backward-compatibility for a long time.

Repository
---------------

The project is hosted on GitHub. You can look at the source here:

 https://github.com/micheles/decorator

Documentation
---------------

The documentation has been moved to https://github.com/micheles/decorator/blob/master/docs/documentation.md

From there you can get a PDF version by simply using the print
functionality of your browser.

Here is the documentation for previous versions of the module:

https://github.com/micheles/decorator/blob/4.3.2/docs/tests.documentation.rst
https://github.com/micheles/decorator/blob/4.2.1/docs/tests.documentation.rst
https://github.com/micheles/decorator/blob/4.1.2/docs/tests.documentation.rst
https://github.com/micheles/decorator/blob/4.0.0/documentation.rst
https://github.com/micheles/decorator/blob/3.4.2/documentation.rst

For the impatient
-----------------

Here is an example of how to define a family of decorators tracing slow
operations:

.. code-block:: python

   from decorator import decorator

   @decorator
   def warn_slow(func, timelimit=60, *args, **kw):
       t0 = time.time()
       result = func(*args, **kw)
       dt = time.time() - t0
       if dt > timelimit:
           logging.warning('%s took %d seconds', func.__name__, dt)
       else:
           logging.info('%s took %d seconds', func.__name__, dt)
       return result

   @warn_slow  # warn if it takes more than 1 minute
   def preprocess_input_files(inputdir, tempdir):
       ...

   @warn_slow(timelimit=600)  # warn if it takes more than 10 minutes
   def run_calculation(tempdir, outdir):
       ...

Enjoy!
