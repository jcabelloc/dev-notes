# Some Git Notes

### Working with branches
```bash
# Create a branch
$ git branch devbranch
# List Branches
$ git branch
# Go to that branch
$ git checkout devbranch
# Modify files or create files on that branch
# Then confirm those changes or add those files
$ git add .
# Next commit those changes
$ git commit -m 'change on devbranch'

# Come back to the Master
$ git checkout master
# Staying in the master, merge changes from "devbranch" branch
$ git merge devbranch
# ---------
# Inspect a commit point 
$ get log
# Go to a particular checkpoint
$ get checkout 21baf38e135304b9df32db69bc6e65ebf30dd837
# Return to master
$ git checkout master
# -----------
# Revert to a checkpoint (being in master)
$ git revert --no-commit fa02ade322ac84a957bab3ba966fe588511d02d1..HEAD
$ git commit -m "Revert back to fa02ade..."

# Create and reference a new branch at the same time
# It will create a feature branch based on the branch you are on
$ git checkout -b feature
```


### Feature Branch and Pull request
* Reference: https://www.youtube.com/watch?v=oFYyTZwMyAg
```bash
# Create a branch
$ git branch demo-feature-branch

# Go to this branch
$ git checkout demo-feature-branch

# Do some changes to code

# Add and commit your changes
$ git add . && git commit -m "Agregar comentario a archivo environment"

# Update master
$ git checkout master
$ git pull

# Try to merge to master
$ git checkout demo-feature-branch
$ git merge master

# Solve your conflicts

# Add and commit your new changes into feature branch
$ git add . && git commit

# See your commits
$ git log

# Push your feature branch to the repository
$ git push --set-upstream origin demo-feature-branch

# Go to Github and generate a pull request

# On Github, review, comment and finally approve the pull request

# Once approved, return to your master local repository and see results
$ git checkout master
$ git pull
$ git log

# Once checked, you can delete the feature branch on Github
```


### Remote branches
* Remote branches are just like local branches, except they map to commits from somebody else’s repository.
```bash
# Inspect remote commits without merging
$ git fetch
$ git checkout origin/master
$ git log

# remote branches
$ git branch -r
```
* Synchronizing your local repository with a remote repository is actually a two-step process: fetch, then merge. The git pull command is a convenient shortcut for this process.
