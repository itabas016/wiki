---
title: "Git command补漏"
layout: page
date: 2017-04-08 22:11:04
---

[TOC]

## Git容易忘的command ##

### git xxx --bare ###

``` bash
# git init empty repo only with .git element
git init --bare

# git clone repo with all the git related revision history
git clone --bare wiki wiki.git

# previous step git clone with bare equals the below command
cp -Rf wiki/.git wiki.git
```

### git remote xxx ###

``` bash
# git remote add new repo address with specific port
git remote add origin ssh://git@mydomain.com:12345/home/git/repo.git/
```

### git diff xxx ###

``` bash
#  git diff [<options>] [<commit> [<commit>]] [--] [<path>...]

# various ways to check working tree
git diff
git diff --cached
git diff HEAD

# comparing with arbitrary commits
git diff test
git diff HEAD -- ./test

# comparing branches
git diff xxbranch master
git diff xxbranch..master
git diff xxbranch...master
```

### git tag xxx ###

``` bash
# create tag
git tag -a v1.0 -m "tag message detail"

# show someone tag
git show v1.0

# push local tags to origin branch
# just one tag
git push origin v1.0
# all the local tags
git push origin --tags
```

### git merge xxx ###

``` bash
#  git merge [<options>] [<commit>...]
#  or: git merge [<options>] <msg> HEAD <commit>

git merge hotfix
git merge fixes enhancements
git merge --no-commit tempbranch

# if has conflict, use mergetool command
git mergetool
git log --merge -p<path>
git show :1:filename
```

### git reset xxx ###

``` bash
#  git reset [--mixed | --soft | --hard | --merge | --keep] [-q] [<commit>]
#  or: git reset [-q] <tree-ish> [--] <paths>...
#  or: git reset --patch [<tree-ish>] [--] [<paths>...]

# reset work tree to the last commit
git reset --bard HEAD

# just reset specific files
git chechout -- <specific.flie>

# How to fix the issue when after you commit
# The better way is create new commit to cover issue use revert
git revert HEAD

# if reset next-to-last commit
git revert HEAD^ 
```

### git commit xxx ###

``` bash
# -a parameter contains git add and git commit
git commit -a

# -v parameter will display all the diff messages
git commit -v

# --amend will use the latest commit instead before commit
git commit --amend -m "the latest commit with forgot files"
```