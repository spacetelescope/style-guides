# Software Versioning and Dependencies

## Introduction

This style guide provides suggestions for software versioning for STScI
software, and the closely related topic of expectations for dependencies. While
some of these rules are of general use, the implementation details usually
depend on particular programming languages.  Hence this guide is subdivided into
a short set of general guidelines, and more detailed suggestions for specific
languages.

This document is meant as a guide to provide implementation suggestions and best
practices, but there *is* a formal
[STScI policy on software versions]( https://innerspace.stsci.edu/display/isec/Software+Language+Supported+Versions)
that should be followed for operation software. At the time of this writing,
following this guide should ensure compliance with this policy.

## General

### Software we build

When reasonable, software should follow the principles of
["Semantic Versioning"](https://semver.org/).  See the linked page for more
details, but the idea here is simple: version numbers should be
MAJOR.MINOR.PATCH, where MAJOR is only updated for backwards-incompatible
changes, MINOR for backwards-compatible changes with new features, or PATCH
for changes without new features (e.g., bugfixes or documentation improvements).
While the definition of "backwards-compatible" or "new feature" can be ambiguous
at times, following this rule of thumb will usually lead to less surprise (and
anger) from users or dependent developers.

The most common exception to this is early prototype or "preview" software,
where backwards-compatibility is neither expected nor desirable. That case is
best dealt with by using "0.y.z", incrementing z following the "PATCH" rule,
and "y" for any changes with either backwards-incompatibility *or* new features.

The expectation is that in-development versions typically have text after the
version number indicating their status. E.g., `v0.3.1beta` or `v.1.2.3dev`. It
is important that these versions correspond to the *next* release however, so
development software (i.e. the "master" branch in the
[git workflows](git-workflow.md) should always point to a as-yet-unreleased
version. This should be enforced by automated tools where practical (e.g.,
the Python package templates require a "release" tag to be set, and scripts
automatically use that to append "dev" to the version).

### Citing Software 

#### How-To-Cite Instructions

For users of our packages to be able to correctly cite our packages in their 
publications a `CITATION` file should be included in the top level of the GitHub
package that gives instructions on how the package should be cited in articles.
An example set of text can be found in the 
[CITATION](https://github.com/spacetelescope/style-guides/blob/master/templates/CITATION)
file in this repository.

The `__init__()` method in the top level directory of the package should contain
code that creates attributes `__citation__` and `__bibtex__` which is text
from the `CITATION` file. This can be accomplished by including:

```python
# Set the bibtex entry to the article referenced in CITATION.
def _get_bibtex():
    citation_file = os.path.join(os.path.dirname(__file__), 'CITATION')

    with open(citation_file, 'r') as citation:
        refs = citation.read().split('@ARTICLE')[1:]
```

As well, the `README` file should include a short paragraph that refers people
to the `CITATION` file to see how the code should be referenced. For example:

```
Refer to the CITATION file to see how to reference this software in articles,
papers and talks.
```

#### DOI for software releases

Our open source software available to the public should include a DOI (Digital
Object Identifier) that has now become the standard for journal articles.
Including a DOI for each released version of software will enable (and
encourage!) the public to reference our software and even more, the specific
version they used.  [Zenodo](https://zenodo.org) enables the automatic DOI
creation when a release is done on GitHub.  Follow the discussion [on the
GitHub guide](https://guides.github.com/activities/citable-code/) in order to
use DOI's for your code releases. The Zenodo badge should be included at
the top of the `README` file for easy referencing.


### Software others build

Whenever practical, the re-use of open software built by others is desirable if
it meets the needs in software *we* are building. This requires managing of
dependent versions: as they may either stop being supported or make
backwards-incompatible changes that break the dependent software. Because of the
broad range of development models that exist, it is very hard to set detailed
guidelines beyond it being the responsibility of the maintainer to keep an 
eye on how dependencies are evolving and set what version
should be supported. However, two good rules of thumb are:

* "The current version and the previous two minor releases" is often a
  reasonable choice of dependency support. This is shorthand for "what the
  majority of the user community uses", though, so if data on the user
  community's uptake is available, that is preferred over this rule of thumb.
* Only versions supported by the dependency's maintainers should be supported
  by the dependent, unless the dependent software's maintainer is willing to
  take complete responsibility for support of the old version of the dependency.

Building these rules into the test infrastructure for the package can be a very
useful way to catch changes in dependencies that may affect your package. 

One final guideline is that it is generally not the responsibility of
individual package authors to to ensure a reproducible scientific environment.
That is, it is up to *users* of our packages to document package dependency
versions if they need to create a reproducible environment (e.g., to reproduce
a scientific result) in the future. So, if they install a specific version of
our package, it is up to them to record versions of all their dependencies as
well. While as an institute we may wish to provide tools or documentation on
how users might do this, any *particular* package need not aim to provide a
specific reproducible environment.

Minimal pinning of dependencies should happen and should likely only be `>=`
type of dependencies (e.g., `numpy>=1.12`) as opposed to pinning a dependency
to a specific minor or patch release (i.e., don't use `numpy==1.12`). It is up
to the package maintainer to regularly check for new releases of dependencies
and to confirm they are working (this could be by automated weekly cron jobs,
for example). If a dependency creates a release that breaks the maintainer's
package, then the maintainer should create an issue in the dependency's issue
tracker reporting a problem.  If not promptly fixed, an exclusion of the broken
dependency version (i.e. one that uses the exclusion mechanics of [PEP
440](https://www.python.org/dev/peps/pep-0440)) should be added in the next
release of the maintained package, or if impractical, a work-around. If fixed
quickly enough, an exclusion may not be necessary, although a "known issues"
note may be useful if such a section exists in the documentation or changelog.


## Python

Python presents some specific challenges owing to the significant changes in the
language in the 3.x series, and the large amount of legacy software that exists
for Python 2.x. Additionally, the enormous open-source community of Python
software has leads to a  diversity of dependent version-tracking tools and
complexities.

We strongly recommend that all code is developed for Python 3.x.  New Python 
packages should target 3.x exclusively and not use compatibility packages 
like `2to3` or `six`.

### Major Version Support
The target major Python version is Python 3.x. 

The reasoning behind this is straightforward: the Python and Astropy developers
are sun-setting support for Python 2.x. This means that by 2019 (possibly 2020),
 no security patches nor new Astropy development will be available for Python
 2.x. This is both a direct security risk and a hinderance to development, as
 maintaining Python 2.x compatibility generally leads to more complex (and
 hence potentially buggy) code as well as preventing the use of any new Python
 3.x features (as these are not backwards-compatible with Python 2.x). Hence
 this guide provides a transition pathway and option to not break *existing*
 code, while discouraging future Python 2.x development. For additional
 background on the value of (particularly for scientific workflows), see the
 ["Python 3 for Scientists"](https://python-3-for-scientists.readthedocs.io/en/latest/)
 page.


Existing code currently not compatible with Python 3.x should either be updated
as soon as practical, or if this is judged to be infeasible, support guarantees
will terminate no later than 2019 (on the same date that the Python language and
Astropy core no longer support 2.x). If 3.x support is enabled by compatibility
packages, these should also be phased out as soon as practical, enabling usage
of Python 3.x-only features.

For released packages that currently support Python 2.x, one final version
should be released with Python 2.x compatibility. The release should inform
users (via readme or docs or whatever is appropriate for the package) that
future versions will not guarantee any Python 2.x compatibility.

Operational environments using Python 2.x should transition to 3.x as soon as
all of the package that are required for them have transitioned. This includes
testing/continuous integration of packages, which should no longer test against
Python 2.x once their dependent packages have completed the transition.

#### Resources for converting to 3.x
Converting code from Python 2.x to 3.x can in some cases be very
straightforward.  The 2to3 tool, maintained by the Python developers, provides
an automated way to convert code written for Python 2 to support Python 3, and
in general it will catch most required changes.  Some subtleties exist, mostly
notably dealing with strings (which are ASCII bytes in Python 2 and Unicode in
Python 3), and certain kinds of iterators (e.g. ``.iteritems()`` in Py 2 vs
``.items()``  in Py 3), but these tend to be unusual cases, or are non-optimal
(rather than breaking) cases that can be dealt with later after conversion.  A
few external resources for transitioning Python 2 to Python 3 code include:

* [Python 3 for Scientists Transition Guide](https://python-3-for-scientists.readthedocs.io/en/latest/python3_transition_guide.html)
* [Python 2 to 3 porting guide (from Python 3 docs)](https://docs.python.org/3/howto/pyporting.html)
* [Exhaustive "cheat sheet" of Python 2→3 idioms](http://python-future.org/compatible_idioms.html)
* [Python 3 Porting "book" (free online)](http://python3porting.com/)

### 3.x Minor Version Support
The above does not specify *which* Python 3.x should be the compatibility
target. The policy on minor versions is more heuristic than policy because
packages may wish to use specific features in newer Python 3.x versions. In
general the following guidelines should be considered:

* Any Python version that is no longer supported by the Python developers (at
  the time of this writing, that includes all versions < 3.4, excepting 2.7.x)
  should not be supported by any tool, as security guarantees cannot be made.
* Any version that is listed as "supported" by a tool should be tested by the
  developers on that Python version.
* The user community for a specific tool might have a quite different spread in
  installed Python versions. A version should continue to be supported if it is
  a significant burden to the user community to be forced to update to a newer
  version. A good rule of thumb for this is that any Python version less than
  two years old should be supported, or if the package is less than two years
  old, whatever versions have existed since the package was created.
* If a particular feature of a new Python version is desired by a package, but
  it conflicts with the above rules, where possible an effort should be made to
  use compatibility functions or external packages (new Python features are
  often backported to older versions via external packages soon after the new
  Python version is released) to support the feature on older Python versions.
  Where this is difficult or impossible, a judgement call must be made as to
  whether the new feature improves maintainability or feature support enough to
  warrant the disruption to the user community.

In general the package maintainer is responsible for applying the above
guidelines, at least where they have regular contact with the user community.
Consultation with team/branch leadership is advised where the right choice is
not clear, however, or a project manager or product owner if one is relevant for
the package.

### Community Dependencies Version Support

In addition to actual Python versions, most Python packages have dependencies on
community libraries (e.g., numpy, scipy, or astropy). The guidelines for these
packages are similar to the general software case, and are essentially the same
as for 3.x Minor Versions, but with two important differences:

* Only versions supported by the community package's maintainers should be supported. 
  That is, in the first guideline replace "Python developers" with "the package's 
  maintainers".
* Because many community supported packages have more uneven release cycles than
  the Python language, and support effort scales roughly with the number of
  requires versions to support, for community packages a better guideline for
  which versions to support is "the current version and the previous two". As
  above, this depends substantially on the user community, but this
  rule-of-thumb is a good one when no other information is available.

As a reference, please see the [Astropy version support policy](https://github.com/astropy/astropy-APEs/blob/main/APE18.rst).

As for 3.x Minor Versions, the package maintainer is responsible for applying
the guidelines, but in consultation with team/branch leadership or a product
owner/project manager.
