# Python Packaging

The following guide is to simplify the process of creating a python
package.  We currently recommend using the [STScI package
template](https://github.com/spacetelescope/stsci-package-template).
This is a simplified and modified version of the [Astropy
Template Package](https://github.com/astropy/package-template) that
includes a number of features that make it easier to set up, install,
and maintain your package while also having the STScI branding,
license, and Code of Conduct as outlined in the [guide for github
repositories](https://github.com/spacetelescope/style-guides/blob/master/guides/github-repositories.md).
This includes the infrastructure to set up the automated testing and
documentation.

The following steps will allow the creation of Astropy affiliated
packages compatible with the STScI requirements.  However, this might
not always be appropriate for each package and we currently do not
recommend existing packages update to these standards.


## Do you need a new python package?

There already is a large existing ecosystem of python libraries with a
wealth of different libraries for astronomy (see the [astropy
affiliated package](https://www.astropy.org/affiliated/) for some
examples) as well as packages in the
[spacetelescope](https://github.com/spacetelescope) organization.
While not required, it is recommended to make a contribution to one of
these packages rather than creating a new one.  For example, some of
the following these packages may be good places to contribute the
code:

* General astronomy related functionality?  Have you consider contributing to astropy?
* Access to a database or a catalog?   This would be a good fit for astroquery!
* Spectral tools?   Have you considered specutils or specviz?
* Analysis tools for an HST instrument?  Consider contributing to one of the HST tools packages like
* Will this be useful for JWST?  Consider contribute to the JWST library.

This list is hardly exhaustive, but provides an example of where to
contribute code.  If unsure, opening an issue in a repository to ask
is a very good way to find out if it is appropriate to contribute the
new functionality there.  Nonetheless, there might be times that it is
useful to create your own repository for new functionality or to test
something prior to merging it into a larger package and as such, there
will be a need to create a new package.


## Setting up a repository for your package

Prior to setting up your repository, it is strongly recommended to
google potential names.  If it already exists consider choosing a
different name (or contributing to that package if appropriate).  If
the repository acronym can have offensive meanings in other contexts,
try something different.  It is also useful to check GitHub and PyPi
for potential conflicts as well.

### GitHub vs GitLab

Following [STScI policy on source code
control](https://innerspace.stsci.edu/display/isec/Source+Code+Control),
the decision tree on where to host code is the following:

* Is it ITAR protected?  Then host it on internal git ITAR server.
* Is it functional work?   Then it must be hosted on the STScI GitLab solution or the `spacetelescope` organization on GitHub
* Will it have external collaborators?  Then it is recommended to host it on the `spacetelescope` organization on GitHub

Code hosted on GitHub should follow the [STScI GitHub
Policy](https://innerspace.stsci.edu/display/isec/GitHub).


### Creating a new repository

For GitHub:

Open a Service Now ticket requesting a new repository.  The ticket
should include the following information:

* Name of the repository
* Short description of the purpose of the repository
* If the repository should be public or private
* If applicable, any teams associated with the repository or other
individuals that should have access to it

For Gitlab:

After authenticating on Gitlab, the user should be able to create
their own new repository.


### Create a new package

To create a new package, we recommend the following steps.

1. Go to https://github.com/spacetelescope/stsci-package-template .

2. Click "Use this template" button (it is big and green).

See https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/creating-a-repository-from-a-template
for more info.

3. Carefully go through your new repository and customize as needed.

To customize GitHub Actions, look in `tox.ini` and `.github/workflows/`.

Your test matrix should test against various versions of Python, astropy,
and numpy, as applicable to your project.

For Jenkins, see https://github.com/spacetelescope/jenkins_shared_ci_utils .

4. Register your package on https://codecov.io if you want test coverage.

5. Set up ReadTheDocs (RTD)

If you want the documentation for your project to be hosted by
RTD, then you need to set up an account there. Your RTD
settings are controlled by `.readthedocs.yaml` in your repository.
Optionally, you can enable "Build pull requests for this project"
under Advanced Settings in Admin section.

Once these steps have been completed, the package should be ready for
further development with testing and documentation in place.

### Additional Reading

* [Guide for github repositories](https://github.com/spacetelescope/style-guides/blob/master/guides/github-repositories.md)
* [Documentation for the Cookiecutter Project](https://cookiecutter.readthedocs.io/en/latest/readme.html)
* [The Astropy Package Template](http://docs.astropy.org/projects/package-template/en/latest/)
