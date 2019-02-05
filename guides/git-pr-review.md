# GitHub Pull Request Review ("Code Review") 

### Introduction
Ideally all code produced at the institute is reviewed by at least one other
staff member.  This is a standard practice in the software industry, as code
(like all writing) benefits greatly from editing and multiple perspectives.  At
STScI it also has the potential added advantage of facilitating communication
across disparate divisions or buildings (e.g. scientists and engineers). While
there are a variety of tools for this process, the broad availability and
adoption of GitHub has rendered it the preferred method in science, the open
source community, and much of STScI.  Hence this guide on code review is
oriented around the GitHub Pull Request model, but note that the guidelines
here are generally transferrable to a variety of similar tools.

A code review for a GitHub Pull Request (PR) could mean many things but it
should consist of a few steps that are beyond just "reviewing the code".  Below
are some additional steps that should be taken as part of the process of
reviewing a PR. Note that not all steps are necessarily relevant depending on
exactly what has been set up in a GitHub repository.  But in most cases this
process is the goal, as this will help improve maintainability, code quality,
and science applicability.  Remember, too, that a review should be respectful
and abide by our [Code of
Conduct](https://github.com/spacetelescope/style-guides/blob/master/CODE_OF_CONDUCT.md)
that we follow.

### Who Should Review

There are several groups of people who might be interested in reviewing a PR.
Members of the development team, package maintainers (GitHub repository
maintainers), scientists at the institute / product owners, or external
contributors (often but not always scientists).  Each group may have a
different focus on the steps below, for example:

* The development team might focus more on Steps 2 and 3 (obviously with
  knowledge of Step 1).

* A scientist, content specialist, or product owner might focus more on Steps 4
  and 5, so they can confirm that the changes make sense from a science
  point-of-view. 

* The package maintainer / GitHub repository maintainer will want to make sure
  that each of these steps are done but may not need to be the one doing each
  step.  The package maintainer will be the one who will want to confirm the code
  to be merged fits the framework of existing code, is correct and the
  documentation and tests are sufficient. 

Note also that following this guide helps contributors as much as reviewers.  A
clear understanding of the process makes it easier for contributions to be
reviewed on reasonable timescales and with a clear understanding of the
responsibility. Put another way: code review is a social contract and we must
agree on how it works for it to work well!

### PR Review Steps

#### 1. Review the PR and Issue

Read through the comments in the PR to see what was done and the discussion
people have had on there. Confirm the GitHub "fixes" or "closes" magic words
are used to automatically close the related issues.  (For example, if the text
`fixes #42` appears in the description of the issue or the commit log, merging
the PR will close issue #42.  For more see [the GitHub documentation on this
topic](https://help.github.com/articles/closing-issues-using-keywords/))

Then, if there is a corresponding GitHub issue or issues, read through the
original content of the issue and then read through the discussion and
clarification.

Many times there are going to be other "requirements" or clarifications that 
might influence the PR beyond the original issue text.

If the PR does not appear to fix/implement the issue or the description is not
clear in the PR text, then stop here and ask questions. Document any new
understanding or thoughts in the PR for future reference.

#### 2. Review the code

Look through the code and see if it addresses the items in the PR and any
related issue comments.  It should answer only the issue and not other bugs
etc.  Other ones should be a in a separate PR (which will make it easier for
review, and bisecting if a bug is added and found later on). 

The code should also be reviewed for style, maintainability, and to make sure
it fits into the framework of the code in the GitHub repository. It would be
better to maintain similarity to current code unless there is a need to make
changes to the repository code framework.  Python code should follow the
[Python style
guide](https://github.com/spacetelescope/style-guides/blob/master/guides/python.md).

In particular, check that the code follows the PEP8 style (within the
guidelines outlined in the above style guide).  Where possible, such style
checking is best implemented using CI / automated test infrastructure for a
first pass, with the reviewer just checking that it makes sense.  This is not
always possible, in which case it is the reviewer's responsibility to whatever
extent is practical.

If the code appears to have bugs or implementation issues, then stop now and
ask for clarification from the author (and maybe get them to make code
changes). Where possible, use in-line comments in the GitHub "Files changed"
interface to give the PR author specific and actionable suggestions.

#### 3. Check and Run the tests

Note: all packages following these style guides should use continuous
integration (CI) as thoroughly as practical to simplify the testing process.
On GitHub/public projects, this usually means Travis-CI, whereas internal
projects use Jenkins.  In general these both address the same core problem:
automatically running unit tests to ensure the changes a PR introduces do not
break code. For more details, see [a forthcoming CI style
guide](https://github.com/spacetelescope/style-guides/issues/8).

1. Confirm the tests are running and passing in the CI. Be mindful that green
   check marks can be false positives, so it's often wise to read some of the
   logs to be sure before merge. Did you check the jobs that are allowed to fail?
   Are they failing for the right reasons?

2. Are other tests needed based on new functionality OR to check for the bug
   from the issue? If so they need to be included in the same PR as the code.

If the tests do not run or fail, well, definitely stop now and get the author
to fix the code so the tests pass.  

If more tests are needed, request them from the author.

#### 4. Review the documentation

Read through the relevant portion of the documentation to see if it is
sufficient or correct given the changes in the PR.  Does more documentation
need to be added? Does the existing documentation need to be modified based on
the PR? Can I come back in 6 months and understand how things work from this
documentation? Can a new user or maintainer understand it?

If the code is in Python, are there docstrings for all new classes and
functions?  Do they follow the style expected in the package and appear in any
built documentation? (Not all packages require this, but any that do you must
ensure they are included and correct.) If it is not Python, is the correct API
documentation format followed?`

Is a change log entry necessary? If yes, was it included in the right place?


#### 5. Run the branch locally

Local testing is not always necessary if you are confident that the changes
the PR introduced are *thoroughly* tested via the CI tests (either via new tests
in the PR or existing tests). However, some features, like GUI elements, are
not easily tested via CI, or CI may not yet be set up in the package you are
working on.  In that case you must run tests locally. To do this you will need
to fetch the branch from GitHub, merge it with master in a local testing
branch, and run the tests (manual or automatically) yourself.

If there are GUI changes, test the GUI related to the code changes and confirm
any science results that are not automatically confirmed by automated test.
Running the code locally is important!  

To see how Cubeviz implements this step, please see [Procedure for testing open
github
PRs](https://innerspace.stsci.edu/display/~ddavella/Procedure+for+testing+open+github+PRs).
