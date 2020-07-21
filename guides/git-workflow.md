# Workflow for contributing to a Git / GitHub repository in the `spacetelescope` organization

# Table of contents
1. [Introduction](#introduction)
2. [Which workflow should I use?](#which-workflow)
    1. [Branching Workflow](#branching-pros-cons)
    2. [Forking Workflow](#forking-pros-cons)
3. [Workflow Instructions](#workflow-instructions)
    1. [Opening a GitHub Issue](#github-issue)
    2. [Branching Workflow](#branching-workflow)
        1. [Keeping your branch updated](#update-branch)
    3. [Forking Workflow](#forking-workflow)
        1. [Keeping your fork updated](#update-fork)
        2. [Collaborating on someone else's fork](#collaborate-on-fork)


## Introduction <a name="introduction"></a>
The following describes two different workflows for contributing software to an open-source repository on GitHub under the `spacetelescope` organization (hereby referred to as the "`spacetelescope` repository"). One model involves creating branches off of the `spacetelescope` repository directly (hereby referred to as the "branching workflow"), while the other involves forking the `spacetelescope` repository and developing changes on personal branches of the fork (hereby referred to as the "forking workflow").  Regardless of which model is chosen, software changes ultimately get incorporated into the `spacetelescope` repository through pull requests.  Below, we provide some pros/cons for each workflow.  We encourage developers to chose a workflow that is most appropriate for your repository.

## Which workflow should I use? <a name="which-workflow"></a>
There are various pros and cons to each workflow, and it may not be obvious as to which workflow is best to choose for your project.  However, the power and benefit of the workflow comes in its consistent use across all team members and collaborators during the development of the software.  Provided below are a few things to consider when choosing a particular workflow.

### Branching Workflow <a name="branching-pros-cons"></a>

Under the branching workflow:

- Any team member with write access to the repository can collaborate directly in adding code, revising, or testing a branch.
- Continuous integration tools such as Travis or Jenkins can be enabled for all branches of the repository in a single location, so new feature branches can automatically be tested when a pull request is opened or modified.
- It is not possible for external collaborators to contribute unless they explicitly have write access to the `spacetelescope` repository, and when external collaborators are required, repositories must adhere to [ITSD policies](https://innerspace.stsci.edu/pages/viewpage.action?spaceKey=isec&title=Source+Code+Control).
- It is easier to accidentally delete or overwrite the work of others who are also working on the same branch, or happen to have their own branch of the same name.  While it is possible to recover this work or to restrict the editing of specific branches to only select developers, it can sometimes be a painstaking process.
- Repositories may accrue a fair amount of stale branches over time, which may become burdensome for developers, and may cause collaborators to infer that the project is not being maintained.


### Forking Workflow <a name="forking-pros-cons"></a>

A forking workflow:

- Promotes "open development," wherein potential external collaborators who are interested in contributing may do so despite not having write access to the `spacetelescope` repository.
- Provides a consistent experience for everyone involved in the project, whether you are an internal or an external collaborator.
- Allows for an explicit namespace in pull requests, merge commits, and repository history that makes it more apparent who "owns" which branch (e.g. "Merge pull request #1 from ``<username>/<branch_name>``).
- Allows for contributors to experiment with various repository settings and/or continuous integrations tools without affecting how the `spacetelescope` repository operates.
- Requires extra knowledge of how to work with multiple remotes in order to keep up-to-date with the parent, 'upstream' repository, or to collaborate on someone else's fork.
- Requries that one must be explicitly added as a collaborator to someone's personal fork for direct contributions to that fork, if the person is not already a maintainer of the `spacetelescope` repository.
- May result in a substantial and burdensome amount of notifications if forks are of a private repository, since everyone watching the parent repository are automatically subscribed to all of the fork activity by default.


## Workflow Instructions <a name="workflow-instructions"></a>

Below we provide step-by-step instructions on how to employ each workflow.

### Opening a GitHub Issue <a name="github-issue"></a>
Regardless of which workflow you chose, before contributing, you should determine if the eventual change is significant enough to warrant a GitHub issue; if you think that this change will be solving a significant problem or providing a significant enhancement to the project, then it would be advantageous to open an issue under the `spacetelescope` repository. This will allow both contributors and repository maintainers to keep track of the project and capture any needed context of the particular change. Any appropriate individuals should be assigned to the issue, and any appropriate label(s) should be tagged.

Meanwhile, for trivial fixes (e.g., typos), a pull request without an accompanying issue would be acceptable.

### The Branching Workflow <a name="branching-workflow"></a>

1. Make a local copy of the `spacetelescope` repository by cloning the repository (e.g. `git clone https://github.com/username/repository_name.git`).  Note that, unless you explicitly delete your clone of the repository, this only has to be done once.

2. Ensure that your clone is pointing to the `spacetelescope` remote repository with `git remote -v`.  You should see that `origin` corresponds to the URL from step (1).  If it doesn't, you can add it with `git remote add origin https://github.com/username/repository_name.git`.

3. Create a branch on the clone to develop software changes on.  Branch names should be short but descriptive (e.g. `new-database-table` or `fix-ingest-algorithm`), and not too generic (e.g. `<username>-bug-fix`).  Consistent use of hyphens is encouraged.
    1. `git branch <branchname>`
    2. `git checkout <branchname>` - you can use this command to switch back and forth between existing branches.
    3. Perform local software changes using the nominal `git add`/`git commit -m` cycle:
       1. `git status` -  allows you to see which files have changed.
       2. `git add <new or changed files you want to commit>`
       3. `git commit -m 'Explanation of changes you've done with these files'`

4. Push the branch to the `spacetelescope` repository with `git push origin <branchname>`.

5. In the `spacetelescope` repository, create a pull request for the recently pushed branch.  You will want to set the "base" option to point to the branch you would like to merge changes into (e.g. `master`), and the "compare" option to point to the new branch (i.e. `<branchname>`).  Note that if the branch is still under development, you can use the GitHub "Draft" feature (under the "Reviewers" section) to tag the pull request as a draft. Not until the "Ready for review" button at the bottom of the pull request is explicitly pushed is the pull request 'mergeable'.

6. Assign the pull request a reviewer, selecting a maintainer of the `spacetelescope` repository.  They will review your pull request and either accept the request and merge, or ask for additional changes.

7. Iterate with your reviewer(s) on additional changes if necessary, addressing any comments on your pull request.  If changes are required, you may end up iterating over steps 3.ii, 3.iii and 4 several times while working with your reviewer.

8. Once the pull request has been accepted and merged, you can delete your local branch with `git branch -d <branchname>`.

#### Keeping your branch updated <a name="update-branch"></a>
If you wish to, you can keep a branch up-to-date with the latest version of the `spacetelescope` repository by checking out and merging in changes from the `master` branch:
1. `git checkout master`
2. `git pull origin master`
3. `git checkout <branchname>`
3. `git merge master`


### The Forking Workflow <a name="forking-workflow"></a>

1. Create a personal fork of the `spacetelescope` repository by visiting its location on GitHub and clicking the `Fork` button.  This will create a copy of the `spacetelescope` repository under your personal GitHub account (hereby referred to as "personal fork").  Note that this only has to be done once.

2. Make a local copy of your personal fork by cloning the repository (e.g. `git clone https://github.com/username/repository_name.git`, found by clicking the green "clone or download" button.).  Note that, unless you explicitly delete your clone of the fork, this only has to be done once.

3. Ensure that the personal fork is pointing to the `upstream` `spacetelescope` repository with `git remote add upstream https://github.com/spacetelescope/repository_name.git`.  Note that, unless you explicitly change the remote location of the repository, this only has to be done once.

4. Create a branch on the personal clone to develop software changes on. Branch names should be short but descriptive (e.g. `new-database-table` or `fix-ingest-algorithm`), and not too generic (e.g. `bug-fix`).  Consistent use of hyphens is encouraged.
    1. `git branch <branchname>`
    2. `git checkout <branchname>` - you can use this command to switch back and forth between existing branches.
    3. Perform local software changes using the nominal `git add`/`git commit -m` cycle:
       1. `git status` -  allows you to see which files have changed.
       2. `git add <new or changed files you want to commit>`
       3. `git commit -m 'Explanation of changes you've done with these files'`

5. Push the branch to the GitHub repository for the personal fork with `git push origin <branchname>`.

6. In the `spacetelescope` repository, create a pull request for the recently pushed branch.  You will want to set the base fork pointing to `spacetelescope:master` and the `head` fork pointing to the branch on your personal fork (i.e. `username:branchname`).  Note that if the branch is still under development, you can use the GitHub "Draft" feature (under the "Reviewers" section) to tag the pull request as a draft. Not until the "Ready for review" button at the bottom of the pull request is explicitly pushed is the pull request 'mergeable'.

7. Assign the pull request a reviewer, selecting a maintainer of the `spacetelescope` repository.  They will review your pull request and either accept the request and merge, or ask for additional changes.

8. Iterate with your reviewer(s) on additional changes if necessary, addressing any comments on your pull request.  If changes are required, you may end up iterating over steps 4.ii, 4.iii and 5 several times while working with your reviewer.

9. Once the pull request has been accepted and merged, you can delete your local branch with `git branch -d <branchname>`.

#### Keeping your fork updated <a name="update-fork"></a>
If you wish to, you can keep a personal fork up-to-date with the `spacetelescope` repository by fetching and rebasing with the `upstream` remote:
1. `git checkout master`
2. `git fetch upstream master`
3. `git rebase upstream/master`

#### Collaborating on someone else's fork <a name="collaborate-on-fork"></a>
Users can contribute to another user's personal fork by adding a `remote` that points to their fork and using the nominal forking workflow, e.g.:

1. `git remote add <username> <remote URL>`
2. `git fetch <username>`
3. `git checkout -b <branchname> <username>/<branchname>`
4. Make some changes (i.e. `add/commit` cycle)
5. `git push <username> <branchname>`
