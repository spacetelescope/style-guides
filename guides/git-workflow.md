# Workflow for contributing to a `git`/ GitHub repository in the `spacetelescope` organiztation

### Introduction
The following describes a workflow for contributing software to an open source repository on GitHub under the `spacetelescope` organization (hereby referred to as the "`spacetelescope` repository". It involves forking the `spacetelescope` repository, developing changes on personal branches, and opening pull requests through GitHub.

### Opening a GitHub Issue
Before contirbuting, one should determine if the eventual change is significant enough to warrant a GitHub issue; if you think that this change will be solving a significant problem or adding a providing a significant enhancement to the project, then it would be advantageous to open an issue under the main GitHub repository. This will allow both contributors and repository maintainers to keep track of the project and capture any needed context of the partiuclar change. Any appropriate individuals should be assigned to the issue, and any appropriate label(s) should be tagged.

### Workflow
Any changes that you want to eventually make to the `master` branch of the `spacetelescope` repository should be done through the workflow wherein you create a fork and work on a personal branch before submitting those changes to be reviewed through a pull request. Instructions on how to do those things can be found below:

1. Create a personal fork of the `spacetelescope` repository by visiting its location on GitHub and clicking the `Fork` button.  This will create a copy of the `spacetelescope` repository under your personal GitHub account (hereby referred to as "personal fork").  Note that this only has to be done once.

2. Make a local copy of your personal fork by cloning the repository (e.g. `git clone https://github.com/username/repository_name.git`, found by clicking the green "clone or download" button.).  Note that, unless you explicitly delete your clone of the fork, this only has to be done once.

3. Ensure that the personal fork is pointing to the `upstream` `spacetelescope` repository with `git remote add upstream https://github.com/spacetelescope/repository_name.git`.  Note that, unless you explicitly change the remote location of the repository, this only has to be done once.

4. Create a branch on the personal clone to develop software changes on.  Branch names should be short but descriptive (e.g. `new-database-table` or `fix-ingest-algorithm`) and not too generic (e.g. `bug-fix`).  Also consistent use of hyphens is encouraged.
    1. `git branch <branchname>`
    2. `git checkout <branchname>` - you can use this command to switch back and forth between existing branches.
    3. Perform local software changes using the nominal `git add`/`git commit -m` cycle:
       1. `git status` -  allows you to see which files have changed.
       2. `git add <new or changed files you want to commit>`
       3. `git commit -m 'Explanation of changes you've done with these files'`

5. Push the branch to the GitHub repository for the personal fork with `git push origin <branchname>`.

6. On the `spacetelescope` repository, create a pull request for the recently pushed branch.  You will want to set the base fork pointing to `spacetelescope:master` and the `head` fork pointing to the branch on your personal fork (i.e. `username:branchname`).  Note that if the branch is still under heavy development, you can put `WIP:` at the beginning of the pull request title to signify that the pull request is still a work in progress (i.e. `WIP: Example Pull Request Title`).  Not until the `WIP:` tag is explicitly removed will the pull request be deemed 'mergable'.

7. Assign the pull request a reviewer, selecting a maintainer of the `spacetelescope` repository.  They will review your pull request and either accept the request and merge, or ask for additional changes.

8. Iterate with your reviewer(s) on additional changes if necessary, addressing any comments on your pull request.  If changes are required, you may end up iterating over steps 4.ii, 4.iii and 5 several times while working with your reviewer.

9. Once the pull request has been accepted and merged, you can delete your local branch with `git branch -d <branchname>`.

### Keeping your fork updated
If you wish to, you can keep a personal fork up-to-date with the `spacetelescope` repository by fetching and rebasing with the `upstream` remote:
1. `git checkout master`
2. `git fetch upstream master`
3. `git rebase upstream/master`

### Collaborating on someone else's fork
Users can contribute to another user's personal fork by adding a `remote` that points to their fork and using the nominal forking workflow, e.g.:

1. `git remote add <username> <remote URL>`
2. `git fetch <username>`
3. `git checkout -b <branchname> <username>/<branchname>`
4. Make some changes (i.e. `add/commit` cycle)
5. `git push <username> <branchname>`
