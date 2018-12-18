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
