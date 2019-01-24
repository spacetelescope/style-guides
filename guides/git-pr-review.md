# Github PR Review ("Code Review")

### Introduction
A code review for a Github PR could mean many things but likely it should consist of a few steps that are beyond just "reviewing the code". Below are some steps that could be used as a guide in the review of a PR. Remember, too, that a review should respectful and that we have a [Code of Conduct](https://github.com/spacetelescope/style-guides/blob/master/CODE_OF_CONDUCT.md) that we follow.

### Audience
There are several groups of people who might be interested in reviewing a PR.

For example, members of the development team, package maintainer (Github repo maintainer), or scientists / product owners.  Each group may have a different focus on the steps below.  For example, the development team might focus more on Steps 2 and 3 (obviously with knowledge of Step 1).

A scientist, content specialist or product owner might focus more on Steps 4 and 5, so they can confirm that the changes make sense from a science point-of-view. 

The package maintainer / Github repo maintainer will want to make sure that each of these steps are done but may not need to be the one doing each step. The package maintainer will be the one who will want to confirm the code to be merged fits the framework of existing code, is correct and the documentation and tests are sufficient. 


### PR Review Steps

#### 1. Review the PR and Issue

Read through the PR to see what was done and the discussion people have had on there.

Then, if there is a corresponding issue or issues, read through the original content of the issue and then read through the discussion and clarification.

Many times there are going to be other "requirements" or clarifications that my influence the PR beyond the original issue text.

If the PR does not appear to fix/implement the issue, then stop here and ask questions.

#### 2. Review the code

Look through the code and see if it appears to answer the question laid out by the PR and Issue text. It should answer only the issue and not other bugs etc. Other ones should be a in a separate PR (which will make it easier for review, and bisecting if a bug is added and found later on). 

The code should also be reviewed for style and to make sure it fits into the framework of the code in the Github repo. It would be better to maintain similarity to current code unless there is a need to make changes to the repo code framework.  Automated PEP8 style checking should happen as part of the CI / automated test infrastructure.  (ref: https://github.com/spacetelescope/style-guides/issues/73)

If the code appears to have bugs or implementation issues, then stop now and ask for clarification from the writer (and maybe get them to make code changes).

#### 3. Check and Run the tests

1. Confirm the tests are running in the CI.
1. Are other tests needed based on new functionality OR to check for the bug from the issue. 

If the tests do not run, well, definitely stop now and get them to fix the code so the tests run.  

If more tests are needed, request more tests from the PR author.

#### 4. Review the documentation

Read through the relevant portion of the documentation to see if it is sufficient given the changes in the PR.  Does more documentation need to be added? Does the existing documentation need to be modified based on the PR?

Confirm entries in the CHANGELOG were included.

#### 5. Run the branch locally

Fetch the branch from github and run the tests locally. If there are GUI changes, test the GUI related to the code changes and confirm any science results that are not automatically confirmed by automated test.  Running the code locally is important!  For more information see [Procedure for testing open github PRs](https://innerspace.stsci.edu/display/~ddavella/Procedure+for+testing+open+github+PRs).
