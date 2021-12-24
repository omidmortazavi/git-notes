# GIT Notes

>branch early, and branch often

## Quick Reference

`git init`  
`git config`  
`git add <file>`  
`git commit -m <message>`  

`git branch <branch>`  
`git checkout <branch>`  
`git merge <branc>`  
`git rebase <branch>`  

`git cherry-pick <commit>`  

`git status`  
`git log --oneline`  

`git stash`  
`git tag -a <tag> -m <message>`  

`git clone <remote>`  
`git fetch <remote>`  
`git push`  

### Undoing Change

`git reset`  
`git checkout <file>`  
`git reset --hard <branch>`  
`git reset --hard origin/<branch>`  
`git revert <hash>`  

### Feature Branch Workflow

Rebase work on new commits from remote:  
`git pull --rebase; git push`  

### Undo accidental commit to local main

```bash
git reset --hard origin/main ||
git reset --hard <commit> ||
git reset --hard HEAD~n

git checkout -b feature <commit>
git push origin feature
```

## Concepts

### Working Directory

Unstaged Files  

### Staging Area  

Allows grouping of changes before commiting to project history  

### Git History  

History of branches and commits  

### Commit

Immutable snapshot of the project (file base)

### Branch

Git stores a branch as a reference to a commit in the git history

### Tags  

Another way of holding a pointer to a commit, typically used to 'tag' releases with a semantic version

### Status  

![Stages](https://i.stack.imgur.com/MRkpH.png)

- **untracked** - git knows the file exists but will perform no actions unless instructed  
- **unmodified** - git is tracking the file but there are no current changes  
- **modified** - git is tracking the file, there are current uncommitted changes but changes will not be committed  
- **staged** - there are current changes that will be committed. Due to the way git   works, files can have a dual status of modified and staged, when changes have been made after a stage.

### HEAD
HEAD is Git’s way of referring to the current snapshot  
In most cases, the pointer to the most recent commit in the current branch  
`cat .git/HEAD && cat .git/refs/heads/master`  
`git log --oneline`  

### detached HEAD

When HEAD does not point to most recent commit i.e checkout older commit

## GIT Operations

### init

`git init`  
`git init --bare` Init a project with no working directory, useful for shared local projects as you cannot edit files or commit changes.  

### config

Config can be set locally or globally:  
`git config`  
`git config --global`  

Setting up your identitity, used only for tracking changes to git db, not for auth:  
`git config --global user.name "Joe Bloggs"`  
`git config --global user.email "Joe@Bloggs.com"`  

Change Additional Settings:  
`git config --global core.editor "usr/bin/vi"`  

List Config:  
`git config --list`  
`git config user.name`  

Stored in:  
`~/.gitconfig`  

### add

`git add` Add changes in working dir to the index file so they can be tracked in stating area  
`git add .` Add everything

Empty folders are ignored by default, use a .keep file:  
`touch .keep`  

### status

`git status` show files in the staging area yet to be committed  
`git status -s` short output, *prefered*  
`git status -v` verbose output

### commit

`git commit` commit files from the staging area to git history  
`git commit -m "commit message"` bypass editor  
`git commit -a -m "commit message"` adds and commits all unstaged files, use with caution  

`git config --global core.editor vi|vim|nano` update default editor

### remove

`git rm` removes a file from the working tree AND index
`git rm --cached` removes a file from index file only  
`git rm -f` removes from from index file AND DELETS file

### log

`git log`  
`git log --oneline` sparse output, *prefered*  
`git log --graphs` graphing  
`git log -S <keyword>` keyword search  

### diff

`git diff` unstaged changes in working tree  
`git diff --summary` view summary only  
`git diff HEAD^ HEAD` diff changes to current head  
`git diff <hash> <hash>`  

### reset

`git reset` move branch ref backwards to previous commit  
`git checkout <file>` undo all changes to the file, reverting it to the last committed state  
`git reset --hard <branch>` reset everything back to the latest commit on a branch  
`git reset --hard origin/<branch>` sets your current branch to the state of the remote branch

### revert

`git revert HEAD` create a NEW commit that will undo previous one  
`git revert HEAD~n` revert commits at index(n) from HEAD  
`git revert <hash>` revert specific commit  

### cherry-pick

`git cherry-pick <commit1 .. n>` choose 1 or more commits to add to your HEAD *love this*

### ignore

`cat .gitignore`  
`git status --ignored`

### tags

Mark a specific commit in your project, a Tag can be annnotated or lightweight  
`git tag -a <tag> -m <message>` Add Tag  
`git tag -d <tag>` Delete Tag  

`git tag` Check all tags  
`git show <tag>` Show Tag details  

### branch

`git branch <branch>` create a new branch  
`git branch -d <branch>` delete branch
`git switch <branch>` switch branches (replaces branch checkout)

`git branch -f main HEAD~3` moved (by force) main branch to three parents behind HEAD

### checkout

In Git terms, a "checkout" is the act of switching between different versions of a target entity. The git checkout command operates upon three distinct entities: files, commits, and branches.  

#### branches

`git checkout <branch>` switch to another branch  
`git checkout -b <branch>` create and checkout a branch  

#### files

Checking out a file is similar to using git reset with a file path, except it updates the working directory instead of the stage. Unlike the commit-level version of this command, this does not move the HEAD reference, which means that you won’t switch branches  
`git checkout <file>`  

#### commits

`git checkout HEAD^` first parent of HEAD  
`git checkout HEAD^^` grandparent of HEAD  
`git checkout <branch>^` first parent of branch  
`git checkout HEAD~4` moved back 4 places  

### merge

Combine diffrences of branches then creating a new commit pointer  
*Invoke merge from destination branch (i.e branch you want to merge into)*  
`git merge <branch>`  
`git merge --abort` abort a merge

```bash
# fix merge conflict
vi <file>
git add <file>
git commit -m "<message>"
```

`git branch -d <branch>` delete branch after a merge

### rebase

Replay changes made to one branch over the top of another  
*Merge shows accurate history of project, rebase provides a falsified history which is cleaner*  
`git rebase <branch>`  
`git rebase -i` interactive, allows you to squash, re-order and ommit commits

### clone

Clone a local project  
*Useful for testing commit reverts*  
`git clone <local repo> <new repo>`

Clone a remote repository  
`git clone <remote URL>`

### fork
Copy of project to be worked on, can then ask for a pull request from original

### gc

Garbage Collection  
`git gc` cleans old objects that cannot be ref by db  
`git gc --prune` gc older than 2 weeks  
`git gc --auto` highlights if gc is required  

## Remote Operations

### remote

Remote branches viewed locally have a required naming convention.  

```bash
# *origin* is usually the name(alias) used for the remote repo.  
<remote name>/<branch name>
origin/<branch name>
```

`git remote add <name> <url>`  
`git remote` shows the name (alias) for the remote repo  
`git remote -v` shows the actual remote servers tracked by current repo and the alias  
`git remote show origin` remote branches being tracked  

### fetch

`git fetch origin` pulls down remote commits to local copy of remote, does not commit anything to local branch  
`git pull` pulls down commits and attempts to automatically merge  

### push

>It is Bad form to push directly to main branch (Protect Yo Master)

`git push -u <remote> <local-branch>`  
`git push --set-upstream origin <branch_name>` Required on first push if branch does not exist on remote

### fetch/pull

Create an merge request or pull request (typicaly from UI)  
`git fetch origin`  pulls down remote commits to local copy of remote, does not commit anything  
`git merge origin`  
`git pull` pulls down commits and attempts to automatically merge

---
## Divergence

When the remote has moved ahead of your local work. You need to base from the most recent version of the remote. Easiest way to do this is a rebase, here are some methods:  
`git fetch; git rebase origin/main; git push`  
`git fetch; git merge origin/main; git push`  
`git pull --rebase; git push`  
`git pull; git push`  

---

## Useful Resources
<https://git-school.github.io/visualizing-git/>
<https://learngitbranching.js.org>
<https://www.atlassian.com/git/tutorials>
