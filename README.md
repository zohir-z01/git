# Git
## Description:
This project is designed to introduce us to the world of version control and collaboration using **Git**.
 
**Author:** zelbaz  
**Date:** January 11, 2026

## Getting started:
I get started by creating a directory called `work` and i created a subdrectory called `hello` and i created a file inside it called `hello.sh`


```sh
# Creating the directories and initializing the git in hello dir and adding the hello.sh file
mkdir work
cd work
mkdir hello
cd hello
git init
touch hello.sh
```

```sh
# the content of hello.sh
the final content of `hello.sh`
#!/bin/bash
# Default is "World"
name=${1:-"World"}
echo "Hello, $name"
```

The first task is learn how to use `git staus` and `git add` and `git commit`
```sh
# to check the status:
git status

# to add the changes to the staging area:
git add <chnages>

# to commit the changes:
git commit -m <message>
```

## History
to check the histoy of the working directory we can use `git log` command.
```sh
# show the history of the working directory (check [man git log])
git log

# show one-line history showing only the commits hashes and messages
git log --oneline

# to show a specific range of history (by number):
git log -2 #it will show the last 2 commits

# to show a specific range of history (by time):
git log --since="5 minutes ago"

# to personalized format like: 
# * hash date | message (HEAD -> main) user
git log --date=short --pretty=format:"* %h %ad | %s %d [%an]"

# --date=short | show the short format of the date
# %h to show the hash
# %ad to show the author date
# %s to show the commit message
# %d to show the branch info
# %an to show the author
```

## Checkout:
to restore the first snapshot:
```sh
git checkout <first commit hash>
OR
git checkout HEAD~<n> #n= steps to go abck to it

# to retore to the second racent snapshot:
git checkout HEAD~1

# to return to the latest version without using its commit hash:
git checkotu -
```

## Tag:
to tag the current version with a specific tag we can use:
```sh
git tag <tagname>

# to tag a previous commit, we can go to it first and then tag it
git checkout HEAD~1
# then
git tag <tagname>

# to list the available tags:
git tag
```

## Changing mind:
we can undo a change easily, by depending on the current situation, for example if we did a change and we didn't added yet to the staging area by using:
```sh
git restore <changes>
```

But if we made the changes and we added them to the staging area (without commiting them), then we can use:
```sh
git restore --stage <changes>

# then we can restore them if we want to initial stat:
git restore <changes>
```

And if we already commited the changes=, then we can go back to the initial stat by using:
```sh
git revert HEAD
```

And we made a commit and we tagged it, and we want to reset to a previous version:
```sh
# ... changes
git reset --hard v1

# this way we reset the HEAD to point to v1 version
```

we can list the whole commits including the once we reset from, by using:
```sh
git log --all --oneline --graph

# we can also focus on a specific tag:
git log --oneline tagname

# and we can also delete the taged unwanted change:
git tag -d tagname
```

- **Expire old refs and clean unreachable commits**
```sh
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```
<br>

```sh
# and if we commited changes and we needed to edit the changes again and rewrite the previous commit we can use --amend
git commit --amend -m <newmessage>
```

## Move it:
to move using git command we can use `git mv file newname/newpath`, this way the git does the staging for us and keeps tracking renames in the git history.
After that I created the Makefile in the root dir

## .git/ Directory - Key 

### **`objects/`**
**Git's data vault** - stores all commits/files/trees as compressed SHA1 objects

### **`config`**
**Repo settings** - user info, remotes, aliases in INI format

### **`refs/`**
**Pointers directory** - branches/tags map names → commit hashes

### **`HEAD`**
**Current position** - points to branch/commit you're on

<br>

---
<br>

```sh
git rev-parse HEAD        # Get latest commit hash

git cat-file -t HEAD      # Print type (commit)

git cat-file -p HEAD      # Print content
```

## Dumping Directory Tree:
- Dump the full directory tree:
```sh
git ls-tree -r HEAD
```

- Dump lib/ directory content:
```sh
git ls-tree HEAD:lib/
```

- Dump hello.sh content:
```sh
git git show HEAD:lib/hello.sh
```

## Branching:
We can get started by creating a branch using:
```sh
git checkout -b branchname
```

We can switch between branches using `git checkout branchname`

To draw a commit tree diagram illustrating the diverging changes between all branches to demonstrate the branch history, we can use:
```sh
git log --graph --oneline --decorate --all
```

To compare 2 branches we can use:
```sh
git diff main greet
```
To compare 2 branches for a specific file we can use::
```sh
git diff main..greet -- <filename>
```
To drow a tree diagram illustrating the diverging changes between all branches to demonstrate the branch history we can use:
```sh
git log --all --oneline --decorate --graph
```

# Conflicts, merging and rebasing:
to mergea branch we can use:
```sh
git merge <branchname>
```
**conflict** happens when we have 2 different versions (content) on the same spot on both branches, then we have to decide which version to us, this way we resolve the conflict!  
for example if we want to merge `main` into `greet` and there was a conflic, then we can resolve it by accepting from the main:
```sh
git checkout --theirs <filename>
```

To go back to the point before the initial merge between main and greet.
we can check the initial commit before the merge and checkout to it then make out rebasing.
```sh
git reset --hard <initialgitthash>

# then we can do our rebase
git rebase main

# if we run into any conflict we can resolve it and then continue our rebasing
# fix conflict... and then:
git rebase --continue
# or we can abort the rebase:
git rebase --bort
```
### Fast forwarding:
fast forwarding is simply the proccess where git moves the branch pointer forward when margin a branch

## Local and remote repositorie:
We can clone a reposatory using `git clone <url>`. As in the subject asks in the `work` directory to clone the `hello` repo as `cloned_hello`:
```sh
git clone hello cloned_hello
```

after that we can go to the cloned repo and show the logs using `git log`.  

after that we can display the name of the remote repo using:
```sh
git remote -v

# fro detailed info (branches, URLs, status)
git remote show origin
```

To show the local and remote branches in the cloned repo:
```sh
# local branches only:
git branch

# remote branches only:
git branch -r

# remote and local branches:
git branch -a
```

To fetch the changes from the remote repo and show the logs including the remote commits we can use:
```sh
# fetch changes from the loca (gets just info, no changes to the working directory):
git fetch --all

# show complete logs including the remote commits
git log --graph --all --oneli
ne --decorate
```

To merge the remote `main` branch into the local `main` branch:
```sh
# after making sure we are in the local main branch, and we fetched the latest changes from the remote, we can then merge them:

git merge origin/main
```

To add a new local branch named `greet` tracking the remote `origin/greet` branch:
```sh
git checkout -b greet origin/greet

# Or

git checkout --track origin/greet
```
This means that we create and switch to a new local branch pointing to the remote `origin/greet` branch, so the `git fetch/pull` works automatically.  
we can verify the the tracking using:
```sh
git branch -vv
```

### Remote:
to push our work to our remote repo, as in the subject asks to push the `main` and `greet` branches, we can use:
```sh
# add the remote:
git remote add <remotename> <remoteurl>

# push the main branch
git push remotename main

# push the greet branch
git push remotename greet
```

# Bare repositories:
- **what is bare repo**: a bare repo is a git repo without a working directory, it contains only the content of `.git` folder (commits, branches,r refs...), no editable project files

To create a bare repo we can use:
```sh
git init --bare
# Or
git clone --bare
```



# usage:
to see the full hisrtory rename each git dir in each dir `hello` `cloned_hello` to .git






