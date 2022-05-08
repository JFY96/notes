- Git is a popular version control system
- GitHub is a service that allows you to upload your code using Git and manage your code with a nice web interface

# Configuring Git after installation

Check git is properly installed (check locally installed version):
```
git --version
```

Configure git user (name and email):
```
git config --global user.name "Your Name"
git config --global user.email "yourname@example.com"
```

GitHub now uses `main` as the default branch instead of `master` (from Oct 2020), so change it for Git using this command:
```
git config --global init.defaultBranch main
```

To enable colorful output with git, type
```
git config --global color.ui auto
```

To verify things are as expected:
```
git config --get user.name
git config --get user.email
```
or to get a list of all git configurations:
```
git config --list
```

## GitHub SSH Key

- An SSH key is a cryptographically secure identifier
- GitHub uses SSH keys to allow you to upload to repository without having to type user/password every time

To check if you already have an SSH key already installed:
```
ls ~/.ssh/id_rsa.pub
```

If it doesn't exist and shows an error, then no SSH key exists. If no message appears then there already is one.

To create a new SSH key:

```
ssh-keygen -C <youremail>
```

### Linking SSH key with GitHub

On GitHub, navigate to `Settings` > `SSH and GPG keys`. Then select `New SSH Key`, enter a description (e.g. computer name), and enter your public SSH key, then add. To grab the key, the `cat` command can be used:

```
cat ~/.ssh/id_rsa.pub
```

### Testing the key

```
ssh -T git@github.com
```

# Git and GitHub Overview

- Git is a fast and modern version control system, designed to work well with text files
- Git is a distributed version control system (DVCS) - the complete codebase (and version history) is mirrored on every developers computer
- Git keeps an historical record (snapshots) of content changes
- Git facilitates collaborative changes to files
- Nearly every git operation is local, so there is very little you can't do with it when offline
    - commit happily when offline until you get a network connection to upload

- GitHub allows you to store your work in an online folder ("repository")

# Git States

![git states](https://git-scm.com/book/en/v2/images/areas.png)

# Git Commands cheatsheet

## For help

```
git help <verb>
OR
git <verb> --help
```
i.e.
```
git help config
git config --help
```

## Initialize a reposity from existing code

to initialise a git repository in the current folder:
```
git init
```

create a git ignore file (unix):
```
touch .gitignore
```

## Cloning a remote repository

```
git clone <url> <where to clone>
```

e.g. clone files from remote `https://github.com/user-name/repository-name.git` to `.` (current directory)
```
git clone https://github.com/user-name/repository-name.git .

or

git clone git@github.com:USER-NAME/REPOSITORY-NAME.git .
```

Skipping `<where to clone>` will create a directory in the current directory. e.g.
```
cd C:/Users/user/Documents/GitHub
git clone git@github.com:USER-NAME/REPOSITORY-NAME.git
```
creates a folder named `REPOSITORY-NAME`

## Check working tree

Run this often (e.g. after add/reset):
```
git status
```

## Add files to staging area

add single file for commiting:
```
git add <filename>
```

Adds all files (in the current directory) for commiting:
```
git add .
```
Alternatively, this does the same (except it ignores all files whose names being with `.`)
```
git add *
```

Adds all files (throughout project) for commiting:
```
git add -A
```

remember `git status` to check state after

## Remove files from staging area

remove single file:
```
git reset <filename>
```

remove all files:
```
git reset
```

remember `git status` to check state after

## Commit

```
git commit -m <detailed message of changes made>
```

## Check log

Shows commit ids, authors, dates
```
git log
```

## View remote info

list info about the repo:
```
git remote -v
```

list all branches (locally and remotely)
```
git branch -a
```

## Pull
(`main` is the default branch - may want to do a pull for a different branch)
```
git pull origin main
```

## Push
```
git push origin main
```

## Create and checkout a branch

Create new branch:
```
git branch <branchname>
```

Checkout branch:
```
git checkout <branchname>
```

## Pushing branch to remote

```
git push -u origin <branchname>
```

`-u` is to associate our local branch with the remote version, so we can just do `git pull` and `git push` later 

remember `git branch -a` to check branches after

## Merge branch

To check branches merged in so far:
```
git branch --merged
```

Merge branch
```
git merge <branchname>
```

## Delete a branch

delete local branch:
```
git branch -d <branchname>
```

delete remote branch:
```
git push origin --delete <branchname>
```

# Common workflow

- Create and checkout branch
- make file changes
- commit changes (git status, stage changes, then commit with detailed message)
- push branch to origin
- checkout master/main
- pull
- merge
- push to remote

## Process for pushing changes

1. First check file changes:

this command shows differences made in the files changed
```
git diff
```

2. Check working tree
```
git status
```

3. Add files to staging area
```
git add -A
```

4. Commit
```
git commit -m <detailed message of changes made>
```

5. Pull
```
git pull origin main
```

6. Push
```
git push origin main
```

## Merging a branch

First switch to `main` and pull
```
git checkout main

git pull origin main
```

Check branches merged in so far:
```
git branch --merged
```

Merge branch with main
```
git merge <branchname>
```

Push to origin:
```
git push origin master
```

# Squashing commits

To work with last 4 commits:
```
git rebase -i HEAD~4
```

then replace `pick` with `squash` (or `fixup`) on the commits that should be squashed

`:w` to write then `:q` to quit (or `:wq` for both)

# Revert file(s) to a specific commit

```
git checkout XXXXXXX -- fileToRestore file2ToRestore
```

where `XXXXXXX` is the commit

# Reverting a pushed commit

To see history of branch:
```
git reflog
```

Find the commit id of the one that it needs to be reset to.
```
git reset --hard <commit-number>
```

To change remote repository history (not recommended if someone has already pulled in the new changes):
```
git push -f
```

# Submodules

When cloning a project, by default you get the directories that contain submodules, but not their contents. You must run these commands below.

To initialize local configuration file:
```
git submodule init
```

To fetch all the data:
```
git submodule update
```