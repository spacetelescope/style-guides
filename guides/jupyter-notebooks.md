# Jupyter Notebooks

Juptyer Notebooks are a convenient format for creating and sharing documents that combine code, data analyses, vizualizations, and prose. This guide describes best practices for creating readable (easy to understand), portable (likely to work on many computers) notebooks. Following this guide is a requirement for those authors contributing content to the [STScI notebooks repository](https://github.com/spacetelescope/notebooks).

## Principles

**Import your dependencies at the start**

If you need specific packages to enable your notebook to execute, import them at the top of your notebook and explain why you're using them. For example:

![Imports](https://user-images.githubusercontent.com/4483/44419575-98d39980-a549-11e8-9441-e57ae20256f4.png)

**Make no assumptions**

As the notebook author, don't assume people know the same things as you. This means any terms/common acronyms should be defined when they are first used. If you're using some kind of astronomical parameter, make sure you define it (e.g. in its mathematical form) and link to any definitions (literature/Wikipedia etc.)

**Design for portability**

Notebooks should be portable, that is, the should be designed to work on multiple computers. There are a few simple steps you can take as a notebook author to increase the 'portability' of a notebook:

- Use APIs not file systems to access data. Where at all possible, use libraries such as [`astroquery.mast`](https://astroquery.readthedocs.io/en/latest/) to retrieve the data required for your notebook. Never hard-code a path to a file on e.g. a shared filesystem.
- If you need specific packages installed to enable your notebook to execute, define them in a custom [`requirements.txt`](https://pip.pypa.io/en/stable/reference/pip_install/#example-requirements-file) file that can be used to install these dependencies.

**Keep good cell discipline**

![A notebook cell](https://user-images.githubusercontent.com/4483/44419332-d7b51f80-a548-11e8-8125-457bcfc23d30.png)

Creating a new notebook can take time, and in the development process, some content cells (code and prose) may become out of date/superfluous. Before checking in your work, make sure that:

- Cells capture logical units of work. i.e. don't put all of your code in a single giant cell of logic. Try and break it out into smaller units, inter-dispersed with text explaining what you are doing.
- All of the cells are required. i.e. you can go from the start of your notebook to the end, executing each cell.
- Checked-in notebook don't contain the executed cell outputs. When your notebook is checked into the [STScI notebooks repository](https://github.com/spacetelescope/notebooks), we run [`nbconvert`](https://nbconvert.readthedocs.io/en/latest/) and turn your notebooks into web-hosted versions. At that point, the cells will be executed... and rendered?

## Notebook structure

- [Table of Contents](#table-of-contents)
- [Introduction](#introduction)
- [Defining terms](#defining-terms)
- [Section structure](#section-structure)

## Prose

Markdown for prose. Inline comments used sparingly.

## Associated files

e.g. images. Where to put them, other files required by the notebook (such as `requirements.txt`)

## Example notebooks following this style guide

- Notebook 1
- Notebook 2
- Notebook 3

## Contributing to the STScI notebooks repository

This seems like a duplication of the contributing guidelines for the [STScI notebooks repository](https://github.com/spacetelescope/notebooks).

- [Naming](#naming)
- [Maintainers](#maintainers)
- [What to check in](#what-to-check-in)
