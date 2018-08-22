# Jupyter Notebooks

Juptyer Notebooks are a convenient format for creating and sharing documents that combine code, data analyses, vizualizations, and prose. This guide describes best practices for creating readable (easy to understand), portable (likely to work on many computers) notebooks. Following this guide is a requirement for those authors contributing content to the [STScI notebooks repository](https://github.com/spacetelescope/notebooks).

## Design principles

### Import your dependencies at the start

If you need specific packages to enable your notebook to execute, import them at the top of your notebook and explain why you're using them. For example:

![Imports](https://user-images.githubusercontent.com/4483/44419575-98d39980-a549-11e8-9441-e57ae20256f4.png)

### Make no assumptions

As the notebook author, don't assume people know the same things as you. This means any terms/common acronyms should be defined when they are first used. If you're using some kind of astronomical parameter, make sure you define it (e.g. in its mathematical form) and link to any definitions (literature/Wikipedia etc.)

### Design for portability

Notebooks should be portable, that is, the should be designed to work on multiple computers. There are a few simple steps you can take as a notebook author to increase the 'portability' of a notebook:

- Use APIs not file systems to access data. Where at all possible, use libraries such as [`astroquery.mast`](https://astroquery.readthedocs.io/en/latest/) to retrieve the data required for your notebook. Never hard-code a path to a file on e.g. a shared filesystem.
- If you need specific packages installed to enable your notebook to execute, define them in a custom [`requirements.txt`](https://pip.pypa.io/en/stable/reference/pip_install/#example-requirements-file) file that can be used to install these dependencies.

### Keep good cell discipline

![A notebook cell](https://user-images.githubusercontent.com/4483/44419332-d7b51f80-a548-11e8-8125-457bcfc23d30.png)

Creating a new notebook can take time, and in the development process, some content cells (code and prose) may become out of date/superfluous. Before checking in your work, make sure that:

- Cells capture logical units of work. i.e. don't put all of your code in a single giant cell of logic. Try and break it out into smaller units, inter-dispersed with text explaining what you are doing.
- All of the cells are required. i.e. you can go from the start of your notebook to the end, executing each cell.
- Checked-in notebook don't contain the executed cell outputs. When your notebook is checked into the [STScI notebooks repository](https://github.com/spacetelescope/notebooks), we run [`nbconvert`](https://nbconvert.readthedocs.io/en/latest/) and turn your notebooks into web-hosted versions. At that point, the cells will be executed... and rendered?

## Recommended notebook structure

It's recommended that Jupyter notebooks use the following suggested structure:

- [Title](#title)
- [Table of Contents](#table-of-contents)
- [Imports](#imports)
- [Introduction](#introduction)
- [Loading data](#loading-data)
- [File information](#file-information)
- [Sections](#sections)
- [Additional resources](#additional-resources)
- [About this notebook](#about-this-notebook)
- [Footer](#footer)

### Title

Pick a clear, descriptive title. For titles, use the [following Markdown syntax](https://www.markdownguide.org/basic-syntax/#headings):

```
# My clear, descriptive title
```

### Table of Contents

Use a [Markdown list](https://www.markdownguide.org/basic-syntax/#unordered-lists) to link to the different sections of your notebook:

```
- [Imports](#imports)
- [Introduction](#introduction)
- [Defining terms](#defining-terms)
```

### Imports

Import your dependencies near the top of the notebook, explaining why you're including each one. For example:

![Imports](https://user-images.githubusercontent.com/4483/44419575-98d39980-a549-11e8-9441-e57ae20256f4.png)

### Introduction

Write a short introduction explaining the purpose of the notebook. Link to any background materials/resources that may be useful to the reader to provide additional context.

#### Defining terms

Be sure to define any terms/common acronyms at the end of your introduction. If you're using some kind of astronomical parameter, make sure you define it (e.g. in its mathematical form) and link to any definitions (literature/Wikipedia etc.)

### Loading data

If the user needs to download data to run the tutorial properly, where possible, use [Astroquery]((https://astroquery.readthedocs.io/en/latest/)) (or similar) to retrieve files.

### File information

Explain pertinent details about the file you've just downloaded. For example, if working with Kepler lightcurves, explain what's in the different file extensions:

```
- No. 0 (Primary):
This HDU contains meta-data related to the entire file.
- No. 1 (Light curve):
This HDU contains a binary table that holds data like flux measurements and times. We will extract information from here when we define the parameters for the light curve plot.
- No. 2 (Aperture):
This HDU contains the image extension with data collected from the aperture. We will also use this to display a bitmask plot that visually represents the optimal aperture used to create the SAP_FLUX column in HDU1.

```

### Sections

Break sections up with the following [Markdown syntax]((https://www.markdownguide.org/basic-syntax/#headings)):

```
## Section 1

## Section 2
```

### About this notebook

Let the world know who the author of this great notebook is! If possible/appropriate, include a contact email address for users who might need support (e.g. `archive@stsci.edu`)

### Footer

Notebooks should use the standard STScI footer:

![Footer](https://user-images.githubusercontent.com/4483/44435082-4e1f4500-a57c-11e8-88d3-6d09129f5f61.png)

## Prose

Use [Markdown](https://www.markdownguide.org/basic-syntax/#paragraphs-1) for text formatting and prose. When necessary, include small inline comments before a code block in a cell.

## Ancillary files

Sometimes you need to include ancillary files with your notebook. Examples include images, small data files (e.g. CSV, FITS tables). If your notebook needs ancillary files to work, make sure you include them at the root level relative to your notebook. E.g.:

```
Notebooks/
|-- MyAwesomeNotebook
|    |-- my_awesome_notebook.ipynb
|    +-- galaxy.png
|    +-- data.csv
|    +-- requirements.txt
```

## Further reading

### Example notebooks following this style guide

Here are some example notebooks that follow this style guide:

- [Kepler Full Frame Images (FFI)](https://github.com/spacetelescope/notebooks/blob/master/MAST/Kepler/Kepler_FFI/kepler_ffi.ipynb)
- [Kepler Lightcurves](https://github.com/spacetelescope/notebooks/blob/master/MAST/Kepler/Kepler_Lightcurve/kepler_lightcurve.ipynb)
- [Kepler Target Pixel Files (TPF)](https://github.com/spacetelescope/notebooks/blob/master/MAST/Kepler/Kepler_FFI/kepler_ffi.ipynb)

### Contributing to the STScI notebooks repository

View the contributing guide in the [STScI notebooks repository](https://github.com/spacetelescope/notebooks).
