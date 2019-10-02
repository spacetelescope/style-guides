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

1. Box: the institute uses Box as a file-sharing solution, but when used with
   static links it is also a useful (and ITSD-sanctioned) solution for
   intermediate data storage. The data may be shared (including for automated 
   tests) by creating a direct static link to the data file. Detailed 
   instructions for this are below.
2. Artifactory: this is an enterprise-level data storage solution that STScI has
   bought into.  You may want to use this for larger datasets for functional work 
   that is used by STScI but not external users.
3. Zenodo: this is a data store intended for academic users, and includes
   fully-citable DOIs for arbitrary data sets. Such data sets are versioned and
   accessible on the public internet, and therefore are well-suited for data of
   interest to astronomy researchers or other public audiences.

There are a variety of other possibilities (e.g. `git lfs` + cloud storage),
but in the name of keeping uniformity at the institute, the above are the
primary recommendations.


#### Obtaining Static Box Links

Note that if you are making a static link publicly visible, it is best practice
to use a shared folder that is managed by a shared data account rather than
being tied to your user account.  This is also helpful to prevent these files
from counting against your account's size restrictions.  Talk to ITSD or your 
team to see what, if any shared Box folder you might have.

1. Go to the box web page for a box folder 
   (`https://stsci.app.box.com/folder/######`) and upload your file.
2. Click on the "Share" button for the file you just uploaded.
3. In the dropdown, change the permissions to "People with the link" (this may be default depending on your settings)
4. Click on the "gear" icon by "Shared Link" (or the gear might be "Link Settings" depending on your browser)
5. Make sure "Allow Download" is checked
6. You should see a "Direct Link" field at the bottom of the panel. This is the
   link to the file. You can use this directly if desired, but it is rather opaque and thus can be off-putting or alarming to users.  Hence, it is recommended to use the more intuitive URL that can be obtained using the instructions below.


#### Human-Friendly URLs to public Box files
DMD maintains a redirector API to retrieve files from Box using a more intuitive URL than the default. The link is constructed as follows:

1. The base of the URL is https://data.science.stsci.edu/redirect
2. Each nested folder within the `DMD_Managed_Data` folder is a path added to the URL. You can see the nested path at the top of the Box web interface, underneath the Box search bar.
3. The final path is the name of the file you uploaded. 

For example, https://data.science.stsci.edu/redirect/JWST/jwst-data_analysis_tools/README.txt


## Python

In the context of Python (currently the language of much of the public code and
most of the notebooks at STScI), more specific guidelines apply - the following
is recommended to decide how to make data accessible in a Python project:

1. Use MAST, preferably through [`astroquery.mast`](https://astroquery.readthedocs.io/en/latest/mast/mast.html).
   This should be used even if an intermediate data product is actually needed -
   e.g., download single-exposures from MAST and combine them using drizzle
   instead of providing a custom drizzled image and serving that (as long as it
   isn't computationally infeasible).  This is more reproducible and
   understandable to the end-user.

2. If the dataset is not on MAST but is on some other standardized public
   astronomy source:

   a. Use [`astroquery`](https://astroquery.readthedocs.io/en/latest/) if the
      data are available there.

   b. Otherwise, use a VO protocol if it is available (using astroquery's VO
      interfaces, [PyVO](https://pyvo.readthedocs.io/en/latest/), or similar
      standard packages - do *not* roll your own reader unless no other way
      exists.

3. Otherwise for public data, choose follow the guidelines above in "Storage
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
