# Jupyter Notebooks

Juptyer Notebooks are a convenient format for creating and sharing documents that combined code, analyses, vizualizations, and prose. This guide describes best practices for creating readable (easy to understand), portable (likely to work on many computers) notebooks, and is the required format for any contributions to the [STScI notebooks repository](https://github.com/spacetelescope/notebooks).

## Principles

**Design for portability**

Notebooks should be portable. Don't assume a consumer of this notebook is running in the same environment as you.

- APIs not file systems. Use libraries such as [`astroquery.mast`](https://astroquery.readthedocs.io/en/latest/) to retrieve data you use with your notebook. Never hard-code a path to a file on e.g. a shared filesystem.
- If you need specific packages installed to enable your notebook to execute, define them in a custom [`requirements.txt`](https://pip.pypa.io/en/stable/reference/pip_install/#example-requirements-file) file that can be used to install these dependencies.

**Don't make assumptions**

As the notebook author, don't assume people know the same things as you. This means any terms/common acronyms should be defined when they are first used. If you're using some kind of astronomical parameter, define it (e.g. in its mathematical form), link to definitions in the literature etc.

**Execute all cells**

When your notebook is checked into the [STScI notebooks repository](https://github.com/spacetelescope/notebooks), all of the cells will be executed by our testing infrastructure.

**Don't check in executed cells**

When your notebook is checked into the [STScI notebooks repository](https://github.com/spacetelescope/notebooks), we run `nbconvert` and turn your notebooks into web-hosted versions. At that point, the cells will be executed... and rendered?

**Define your environment**

If you need specific packages installed to enable your notebook to execute, define them in a custom [`requirements.txt`](https://pip.pypa.io/en/stable/reference/pip_install/#example-requirements-file) file that can be used to install these dependencies.

## Notebook structure

- [Table of Contents](#table-of-contents)
- [Introduction](#introduction)
- [Defining terms](#defining-terms)
- [Section structure](#section-structure)

## Prose

Markdown for prose. Inline comments used sparingly.

## Associated files

e.g. images. Where to put them, other files required by the notebook (such as `requirements.txt`)

## Contributing to the STScI notebooks repository

This seems like a duplication of the contributing guidelines for the [STScI notebooks repository](https://github.com/spacetelescope/notebooks).

- [Naming](#naming)
- [Maintainers](#maintainers)
- [What to check in](#what-to-check-in)
