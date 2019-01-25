# GitHub Pull Request Review ("Code Review")

### Introduction
A code review for a GitHub Pull Request (PR) could mean many things but likely
it should consist of a few steps that are beyond just "reviewing the code".
Below are some steps that could be used as a guide in the review of a PR.
Remember, too, that a review should respectful and that we have 
a [Code of Conduct](https://github.com/spacetelescope/style-guides/blob/master/CODE_OF_CONDUCT.md) 
that we follow.

### Who Should Review
There are several groups of people who might be interested in reviewing a PR.
Members of the development team, package maintainer (GitHub repository
maintainer), or scientists / product owners.  Each group may have a different
focus on the steps below, for example:

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


### PR Review Steps

#### 1. Review the PR and Issue

Read through the PR to see what was done and the discussion people have had on
there. Confirm the GitHub "fixes" or "closes" magic words are used to
automatically close the related issues.

Then, if there is a corresponding issue or issues, read through the original
content of the issue and then read through the discussion and clarification.

Many times there are going to be other "requirements" or clarifications that 
might influence the PR beyond the original issue text.

If the PR does not appear to fix/implement the issue or the description is not
clear in the PR text, then stop here and ask questions. Document any new
understanding or thoughts in the PR for future reference.

#### 2. Review the code

Look through the code and see if it appears to answer the question laid out by
the PR and Issue text. It should answer only the issue and not other bugs etc.
Other ones should be a in a separate PR (which will make it easier for review,
and bisecting if a bug is added and found later on). 

The code should also be reviewed for style, maintainability, and to make sure
it fits into the framework of the code in the GitHub repository. It would be
better to maintain similarity to current code unless there is a need to make
changes to the repository code framework.  Automated PEP 8 style checking
should happen as part of the CI / automated test infrastructure.  (ref:
https://github.com/spacetelescope/style-guides/issues/73)

If the code appears to have bugs or implementation issues, then stop now and
ask for clarification from the author (and maybe get them to make code
changes).

#### 3. Check and Run the tests

1. Confirm the tests are running and passing in the CI. Be mindful that green
   check marks can be false positives, so read the logs to be sure before
   merge. Did you check the jobs that are allowed to fail? Are they failing for
   the right things?

2. Are other tests needed based on new functionality OR to check for the bug
   from the issue. 

If the tests do not run or fail, well, definitely stop now and get the author
to fix the code so the tests pass.  

If more tests are needed, request them from the author.

#### 4. Review the documentation

Read through the relevant portion of the documentation to see if it is
sufficient or correct given the changes in the PR.  Does more documentation
need to be added? Does the existing documentation need to be modified based on
the PR? Can I come back in 6 months and understand how things work from this
documentation? Can a new user or maintainer understand it?

Is a change log entry necessary? If yes, was it included in the right place?


#### 5. Run the branch locally

Fetch the branch from GitHub and run the tests locally. If there are GUI
changes, test the GUI related to the code changes and confirm any science
results that are not automatically confirmed by automated test.  Running the
code locally is important!  

To see how Cubeviz implements this step, please see [Procedure for testing open
github
PRs](https://innerspace.stsci.edu/display/~ddavella/Procedure+for+testing+open+github+PRs).
