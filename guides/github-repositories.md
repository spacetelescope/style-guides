# GitHub Repositories

STScI-owned repositories on GitHub have conventions for the following:

- [Naming](#naming)
- [Repository descriptions](#repository-descriptions)
- [Repository website](#repository-website)
- [README](#readmemd)
- [Repository tags/topics](#tagstopics)
- [Maintainers](#maintainers)
- [License](#license)
- [Code of conduct](#code-of-conduct)
- [Contributor guidelines](#contributor-guidelines)
- [Releases](#releases)
- [Status](#status)

## Conventions

### Naming

Pick a logical, ideally human-readable name. Hyphens are preferred over underscores e.g. `style-guides` not `style_guides`

### Repository descriptions

Repository descriptions should be short (ideally 20 words or less) but descriptive. See [asdf](https://github.com/spacetelescope/asdf) as a good example:

> ASDF (Advanced Scientific Data Format) is a next generation interchange format for scientific data

### Repository website

The website field should link to the software documentation or user guides. Basically wherever you think users of the software should go first.

<img width="987" alt="screen shot 2018-05-04 at 9 23 13 pm" src="https://user-images.githubusercontent.com/4483/39658373-735f7614-4fe1-11e8-87d3-4debdde0cc90.png">

### README.md

Readme should include short statement of need. What is this software, what does it do? The README should also link out to other resources in the repository such as contributing guidelines, who the maintainers are, the code of conduct etc.

### Repository tags/topics

Repository [topics](https://help.github.com/articles/about-topics/) are used to index and search our repositories on the `stsci.edu` website. Mission and instrument abbreviations are preferred to full names e.g. `hst` not `hubble space telescope` and `cos` not `cosmic origins spectrograph`.

### Maintainers

We follow GitHub's `CODEOWNERS` convention: https://help.github.com/articles/about-codeowners/

```
# Lines starting with '#' are comments.
# Each line is a file pattern followed by one or more owners.

# These owners will be the default owners for everything in the repo.
*       @arfon

# Order is important. The last matching pattern has the most precedence.
# So if a pull request only touches javascript files, only these owners
# will be requested to review.
*.js    @octocat @github/js

# You can also use email addresses if you prefer.
docs/*  docs@example.com

```

### License

BSD 3-Clause is the preferred license for STScI produced open source software. A plain text file named `LICENSE` should be at the top level of the repository.

The copyright holder should be 'Association of Universities for Research in Astronomy'. [Example license](https://github.com/spacetelescope/style-guides/blob/f786ccd397c48ec4b76c0bdae96aad86a5e4e4a5/templates/LICENSE)

### Code of conduct

We expect all STScI projects to adopt a code of conduct that ensures a productive, respectful environment for all open source contributors and participants. A file named `CODE_OF_CONDUCT.md` should be at the top level of the repository and should have the following content: [Example code of conduct](https://github.com/spacetelescope/style-guides/blob/75d52647344f85527d9b60b6bf38bde46d30e2b2/templates/CODE_OF_CONDUCT.md)

### Contributor guidelines

When possible, include contributing guidelines in your repository either as a section named `Contributing` in the README or in a dedicated file named `CONTRIBUTING.md`

### Issue and PR Templates

Each repository should have an Issue Template and a PR Template. To include
these, copy the templates out of [style guide template
directory](https://github.com/spacetelescope/style-guides/tree/master/templates),
place into the root of the GitHub repository and call the files
ISSUE_TEMPLATE and PULL_REQUEST_TEMPLATE.  

More information is found on the [GitHub
website](https://help.github.com/articles/about-issue-and-pull-request-templates/)

### Releases

Where possible use [GitHub releases](https://help.github.com/articles/creating-releases/) following [SemVer](https://semver.org/) conventions.

### Status

Actively maintained or not? If not, mark your repository as [archived](https://help.github.com/articles/archiving-repositories/).
