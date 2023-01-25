# Testing Python Packages

The following guide is to layout a general guideline for testing Python
packages. This document is not to be followed exactly word-by-word, but rather
it lays out multiple things you should consider. Feel free to adopt and adapt
as you deem necessary.

## Framework for python tests

[pytest](https://docs.pytest.org/en/latest/) is recommended.
There are many ``pytest`` plugins that you could utilize to get the most
out of your tests. Example of several useful ones:

* [pytest-doctestplus](https://github.com/astropy/pytest-doctestplus) for
  advanced features for testing example code in documentation.
* [pytest-arraydiff](https://github.com/astrofrog/pytest-arraydiff) for
  generation and comparison of data arrays produced during unit tests.
* [pytest-socket](https://github.com/miketheman/pytest-socket) for handling
  network calls.
* [pytest-faulthandler](https://github.com/pytest-dev/pytest-faulthandler)
  for getting a useful traceback out of segmentation fault.
* [ci-watson](https://github.com/spacetelescope/ci_watson) for functionality
  related to STScI Jenkins and Artifactory usage.

## Testing Layout

See the templates in [Python Packaging section](https://github.com/spacetelescope/style-guides/blob/master/guides/python-package.md).

Template aside, there is also the question of whether you want to bundle the
tests with your package or not. While bundling allows users to test their
installations, it also increases the package size. For more information,
see "Choosing a test layout / import rules" in
https://docs.pytest.org/en/latest/goodpractices.html .

## Types of Tests

In theory, there are many kinds of software testing. For instance,
unit testing, integration testing, regression testing, etc. Currently, this
document focuses on unit and regression tests (usually "shipped" with the
package they are tied to), as well as continuous integration testing.

* Unit test: Tests that the functionality is correct. Runs per push event
  (e.g., commit or merge).
* Regression test: Tests that the functionality is robust. Runs in a
  pre-defined cadence (e.g., nightly or weekly).

In unit testing, these are the recommended things to test for:

* Different logic flows.
* Edge cases.
* Code coverage check. Note that 100% coverage does not imply perfection.
* PEP 8 check.
* Documentation build, with warnings turned to exceptions. Yes, your code
  should be documented (but documentation is a topic for another page).

While regression testing is suitable for:

* Testing against different versions of "upstream" dependencies, including
  dev versions.
* Testing against different versions of Python.
* Testing against different operating systems (e.g., Linux, OSX, or Windows).
* Testing against different hardware architectures.

While it is ideal for each test build to only test one thing, usually that is
not practical, as it can take a long time to finish the whole series of builds.
It simply does not make sense for, say, a nightly test to take more than one
night to run. Hence in reality, cross-testing is more common; e.g., testing
against an older version of Python *and* an older version of Numpy (with
the assumption that users who use that older version of Python are also
more likely to have an older version of Numpy).

In some situations, you might also want to fold in some regression tests
when running unit tests (e.g., testing against several different "upstream"
versions and operating systems when a pull request is opened or modified).
On the other hand, you might also want to consider only running certain
unit tests in regression mode, especially ones that run slow or generate
a lot of network hits, which might exceed time out quota of certain services.

### Tests doomed to fail

See https://docs.pytest.org/en/latest/skipping.html .

### Testing with outside dependencies

If your test calls something that you do not care about (e.g., some database
access that is not part of your package), you can "mock" that portion in your
test. For more information, see
https://docs.python.org/3/library/unittest.mock.html .

## Continuous Integration

As you can see, there are many things one can test for in a package. This can
be a daunting job if you were to test all these manually on different machines.
This is where continuous integration (CI) comes in. CI can be set up to handle
push events (e.g., commits to GitHub) or to run in a preset cadence
(e.g., nightly).

For GitHub, here are some services available:

* GitHub Actions: This is a paid service billed centrally to STScI.
  Repositories on ``spacetelescope`` GitHub organization should use it,
  if possible.
* Jenkins CI: This is an in-house CI solution with virtually no limitation
  (but do not push your luck). It supports storing big data and artifacts in
  Artifactory. If STScI staff need to execute tests at regular intervals, or
  require direct access to Central Storage, please refer to the following documentation,
  [Users Guide: Running Regression Tests](https://innerspace.stsci.edu/display/SSR/Users+Guide%3A+Running+Regression+Tests)
  for more information. Unfortunately, it is not easily accessible for non-STScI
  personnel. It supports Linux (label: `linux`) and MacOS (label: `macos`) operating systems.
* Circle CI: This is a free service (with limitations). It supports storing
  artifacts but is lacking in good customer service.

For STScI GitLab instances, please contact ITSD.

## Automated test code coverage

There are several packages available to quantify the test-code coverage of the
GitHub repository code.  One is [Coveralls](https://coveralls.io) and some
packages at STScI are using this as part of the CI automated procedure. A
second is [Codecov](https://codecov.io/) that is very similar to Coveralls.
There are other STScI GitHub repositories that are using Codecov.

There are benefits to both packages and one of them should be setup in each
repository to quantify the test code coverage and to encourage writing test
code. At this time, Codecov is the recommended service over Coveralls.
There appear to be several benefits to using Codecov (over Coveralls)
including a cleaner method to automatically report test-code coverage test on a
PR within GitHub. So, it is encouraged that each STScI repository include
Codecov coverage tests as part of the automated CI in Actions or Jenkins.


## Displaying test status

Each of the different CI services provide badges that you can add to your
package repository's read-me file, which GitHub would render nicely.
For example, see https://github.com/spacetelescope/jwst/blob/master/README.md .

## Testing non-Python package

See [HSTCAL](https://github.com/spacetelescope/hstcal), which is a C
pipeline but utilizes ``pytest`` framework with Python test scripts.
