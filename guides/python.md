# Python 

Python is becoming the dominant programming language in astronomy and
much of the supported software for the institute is currently written
or being developed in python.  As such, this style guide is meant to
give recommendations for how to develop in Python to produce code that
is easy to read, to install, and to share.  We strongly recommend that
new Python projects follow the recommendations in this guide.

The style guide is based on the [Python Style Guideline][1] (PEP8),
the [Astropy Coding Guidelines][2], [JWST Quick Look Style Guide][5],
and the [LSST Style Guide][3]. If in doubt, err on the side of making
the code more readable.  If really in doubt, we suggest following the
Zen of Python which you can read by opeinging up Python and typing ``
import this``.

## Version

Python 3 is the recommended version of Python, and when possible, we
strongly recommend developing in the [latest version of
Python](https://github.com/spacetelescope/style-guides/issues/13).

## Style

The recommended standard for code will be the [PEP8 Style Guide for
Python Code][1].  Note that the first section is 'A Foolish
Consistency is the Hobgoblin of Little Minds' - these are not
absolutes, but rather guidelines that you should follow unless you
have a thought-out reason that makes your code more readable.

If it is consistent with the rest of the package, the following
exceptions are allowed:

* Lines may be longer than 80 characters, but it is recommended that comments and docstrings remain within the 80 character limit.

## Naming

The names of objects should provide a description of what is the
purpose of that object.  Following [PEP8][1], we recommend the
following naming conventions:

* Variables and function names should be lowercase with words
separated by an underscore. They should be descriptive.  Example:
`number_of_galaxies` and `calculate_mangitude`

* Constants and global variable names should be all upper case separated
by an underscore.  Example: `SPEED_OF_LIGHT`

* Class names should follow the CapWords, or CamelCase,
convention. Example: `Stars` and `SpaceTelescope`.

* Package and module names should be all lowercase.  can be used in
the module name if it improves readability. Example: 'jwst'

Additional naming conventions should follow the [PEP8][1] standard.

## Coding Conventions

The following conventions are recommended for use in code developed at
STScI.  Some of these conventions provide a consistent style while
others are best practices to help minimize possible bugs.

* Imports are always put at the top of the file, just after any module
comments and docstrings, and before module globals and constants.

* Using `from packagename import *` should be avoided. 

* The `import numpy as np`, `import matplotlib as mpl`, and `import
  matplotlib.pyplot as plt` naming conventions should be used wherever
  relevant.

 
## Documentation

Documentation is a key part to any package and helps guide the
developer and the user.  These recommendations are meant to provide a
consistent format for the documentation to help ease reading,
building, maintaining, and sharing the documentation.  Packages should
have automatically build documentation including narrative
documentation and Application Programming Interfaces (API).  By
following this recommendations, these documents can be built
automatically and hosted automatically on servcies like
[ReadTheDocs](https://readthedocs.org/) or internal STScI services.


### Comments

Inline comments are encouraged to provide greater context and
explanation of the code.  They should follow the [PEP8
comments](https://www.python.org/dev/peps/pep-0008/#comments)
standards.  Please keep in mind the following:

* Always make it a priority to keep the comments up to date with the
code!  This could include inline comments, docstrings, or package
documentation.

* Inline comments should always be full sentences.

* Inline comments should not replace documentation - if it's getting complicated enough to need several lines, it should probably be in the docstrings

### Docstrings

Docstrings are strongly recommended to follow the
[numpydoc](https://numpydoc.readthedocs.io/en/latest/format.html)
convention.  All functions and classes should have a docstring that
explicitly states what it does, what the input parameters are, and
what it returns.


### Narrative Documentation

Package documnentation should be built using
[Sphinx](http://www.sphinx-doc.org/en/master/index.html) and
recommended to be in [Restructured
Text](http://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#explicit-markup)

Notebooks can be used as part of the documentation.  These can 
be included in the automatically built documentation. 


## Testing Conventions

Testing is critical to making sure the software works and that it
continues to work when the code or dependencies are updated.  The
following is specific to STScI python packages but general
recommendations for the testing infrastructure will be described
elsewhere along with suggested workflows.

[Pytest](https://docs.pytest.org/en/latest/) is recommended to be used
for testing.

### Unit tests

Unit test should be included directly with the code and test low level
functionality.   The inclusion of unit tests is strongly recommended. 

The suggested layout for the unit tests would be in a directory with
the code under a directory named tests.  The following would be an
example of where the unit tests should sit in the package:
`jwst/jwst/assign_wcs/tests`.

Large data sets should be avoided for unit tests.

### Regression tests

Regression tests provide higher-level coverage of workflows using
known good data.  While recommended, they may not always be
appropriate for every package.

Regression tests should be in their own top level directory but with a
structure that follows the same layout as the code,
e.g. `jwst/tests/assign_wcs/`


## Package Conventions

Package conventions can make it easier to deploy and maintain
resources.  The following conventions are recommended to make it
easier to set up the testing, documentation, and workflow.

* All STScI packages should follow the [Github Repositories
  Guide](https://github.com/spacetelescope/style-guides/blob/master/guides/github-repositories.md)

Recommended template packages: 
* [astropy template package](https://github.com/astropy/package-template) with STSci specific changes
* [STScI Template Package](https://github.com/spacetelescope/stsci-package-template)


## Further Reading

The following sources provide more information and examples of other
style guides:

* [Python Style Guideline][1] (PEP8)
* [Python Doc String Guidelines][4]
* [Astropy Coding Guidelines][2]
* [JWST Quick Look Style Guide][5]
* [LSST Style Guide][3].

<!--
References
-->


[1]: https://www.python.org/dev/peps/pep-0008/?

[2]: http://docs.astropy.org/en/stable/development/codeguide.html 

[3]: https://developer.lsst.io/python/style.html   

[4]: https://www.python.org/dev/peps/pep-0257/

[5]: https://github.com/spacetelescope/jwql/blob/master/style_guide/style_guide.md
