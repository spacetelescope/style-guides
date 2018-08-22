# Storing data for code and notebooks

This guide describes best practices for where to store data needed for
STScI-supported code and/or notebooks. Note that this is a fairly narrow
definition of "stored data", so does *not* cover things like research notes,
personnel records,  archival observational data (except for that which is
specifically required for code), etc.

## Language-agnostic guidelines

As a rule, data should only be distributed with code or associated with a
specific notebook *if there is no other location for the data*.  E.g., if it is
a dataset in MAST, or which can be derived easily from raw data in MAST, the
code/notebook should use the MAST API instead of copying the data into the
repository.  Similarly, reference files for STScI-supported instruments should
always use CRDS when possible.

The exception to this rule is for unit tests. Unit tests often require small
*example* datasets, which are best stored as close as possible to the tests.
These datasets should never be large enough to present a bandwidth problem,
however. A good rule of thumb is a handful of files each <~ 100 kB (i.e.,
comparable to a large but not ridiculously so source code file).

Of course plenty of data are not in archives due to a narrow application domain
or limitations of data access rights. In that case, the best location and access
method depend on the data set size and programming language.  From here on we
focus on datasets that are realistic to store in individual or small collections
of files, so e.g. databases are beyond the scope of this guide.

### Storage location/platform for non-archived datasets

There are a few options for where you store such, depending on the data size and
the audience for the code/notebook:

1. Artifactory: this is an enterprise-level data storage solution that STScI has
   bought into.  Use this for larger datasets for functional work that is
   primarily used by STScI but not external users.
2. Zenodo: this is a data store intended for academic users, and includes
   fully-citable DOIs for arbitrary data sets. Such data sets are versioned and
   accessible on the public internet, and therefore are well-suited for data of
   interest to astronomy researchers or other public audiences.
3. `git lfs` + cloud storage: Git LFS is an extension that allows files in `git`
   to be stored outside the code repository (e.g. in a cloud service like Amazon
   S3)., but still have links to those files along side the code. This is a good
   choice for projects where the above options do not fit, but this comes with
   added management overhead and actual dollar cost that is the responsibility
   of the code/notebook maintainer.


## Python

In the context of Python (currently the language of much of the public code and
most of the notebooks at STScI), more specific guidelines apply - the following
is recommended to decide how to make data accessible in a Python project:

1. Use MAST, preferably through [`astroquery.mast`](https://astroquery.readthedocs.io/en/latest/mast/mast.html).
   This should be used even if an intermediate data product is actually needed -
   e.g., download single-exposures from MAST and combine them using drizzle
   instead of providing a custom drizzled image and serving that (as long as it
   isn't computationally infesible).  This is more reproducible and
   understandable to the end-user.

2. If the dataset is not on MAST but is on some other standardized public
   astronomy source:

   a. Use [`astroquery`](https://astroquery.readthedocs.io/en/latest/) if the
      data are available there.

   b. Otherwise, use a VO protocol if it is available (using astroquery's VO
      interfaces, [PyVO](https://pyvo.readthedocs.io/en/latest/), or similar
      standard packages - do *not* roll your own reader unless no other way
      exists.

3. Otherwise for public data, choose follow the guielines above in "Storage
   location/platform for non-archived datasets" and download from public URLs
   using:

   a. The
      [`astropy.utils.data.download_file`](http://docs.astropy.org/en/stable/api/astropy.utils.data.download_file.html)
      function for single-url downloads if `astropy` is already used in the
      code/notebook,

   b. The Python standard library
      [`urllib.request`](https://docs.python.org/3/library/urllib.request.html)
      if `download_file` does not meet the use case,

   c. Or the [`requests`](http://docs.python-requests.org/en/master/) package if
      the request is more complex than a `urllib` easily supports (requires
      authentication, POST, OAuth, etc).

4. Finally, if an STScI-specific data storage platform or private data is needed:

   a. If a specific Python package is available for that platform, use it (e.g.
      the `crds` python package for HST reference files in CRDS).

   b. Otherwise, follow the guidelines in 3, but using internally-accessible
      URLs (e.g. ``file://`` URLs that point to central store paths).

For case 3a/b or 4b, urls should be constructed using the standard library
[`urllib.parse`](https://docs.python.org/3/library/urllib.parse.html#module-urllib.parse)
functions where possible, not hand-constructed with string operations.
