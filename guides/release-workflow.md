# Release And Maintain a Package Using Git

## Introduction

This document aims to facilitate the process of releasing a package by providing
possible workflows for managing the code repository and instructions for
releasing, as well as list good practices for releasing python packages.
Although the document focuses on and uses python packages as examples may of the
“good practices” are general and apply to software using any language. Two kinds
of workflow are discussed. Which one to use depends on whether it is necessary to
maintain old versions of the package or not.

1. A package is developed on the master branch and released from the master branch.
   Old versions are not maintained and each new version is released from the master branch.
2. All work happens on the master branch again. Releases are made on release branches.
   Bug fixes may be applied to release branches, effectively maintaining old versions,
   and “hot-fix” releases are made from the release branches.


## Good Practicies

There are certain activities which are common for all workflows and are simply
good practicies when releasing a package.

1. Version the package using semantic versioning.
   Details can be found in the
   [versioning guide ](https://github.com/spacetelescope/style-guides/blob/master/guides/software-versioning.md).

   Before a release update the version of the package to the intended version
   number. After a release is tagged in the repository and before going back to
   normal development change the version string in setup.py to the next release,
   followed by dev. For example, if v 2.1.3 is to be released and the next version
   will be 2.2.0, before a release is made, set the version string in setup.py to
   version = “2.1.3”. After the release is done, set the version string to
   version=”2.2.0.dev”.

   [PEP440](https://www.python.org/dev/peps/pep-0440) describes python package versioning conventions.

2. Maintain a change log for the package.

   It is useful to users if the package has a change log. Many packages maintain
   a file at the top level, called CHANGES.rst. This file should have a section
   for each released version and possibly subsections for new features, bug fixes
   and API changes. Examples can be found in
   [astropy](https://github.com/astropy/astropy/blob/master/CHANGES.rst) and
   [jwst](https://github.com/spacetelescope/jwst/blob/master/CHANGES.rst).

3. When preparing a release use branches and pull requests (PR) following the
   [Workflow for contributing to a Git repository](https://github.com/spacetelescope/style-guides/blob/master/guides/git-workflow.md).

4. Tag the software with a "release tag" in the repository.

   Create an annotated tag ("release tag") in the repository for each release.
   The tag should be associated with the branch the release is made from.
   The command to do this is:

   ```
   $ git checkout <release_branch>
   $ git tag -a "2.1.3" -m "Tagging release v 2.1.3"
   ```

5. Publishing a Release on Github

   Online, at the repository root, click on “releases” → “Draft a New Release”.
   Enter the release tag. Describe the release, enter highlights and publish it.

6. Release on PyPi

   It is common that releases of python packages are published on the Python
   Packaging Index (PyPi) site. This allows users to find them and install
   them using `pip`. A PyPi account is required.

   For publishing a release of a python package on PyPi it's best to use
   [twine](https://pypi.org/project/twine/) - a utility for publishing Python
   packages on PyPi. Not surprisingly `twine` itself is published on PyPi and
   can be installed using `pip`. The commands for publishing a release on PyPi are:

   ```
   $ git checkout <release_tag>
   $ git clean -fxd
   $ python setup.py build sdist --format=gztar
   $ twine upload dist/*
   ```

   `Note:` The `Project Description` on PyPi is populated by the `Classifiers` in
   setup.py. It is useful to populate those accurately and completely.
   The `long description` field is used to populate the "Project Description" on PyPi.

## Specific release procedures

The following two sections provide specific instructions for releasing a python
package in the two main use cases listed above. It is assumed the person doing the
release uses the `spacetelescope` repository directly as a working directory.
This means the main repository at `spacetelescope` needs to be cloned and
that "origin" points to `spacetelescope`, not to the person's fork.

  ```
  $ git clone git@github.com:spacetelescope/mypackage.git
  $ cd mypackage
  $ git remote -v
    origin git@github.com:spacetelescope/mypackage.git (fetch)
    origin git@github.com:spacetelescope/mypackage.git (pull)
  ```


### Developing and releasing from the master branch.

In this case all work is done on the `master` branch. Old releases are not
maintained. When the code is ready for release, set the version in setup.py
and set the date of the release in CHANGES.rst. Tag the release on master.

  ```
  $ git checkout master
  $ git status
  On branch master
  ```

Create an annotated tag which corresponds to the release and push it to the repository.
Publish the release on Github and PyPi.

### Use a different release branch for each release.

In this case the release is prepared on a dedicated branch ("release branch").
It is possible to maintain old releases by doing a patch release off a release branch.
The instructions below use a third branch called "stable". This is the production branch.
Release branches are merged with it and the release is tagged on the "stable" branch.

The specific steps are:

1. Start by editing the change log file (and possibly setup.py) to set the release
   version and date. Do this on the master branch.

2. Prepare a release branch.

   Make sure you are on the master branch. Make a branch for the release. Name
   the branch 2.1.3x. Always use a letter at the end of the branch name so that
   the version number (2.1.3) can be used later as a tag as duplicate names are harmful.

   ```
   $ git remote -v
     origin git@github.com:spacetelescope/jwst.git (fetch)
     origin git@github.com:spacetelescope/jwst.git (push)

   $ git status # should show you are on master.
     On branch master
     Your branch is up-to-date with 'origin/master'.
     nothing to commit, working tree clean

   $ git branch 2.1.3x
   $ git checkout 2.1.3x
   ```

3. Push the 2.1.3x branch to `spacetelescope` and create a PR to the "stable" branch.
Note, that origin below refers to the `spacetelescope` repository. When tests pass
merge with "stable".

   `git push origin 2.1.3x`

4. Checkout and update the "stable" branch and tag it with the new release.

   ```
   $ git checkout stable
   $ git pull origin stable
   $ git tag -a "2.1.3" -m "tagging release 2.1.3"
   $ git push origin 2.1.3
   ```

   Prepare master for further development

5. Update the change log for the next release and push it to master. Make sure the
current branch is "master".

Note: If this package is to be installed nightly internally in conda-dev then "master"
needs to be tagged with a "newer" tag which is picked up by conda-dev build scripts.
The tag name should be chosen in such a way that conda would resolve it as the
latest tag. This allows the nightly build scripts to always pick up the latest
version from master. The suggested tag is the next version, followed by the
character `a`.

   ```
   $ git checkout master
   $ git tag -a "2.2.0a" -m "tagging for further development"
   $ git push origin 2.2.0a
   ```

#### Make a bug fix release

This section explains how to do a bug-fix (or hot-fix) release.
Development on the master branch towards v 2.2.0 has been going on
for a while. A particularly vicious bug was discovered in 2.1.3 and needs
to be fixed immediately. The bug applies also to code on the master branch
so it is fixed there and merged through a PR. The commit hash is `a266119d38`.
This commit needs to be applied to the already released version 2.1.3 and released
through version 2.1.4.


1. Create a branch for the patch release off the 2.1.3x release branch.

   ```
   $ git checkout -b 2.1.4_patch origin/2.1.3x
   ```

2. Apply the commit to this branch using the `cherry-pick` command. If more than
   one commits are necessary apply all of them to the new branch.

   ```
   $ git cherry-pick -x a266119d38
   ```

   In some cases this commit may have more than one parent commits. Then it has
   to be determined which one was the immediate parent and pass this to the
   cherry-pick command which becomes (first parent is used):

   ```
   $ git cherry-pick -x -m 1 a266119d38
   ```

   What changes are going in after a `cherry-pick` command can be verified
   with `git show`.

   In some cases cherry picking commits may result in conflicts. These should be
   fixed the usual way.

3. Push the branch to the repository, create a PR and after tests pass merge with
   the release branch. The tip of the release branch now has the new patch release.
   Create a PR to merge with the "stable" branch and tag the new release with
   tag `1.2.4` on "stable". These are the steps 4 and 5 in the previous section.
