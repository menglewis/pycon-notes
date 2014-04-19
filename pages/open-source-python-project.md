title: "Set your code free: releasing and maintaining an open-source Python project"

###Structure
    PyFly/
        docs/
        pyfly/
            __init__.py
        tests/
        LICENSE.txt
        MANIFEST.in
        README.rst
        setup.py

###Code Hosting
Use Github because that's where the people are.

###License
* The conditions for use
* No license means I can't use it
* If you aren't sure, use BSD or MIT
    * These are less restrictive
* GPL, Apache, MPL
    * More restrictive
* Don't use anything else
* Using a lesser known license causes people to be less sure if they can use their code without worrying about legal troubles

###Documentation
* If it's not documented, it doesn't exist
* Build it with Sphinx
* Host it at ReadTheDocs

Example:

    #!bash
    pip install sphinx
    ...
    cd docs/
    sphinx-quickstart
    ...

    make html

###Testing
tox saves the day for testing different versions
* Creates a bunch of virtualenvs
* Runs tests in all of those environments

###Continous Integration
* TravisCI
* Give a .travis.yml file to configure how to run tests and tell which versions to test on
* Every push to the repo will show if the build passed or failed

###Packaging
* setup.py
* use setuptools

MANIFEST.in
* Used to include non-Python files in MANIFEST.in
include AUTHORS.rst
include CHANGES.rst
include LICENSE.rst
...

###Before Release

Push tags to Github

Get GPG setup to sign git tags
    
    #!bash
    python setup.py sdist


Create account on PyPI and claim the name for Python project

    #!bash
    pip install twine
    twine upload ...tar.gz


Checking packaging.python.org

###Community
* Semantic Versioning
* X.Y.Z
* X for breaking changes
* Y for backwards-compatible feature additions
* Z for bug fixes
* Keep a changelog
* Have a Contributing Document
    * CONTRIBUTING.rst
    * How to get setup for development
    * How to run the tests
    * What to include in a bug report
    * Coding Standards, test coverage standards
* Keep the tests passing
* Give quick feedback
    * Even if it's a quick, I don't have time to look at this now, I'll take a look next week
* Give credit
* Give commit access to people who contribute a lot and who you trust
* Be nice
