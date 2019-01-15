# Release And Maintain a Package Using GIT

## Introduction

The goal of this document is to facilitate the process of releasing a package by providing possible workflows for managing the code repository and instructions for releasing, as well as list good practices for releasing python packages. Although the document focuses on and uses python packages as examples may of the “good practices” are general and apply to software using any language. Two kinds of workflow are discussed. Which one to use depends on whether it is necessary to maintain old versions of the package. 

1. A package is developed on the master branch and released from the master branch. Old versions are not maintained and each new version is released from the master branch.
2. All work happens on the master branch again. Releases are made on release branches. Bug fixes may be applied to release branches, effectively maintaining old versions, and “hot-fix” releases are made from the release branches.


## Good Practicies
 
There are certain activities which are common for all workflows and are simply good practicies when releasing a package. 

1. Version the package using semantic versioning. Details can be found in the 
[versioning guide ](https://github.com/spacetelescope/style-guides/blob/master/guides/software-versioning.md)

Before a release update the version of the package to the intended version number. After a release is tagged in the repository and before going to normal development change the version string in setup.py to the next release, followed by dev. For example, if v 2.1.3 is to be released and the next version will be 2.2.0, before a release is made, set the version string in setup.py to
Version = “2.1.3”. After the release is done, set the version string to version=”2.2.0.dev”.

[PEP440](https://www.python.org/dev/peps/pep-0440) describes python package versioning conventions.

2. Maintain a change log for the package.

It is useful to users if the package has a change log. Many packages maintain a file at the top level, called CHANGES.rst. This file should have a section for each released version and possibly subsections for new features, bug fixes and API changes. Examples cna be found in [astropy](https://github.com/astropy/astropy/blob/master/CHANGES.rst) and [jwst](https://github.com/spacetelescope/jwst/blob/master/CHANGES.rst).

3. When preparing a release use branches and Pull requests (PR) following the [Workflow for contributing to a git](https://github.com/spacetelescope/style-guides/blob/master/guides/git-workflow.md).

4. Tag the softare with a "release tag" in the repository.

Create a tag ("release tag") in the repository for each release. Create an annotated tag. The tag shoul dbe associated withe the branch the release is made from.
The command to do this is.

  `git checkout <release_branch>`
  ` git tag -a "1.2" -m "Tagging release v 1.2" `
 
5. Making a Release on Github

Online, at the repository root, click on “releases” → “Draft a New Release”.
Enter the release tag (see below).
describe the Release. Enter highlights and publish it.

6. Release on PyPi.

It is common that releases of python packages are published on the Python Packaging Index (PyPi) site. This allows users to find them and install them using `pip`. A PyPi account is required.

For publishing a release of a python package on PyPi it's best to use [twine](https://pypi.org/project/twine/) - a utility for publishing Python packages on PyPi. Not surprisingly `twine` itself is published on PyPi and can be installed using `pip`.
The commands fo rdoing this are:

  `git checkout <release_tag>`
  `git clean -fxd`
  `python setup.py build sdist --format=gztar`
  `twine upload dist/*`

`Note:` The `Project Description` on PyPi is populated by the `Classifiers` in setup.py. It is useful to populate those accurately and completely. The `long dscription` field is used to populate the "Project Description" on PyPi.

## Specific release procedures

The following two sections provide specific instructions for releasing a python package in the two main use cases. They assume the person doing the release uses the spacetelescope repository directly as a working directory. This means that the first thing needed is to clone the main repository and that "origin" poits to the repository on spacetelescope, not to the person's fork.

  `git clone git@github.com:spacetelescope/mypackage.git`
  `git remote -v`
  `origin git@github.com:spacetelescope/mypackage.git (fetch)`
  `origin git@github.com:spacetelescope/mypackage.git (pull)`
  

### Developing and releasing from the master branch.

In this case all work is done on the `master` branch. Old releases are not maintained. When the code is ready for release, set the version in setup.py and set the date of the release in CHANGES.rst. Tag the release on master.

  `git checkout master`
  `git status`
  `On branch master`

Create an annotated tag which corresponds to the release and push it to the repository. Publish the reease on github and PyPi.
 
### Use a different release branch for each release.

In this case the release is prepared on a dedicated branch ("release branch"). It is possible to maintain old releases by doing a patch release off a release branch.

The specific steps are:

1. Strat by editing the change log file (and possibly setup.py) to set the release version and date.

2. Prepeare a release branch.

Make sure you are on the master branch. Make a branch for the release. Name the branch 0.9.4x. Always use a letter at the end of the branch name so that the version number (0.9.4) can be used later as a tag as duplicae names are harmful.

  ```
  $ git remote -v
  origin git@github.com:spacetelescope/jwst.git (fetch)
  origin git@github.com:spacetelescope/jwst.git (push)

  $ git status # should show you are on master.
  On branch master
  Your branch is up-to-date with 'origin/master'.
  nothing to commit, working tree clean

  git branch 0.9.4x
  git checkout 0.9.4x
  ```

3. Push the 0.9.4x branch to STScI-JWST and create a PR to the stable branch. Note, that origin below refers to the STScI-JWST repository.

  ` git push origin 0.9.4x`

4. Push the 0.9.4x branch to STScI-JWST and create a PR to the stable branch. Note, that origin below refers to the STScI-JWST repository.

