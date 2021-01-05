Packaging Guide
===============

Introduction
------------

This repository holds some resources for creating a Python package. If you feel like
something is missing, please file and issue or a PR.

Every section in this document deals with one aspect of a Python package like code,
documentation or continuous integration. Each section provides a list of steps which
describe an aspect of the section. There are steps which are highly recommended and
others which are optional and may suit only particular projects. Optional steps hold
"(optional)" in their title.


.. contents::


General Considerations
----------------------

Before you start to write a package, you should ask yourself some questions.


What is a package?
~~~~~~~~~~~~~~~~~~

A package is an additional way compared to modules to structure code. Modules contain
functions, classes or other objects. To separate logically distinct groups of objects
from one another, you might create multiple modules. The modules are then grouped as a
package.

The more general idea behind this pattern is called modular programming. Modularizing
code has several advantages.

*Under development.*


Do I need a package?
~~~~~~~~~~~~~~~~~~~~

If any of the following statements is true, you probably do not need a package.

*Under development.*


Designing a package
~~~~~~~~~~~~~~~~~~~

*Under development.*

- Purpose and scope
- Audience
- Interface


How to choose dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~~

Dependencies help you to focus on what you really want to achieve. But, having more of
them is not strictly preferred. With each dependency your code becomes dependent on
someone else's work. The API might change and you have to adjust your program or, the
worst case, the project becomes unmaintained. As a general advice, use packages ...

- which are mature and frequently used in the Python ecosystem (NumPy, pandas, etc.).
  They likely evolve and will never cease to exist.
- which do one thing very well instead of multiple things. This allows to easily replace
  them in case they become unmaintained.

Having talked about the potential downsides of dependencies, they can also introduce
positive externalities. Packages which are backed by a lot of people will become better
over time and, as a consequence, your package will become better as well. Exposing your
package to this evolution is part of a good design.


Resources
~~~~~~~~~

- `Real Python - Python Modules and Packages
  <https://realpython.com/python-modules-packages>`_


Writing code
------------

- https://blog.ionelmc.ro/2014/05/25/python-packaging
- https://hynek.me/articles/document-your-tests/
- https://jml.io/pages/test-docstrings.html
- https://github.com/amontalenti/elements-of-python-style


Code Quality
------------

pre-commit
~~~~~~~~~~

`pre-commit <https://pre-commit.com>`_ provides pre-commit-hooks which are checks which
run before every commit. These checks are very useful to run linter, formatter and other
static analysis over your repository. Copy the `.pre-commit-config.yaml
<.pre-commit-config.yaml>`_ to your repository. Install ``pre-commit`` with

.. code-block:: bash

    $ conda install pre-commit -y
    $ pre-commit install

Now, the hooks are executed before every commit. To run the checks manually, type

.. code-block:: bash

    $ pre-commit run -a

From time to time, you have to update your pre-commit-hooks. Type

.. code-block:: bash

    $ pre-commit autoupdate

A full list of projects which maintain a pre-commit configuration is `here
<https://pre-commit.com/hooks.html>`_. But, it also always possible to configure a
`local hook <https://pre-commit.com/#repository-local-hooks>`_ in case the desired tool
does not support pre-commit upstream.


*Resources*

- `Official website of pre-commit <https://pre-commit.com/>`_.
- Some easily available and standard `pre-commit-hooks <https://github.com/pre-commit/
  pre-commit-hooks>`_.


codecov (optional)
------------------

`codecov <https://codecov.io/>`_ allows to measure coverage of your test suites meaning
which parts of the code are visited while executing the tests. Coverage as a measure is
helpful to identify the blind spots of your test suite, but it does not ensure
correctness.

codecov can be configured via a `codecov.yaml` as explained `here
<https://docs.codecov.io/docs/codecov-yaml>`_.

The metric can be easily gamed by tests which use high-level interfaces and simulate
what a typical user might do. Thus, it is helpful to calculate coverage for different
parts of your test suite, for example unit, integration and end-to-end tests.

For an example, see `ci-with-codecov-and-separation-of-tests.yaml
<.github/ci-with-codecov-and-separation-of-tests.yaml>`_.


Distribution
------------

Distribute via conda
~~~~~~~~~~~~~~~~~~~~

The preferred way to distribute scientific packages is via ``conda`` because it is
provided with the Anaconda distribution for scientific programming which is the
entry-point for many newcomers. Additionally, many packages like NumPy are not easily
installed and compiled on Windows, but you can install pre-compiled packages with
``conda``.

We start by specifying a ``setup.py`` in the root directory of the repository. You
should provide at least the minimum of information defined below.

.. code-block:: python

    # Content of setup.py.

    from setuptools import find_packages
    from setuptools import setup

    setup(
        name="package_name",
        version="0.0.1",
        author="Jane Doe",
        author_email="janedoe@email.com"
        packages=find_packages(),
    )

The next step is to specify the ``meta.yaml`` which is the package meta data file for
``conda``. Copy the folder `.conda <.conda>`_ over to your repository. Leave the build
scripts untouched and fill the ``meta.yaml`` with your package requirements and
additional information under ``about:``. Here is the `manual <https://docs.conda.io/
projects/ conda-build/en/latest/resources/define-metadata.html>`_ for defining the
``meta.yaml``.

To build and upload your package (`link <https://docs.anaconda.com/anaconda-cloud/
user-guide/tasks/work-with-packages/>`_), install two packages with

.. code-block:: bash

    $ conda install anaconda-client conda-build -y

To test whether you can actually build your package, run

.. code-block:: bash

    $ conda-build .

To upload a package, log into you anaconda account with

.. code-block:: bash

    $ anaconda login

Then, copy the path displayed in the end of the building process which contains the
package name and ends with ``*.tar.bz2`` and type

.. code-block:: bash

    $ anaconda upload <path>

You can take a shortcut if you allow for automatic uploads and specify the user while
building the package with

.. code-block:: bash

    $ conda config --set anaconda_upload yes
    $ conda-build . --user <user-name>


Documentation
-------------

Set up a documentation with sphinx
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A documentation is necessary to provide an explanation for the

1. Install sphinx with

   .. code-block:: bash

       $ conda install sphinx

2. Create a in the project root which should contain the documentation. Usually, it is
   called ``docs``.

3. Step into ``docs`` and execute

   .. code-block:: bash

       $ sphinx-quickstart

4. Answer the prompts and stick to their defaults if you are not sure.


Host your documentation on readthedocs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Under development.*

x. Create a configuration file in the root of your repository. You can simply copy
   `.readthedocs.yaml <.readthedocs.yaml>`_.

x. The configuration file also declares a conda environment file
   `docs/rtd_environment.yml <docs/rtd_environment.yml>`_  which is used to specify all
   dependencies of the documentation on readthedocs.org. Add only necessary packages as
   the memory is limited on workers which build your documentation on readthedocs.


Document your project with ``nbsphinx`` (optional)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To write sections of your documentation in Jupyter notebooks, use `nbsphinx
<https://nbsphinx.readthedocs.io/>`_.

1. Follow the instructions in the quickstart `quickstart guide
   <https://nbsphinx.readthedocs.io/en/0.7.0/>`_, but install ``nbsphinx`` via ``conda``
   which ensures that you also install ``pandoc``. Use

   .. code-block:: bash

       conda install -c conda-forge nbsphinx

2. If you host your documentation on readthedocs, you also need to ensure that
   ``nbsphinx`` is installed as well. Add it to your conda environment for the
   documentation.


Automatically built your documentation with every pull request
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can build the documentation for each commit to a pull request as an additional check
to the other CI checks. To enable this feature, follow this `guide
<https://docs.readthedocs.io/en/stable/guides/autobuild-docs-for-pull-requests.html>`_.


Continuous Integration
----------------------

Continuous integration (CI) is a process where changes made by developers are
automatically checked for compatibility with the main code. Compatibility checks are,
for example, source code tests and coverage reports.


Github Actions
~~~~~~~~~~~~~~

`Github Actions <https://help.github.com/en/actions>`_ is the CI service provided by
Github and since it has a very generous free tier and is a part of your repository by
default, we recommend it.

(We started using Travis-CI in combination with Appveyor for Windows support, moved to
Azure Pipelines, and now use Github Actions solely.)

To set up continuous integration for your repository, you only have to define workflows
in the folder `.github/workflows`. This repository has some exemplary workflows in
`.github/workflows/ <.github/workflows/>`_ which you almost only need to copy over to
your repository.

- (recommended) `ci.yaml <.github/workflows/ci.yaml>`_ is a simple workflow which runs
  your Python test suite against multiple Python versions and on Linux, MacOS and
  Windows. It assumes that ...

  1. your repository has a `environment.yml` in the root folder which contains all
     necessary packages.
  2. your documentation lies under `docs/`.
  3. you use `pre-commit`.

- `ci-with-codecov.yaml <.github/workflows/ci-with-codecov.yaml>`_ extends `ci.yaml` by
  uploading your test coverage reports to codecov.


Resources
~~~~~~~~~

- `List of CI services <https://github.com/ligurio/awesome-ci>`_.


Repository management
---------------------

Protecting branches
~~~~~~~~~~~~~~~~~~~

Require that checks like the continuous integration workflow have to pass before a pull
request can be merged and that only certain people are allowed to merge pull requests.
Everything about this topic can be found `here <https://help.github.com/en/github/
administering-a-repository/about-protected-branches#branch-protection-settings>`_.


Issue and Pull Request templates
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Issue and pull request templates can help standardize the information provided by users
and developers.

Open Source Economics has `organizational level issue and PR templates <https://github.
com/OpenSourceEconomics/.github/tree/main/.github>`_. Additionally, each repository can
override it by adding their own templates.

- `Github guide <https://help.github.com/en/github/building-a-strong-community/
  about-issue-and-pull-request-templates>`_
- `estimagic's issue and PR templates
  <https://github.com/OpenSourceEconomics/estimagic/tree/master/.github>`_
- `respy's issue and PR templates
  <https://github.com/OpenSourceEconomics/respy/tree/master/.github>`_
- `econsa's issue and PR templates
  <https://github.com/OpenSourceEconomics/econsa/tree/master/.github>`_


Make your code citable with Zenodo
----------------------------------

`Zenodo <https://zenodo.org/>`_ is a handy tool to create DOI across released versions
of your package. You can follow `this guide written by GitHub <https://guides.github.
com/activities/citable-code/>`_ to get started. Note that two requirements need to be
met in order to use Zenodo:

- The target repository should be public, and
- There should be at least 1 release.


How to maintain this guide
--------------------------

- Update pre-commit-hooks from time to time with ``pre-commit autoupdate``.
