# Jupyter Notebooks

.. do we want to specify that this is a guide for creating notebooks and
   tutorials, or would no one ever be contributing a tutorial that wasn't
   created in a Jupyter notebook? As in, are the terms "notebook" and "tutorial"
   interchangeable?

Jupyter Notebooks are a convenient format for creating and sharing documents
that combine code, data analyses, visualizations, and prose. This guide
describes best practices for creating readable (easy to understand), portable
(likely to work on many computers) notebooks. Following this guide is a
requirement for those authors contributing content to the [STScI notebooks
repository](https://github.com/spacetelescope/notebooks).

## Example notebook

This [example notebook](../templates/example_notebook.ipynb) implements this
style guide with placeholder content. If you want to create a new notebook
following this guide then you might want to start from this one.

## Design principles

### Make no assumptions

As the notebook author, don't assume people know the same things as you. This
means any terms or common acronyms should be defined when they are first used.
It you're using some kind of astronomical parameter, make sure you define it
(for example, in its mathematical form) or link to any definitions (literature,
Wikipedia, etc.). If you think this is making your notebook too detailed, use
clearly-named sections with appropriate introductions, or split your notebook
into two separate ones that reference each other.

Above all, know your audience: if you are writing a notebook only for
astronomers in a specific field, you might be more terse on background. But if
so, *say so* at the beginning of your notebook, and know that most readers will
not get anything from it.

Always avoid assumptive or belittling words such as “obviously,” “easily,”
“simply,” “just,” or “straightforward.” Avoid words or phrases that create worry
in the mind of the reader. Instead, use positive language that establishes
confidence in the skills being learned.

### Design for portability

Notebooks should be portable, that is, they should be designed to work on
multiple computers. There are a few basic steps you can take as a notebook
author to increase the "portability" of a notebook:

- Use APIs, not file systems, to access data. Where possible, use libraries such
  as [`astroquery.mast`](https://astroquery.readthedocs.io/en/latest/) to
  retrieve the data required for your notebook. Never hard-code a path to a file
  on, for example, a shared filesystem. See the [data
  guide](where-to-put-your-data.md) for more detail on how you might implement
  this.
- If you need specific packages installed to enable your notebook to execute,
  define them in a custom
  [`requirements.txt`](https://pip.pypa.io/en/stable/reference/pip_install/#example-requirements-file)
  file that can be used to install these dependencies. Be as specific as
  possible: sometimes packages make backward incompatible changes, so specifying
  a particular version of dependencies will protect against that.
- Try to avoid long-running computations or large downloads. Not all users will
  have good internet access or a fast computer when they try to run your
  notebook. Instead, try to use compact examples that work on smaller portions
  of a dataset. While this is not always possible given the goals of a
  particular notebook, it is best to strive for it. At the very least, be sure
  to warn the user clearly if a notebook will have long runtime or large
  downloads.
- Avoid using any unnecessary non-markdown constructs. While it's tempting to
  use HTML directly in a notebook cell, it's best avoided if at all possible,
  because there's no guarantee that the notebook viewer is in the same website
  all the time. Markdown, meanwhile, is specifically designed with that
  portability in mind.

### Keep good cell discipline

![A notebook cell](images/notebook-cell.png)

Creating a new notebook can take time, and in the development process, some
content cells (code and prose) may become out of date or superfluous. Before
committing your work to a source repository, make sure that:

- Cells capture logical units of work. That is, don't put all of your code in a
  single giant cell of logic. Try to break it out into smaller units,
  interspersed with text explaining what you are doing.
- All of the cells are required and *in order*. That is, you can go from the
  start of your notebook to the end, executing each cell. Cells should only fail
  if they are *meant* to, because you're illustrating the meaning of an error
  message or something similar.
- Checked-in notebooks shouldn't contain the executed cell outputs. Any results
  you check in take up valuable space in the notebook, making it harder to
  review and bloating the repository. When your notebook is checked into the
  [STScI notebooks repository](https://github.com/spacetelescope/notebooks), we
  will run [`nbconvert`](https://nbconvert.readthedocs.io/en/latest/) to execute
  your notebook (overriding anything already executed) and
  [`sphinx`](http://www.sphinx-doc.org/) to create web-hosted versions.

### Write readable prose using Markdown

Use cells with
[Markdown](https://www.markdownguide.org/basic-syntax/#paragraphs-1) formatting
to describe anything that isn't actually code. Also use them either before or
after code cells to describe what's happening. When mathematical expressions are
needed, use the [Jupyter additions to
Markdown](https://jupyter-notebook.readthedocs.io/en/stable/notebook.html#markdown-cells),
which are basically carefully controlled LaTeX.

Only use code comments when it's a natural inline comment directly connected to
that line of code. Do **not** use code cells with comments to replace
well-written prose!

## On-disk layout and ancillary/generated files

Individual notebooks should live in their own directory alongside any ancillary
files related to them. For example:

```
notebooks/
|-- MyAwesomeNotebook
|    |-- my_awesome_notebook.ipynb
|    +-- object-data.csv
|    +-- result-plot.png
|    +-- requirements.txt

```

As this demonstrates, sometimes it is appropriate to include ancillary files
with your notebook. Examples include small images or data files such as FITS
cutouts not accessible via MAST, or CSV tables. Files of any significant size (>
100 kB is a good rule of thumb) should not be included with the notebook but
rather stored outside of the repository and accessed via code (go to [the data
guide](where-to-put-your-data.md) for details on how to do this). As shown
previously, if your notebook needs ancillary files, you should include them at
the same level as your notebook.

Similarly, if your notebook involves *writing* files, you should write the
notebook so that they are written in the same location. As an example, if you
are making a scatter plot using matplotlib and want to demonstrate to the user
how to save it, you can do this in the notebook:

```
plt.scatter(...)
plt.savefig('result-plot.png')

```

And the image will end up in the same place as any ancillary files.

## Recommended notebook and tutorial structure

It's recommended that Jupyter notebooks and STScI tutorials use the following
suggested structure:

- [Title](#title)
- [Table of Contents](#table-of-contents)
- [Learning Goals](#learning-goals)
- [Imports](#imports)
- [Introduction](#introduction)
- [Loading Data](#loading-data)
- [File Information](#file-information)
- [Sections](#sections) (xN)
- [Exercises](#exercises) (encouraged)
- [Additional Resources](#additional-resources) (optional)
- [About this Notebook](#about-this-notebook)
- [Citations](#citations)
- [Footer](#footer)

### Title

Pick a clear, descriptive title. For titles, use [title case
capitalization](https://docs.astropy.org/en/latest/development/style-guide.html#capitalization)
and the following [Markdown
syntax](https://www.markdownguide.org/basic-syntax/#headings):

```
# My Clear, Descriptive Title

```

### Table of Contents

Provide a table of contents to help readers quickly navigate the sections of
your notebook. Use the following [Markdown
syntax](https://www.markdownguide.org/basic-syntax/):

```
## Table of Contents
- [My First Section](#my-first-section)
- [My Second Section](#my-second-section)
- [My Third Section](#my-third-section)

```

### Learning Goals

Every notebook should contain three to five explicit learning goals. A learning
goal should describe what a reader should know or be able to do by the end of
the notebook that they didn't know or couldn't do before.

Learning goals can be broken down into:

- Skills: what the reader should be able to do by the end of the notebook.
- Knowledge: what the reader should understand by the end of the notebook.
- Attitudes: what the reader's opinions about the subject matter will be by the
  end of the notebook.

To create your notebook learning goals, write sentences that identify what
skill, knowledge, or attitude the reader will gain from your notebook, and use
strong action verbs (understand, explain, demonstrate, determine, create,
access, calculate, analyze).

Template learning goal sentence:

By the end of this notebook, you will be able to (skill) in order to
(knowledge).

#### Examples

```
By the end of this tutorial, you will:

- Understand how to use aperture photometry to turn a series of two-dimensional
  images into a one-dimensional time series.
- Be able to determine the most useful aperture for photometry on a *Kepler/K2*
  target.
- Create your own light curve for a single quarter/campaign of *Kepler/K2* data.

```

```
By the end of this tutorial, you will be able to:

- Understand how NASA's Kepler Mission collected and released light curve data
  products.
- Download and plot light curve files from the data archive using `Lightkurve`.
- Access light curve metadata.
- Understand the time and brightness units.

```

### Imports

Import your dependencies near the top of the notebook and explain why you're
including each one. For example:

![Imports](images/imports.png)

### Introduction

Write a short introduction explaining the purpose of the notebook. If there are
background materials or resources that may be useful to the reader to provide
additional context, you may link to it here.

#### Defining Terms

As a subsection of your introduction section, define any terms or common
acronyms that your audience may not know. If you're using some kind of
domain-specific astronomical symbol or unusual mathematical concept, make sure
you define it (for example, in its mathematical form) and link to any
definitions (from literature, Wikipedia, etc.).

#### Companion Content

As a subsection of your introduction section, link to any helpful companion
content. For example, if your notebook is a continuation from another notebook,
or there are other notebooks that would be useful for the reader to read before
or after your notebook, mention that here.

### Loading Data

.. should Loading Data and File Information go right after Imports? It seems
   funny to have the Introduction separated from the main content by these two
   sections, and might look cleaner to have all of the importing/downloading
   info together before the learning material starts, but is there a specific
   reason for this order?

If the user needs to download data to run the tutorial properly, where possible,
use [Astroquery](https://astroquery.readthedocs.io/en/latest/) (or similar) to
retrieve files. If this is not possible, see the [data
guide](where-to-put-your-data.md) for other options.

### File Information

Explain pertinent details about the file you've just downloaded. For example, if
working with Kepler light curves, explain what's in the different file
extensions:

```
- No. 0 (Primary): This HDU contains metadata related to the entire file.
- No. 1 (Light curve): This HDU contains a binary table that holds data like
  flux measurements and times. We will extract information from here when we
  define the parameters for the light curve plot.
- No. 2 (Aperture): This HDU contains the image extension with data collected
  from the aperture. We will also use this to display a bitmask plot that
  visually represents the optimal aperture used to create the SAP_FLUX column in
  HDU1.

```

Where possible (if the code supports it), use code examples that use Jupyter to
*show* what's in the data. For example, if you are showing an object that can be
read as an Astropy table, display the table:

![show-data-example](images/notebook_table_data_example.png)

### Sections

The main content of your notebook should be subdivided into numbered sections
with useful names that make sense based on the content. Break sections up with
standard [Markdown syntax
headings](https://www.markdownguide.org/basic-syntax/#headings):

```
## Section 1

Intro to Section 1

### Subsection 1a

More detailed info about Section 1

## Section 2

A complete thought that's as important as Section 1 but doesn't need
subsections.

```

Be sure to use the Markdown headings (that is, the number of `#`'s) in a way
that gives hierarchical meaning to your document. The header levels are used to
do things like intelligently make links, so you don't want to confuse things by
using header levels that don't match the logical flow of the document.

### Exercises

Most notebooks convey information to their reader by posing questions and then
providing answers. But as so many of us learn by doing, it's encouraged for you
to provide exercises in your notebook that pose questions but do not include
immediate answers so that your reader can practice their new skills to cement
the knowledge in their minds.

If you do have one or more exercises, be sure to leave a blank code cell
underneath to show the reader that they're meant to try out their new skill
right there. You may also want to include a "solutions" notebook next to your
main notebook for the reader to check their work after they have finished their
attempt.

### Additional Resources

While this section isn't always necessary, sometimes you want to provide more
resources for the reader who wants to learn something beyond what's in the
notebook. Sometimes these resources don't exist, but if they do, it's good to
put them at the end of your notebook to give the reader somewhere else to go.
Usually a list of links using Markdown bullet list plus link format is
appropriate:

```
* [A neat resource to learn
  more](http://learning.org/how-i-learned-science-is-cool.html)
* [A place to find more relevant code](https://github.com/spacetelescope/jwst)

```

### About this Notebook

Let the world know who the author of this great notebook is! If possible and
appropriate, include a contact email address for users who might need support
(for example, `archive@stsci.edu`). You can also optionally include keywords or
your funding source in this section.

### Citations

.. what else needs to go in this section?

Provide your reader with guidelines on how to cite open source software and
other resources in their own published work.

#### Example

```
If you use `astropy` or `lightkurve` for published research, please cite the
authors. Follow these links for more information about citing `astropy` and
`lightkurve`:

* [Citing `astropy`](https://www.astropy.org/acknowledging.html)
* [Citing `lightkurve`](http://docs.lightkurve.org/about/citing.html)

```

### Footer

Notebooks should use the standard STScI footer:

![Footer](images/footer.png)

This can be implemented with the following code snippet placed in a cell at the
bottom of the notebook.

```
<img style="float: right;" src="https://raw.githubusercontent.com/spacetelescope/notebooks/master/assets/stsci_pri_combo_mark_horizonal_white_bkgd.png" alt="Space Telescope Logo" width="200px"/>

```

The [example notebook](../templates/example_notebook.ipynb) implements the
footer in this way.

## Further reading

STScI follows [The Chicago Manual of
Style](https://www.chicagomanualofstyle.org/home.html) and the [Merriam-Webster
Dictionary](https://www.merriam-webster.com/). STScI writing guidelines are in
agreement with the [Astropy Narrative Style
Guide](https://docs.astropy.org/en/stable/development/style-guide.html).

### Example notebooks following this style guide

Here are some example notebooks that follow this style guide:

- [An example notebook with placeholder content](../templates/example_notebook.ipynb)
- [Kepler Full Frame Images (FFI)](https://github.com/spacetelescope/notebooks/blob/master/notebooks/MAST/Kepler/Kepler_FFI/kepler_ffi.ipynb)
- [Kepler Light Curves](https://github.com/spacetelescope/notebooks/blob/master/notebooks/MAST/Kepler/Kepler_Lightcurve/kepler_lightcurve.ipynb)
- [Kepler Target Pixel Files (TPF)](https://github.com/spacetelescope/notebooks/blob/master/notebooks/MAST/Kepler/Kepler_FFI/kepler_ffi.ipynb)

### Other Resources

For a related view of notebook styles in the astronomy context, see the [Astropy
Contributing Guide](http://learn.astropy.org/contributing.html).

### Contributing to the STScI notebooks repository

View the contributing guide in the [STScI notebooks repository](https://github.com/spacetelescope/notebooks).
