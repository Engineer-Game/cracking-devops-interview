# Control Version

## Undo commits

Unstage all files and directories you might have staged with **git add**:

```
$ git reset --hard HEAD
```

Preview and then remove untracked files and directories:

```
$ git clean -nd
$ git clean -fd
```

## Squash several Git commits into a single commit

### Easy mode: Reset your feature branch to the master state

The easiest way to turn multiple commits in a feature branch into a single commit is to reset the feature branch changes in the master and commit everything again.

```
# Switch to the master branch and make sure you are up to date.
git checkout master
git fetch 
# this may be necessary (depending on your git config) to receive updates on origin/master

git pull
# Merge the feature branch into the master branch.

git merge feature_branch

# Reset the master branch to origin's state.
git reset origin/master

# Git now considers all changes as unstaged changes.
# We can add these changes as one commit.
# Adding . will also add untracked files.
git add --all
git commit
```

### Hard mode: Squash commits

This method is harder than using the`get reset`method above. Also it doesn't work well if you merged the master into the feature branch previously (you'll need to resolve all conflicts again).

What we are describing here will destroy commit history and can go wrong. For this reason, do the squashing on a separate branch:

```
git checkout -b squashed_feature
```

This way, if you screw up, you can go back to your original branch, make another branch for squashing and try again.

To squash all commits since you branched away from master, do

```
git rebase -i master
```

Note that rebasing to the master does not work if you merged the master into your feature branch while you were working on the new feature. If you did this you will need to find the original branch point and call`git rebase`with a SHA1 revision.

Your editor will open with a file like

```
pick fda59df commit 1
pick x536897 commit 2
pick c01a668 commit 3
```

Each line represents a commit (in chronological order, the latest commit will be at the**bottom**).

To transform all these commits into a single one, change the file to this:

```
pick fda59df commit 1
squash x536897 commit 2
squash c01a668 commit 3
```

This means, you take the first commit, and squash the following onto it. If you remove a line, the corresponding commit is actually really lost. Don't bother changing the commit messages because they are ignored. After saving the squash settings, your editor will open once more to ask for a commit message for the squashed commit.

You can now merge your feature as a single commit into the master:

```
git checkout master
git 
merge
 squashed_feature
```
