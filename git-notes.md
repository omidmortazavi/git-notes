# GIT Notes
** branch early, and branch often**

## Cheat Sheet
git init
git config
git add <file>
git status
git commit -a -m <message>
git log --oneline
git tag -a <tag> -m <message>
git branch <branch>
git checkout <branch>
git merge
git branch -d <branch>
git revert <hash>

## Initialising
git init
git init --bare   contains no working directory, useful for shared local projects

## Git Config
Config can be set locally or globally:
git config 
git config --global

Setting up your identitity, used only for tracking changes to git db, not for auth:
git config --global user.name "Joe Bloggs"
git config --global user.email "Joe@Bloggs.com"

Can change Additional Settings:
git config --global core.editor "usr/bin/vi"

List Config:
git config --list
git config user.name

Stored in:
~/.gitconfig

## Adding
git add           Adds file to the index file so they can be tracked in stating area
git status        What files are in the staging areas (not commited yet)
git rm            Removes a file from the project
git rm --cached   Removes a file from index file
git rm -f         Removes from from index file AND DELETS file

.keep         Empty folders are ignored by default, use a .keep file

## Status
git status
git status -s
  A   : new
  ??  : untracked
  M   : modified
  D   : deleted
git status -v

## Committing
git commit                      commit files in the staging area
git commit -m "Commit Message"  bypass editor
git commit -a -m "Message"      commit a modifified file int he staging area

To view commits:
cat .git/COMMIT_EDITMSG
git log
git log --oneline

Reverting:
git revert HEAD                 Create a NEW commit that will undo previous one
git revert HEAD~n               Revert commit at index(n) from HEAD (See git log --oneline)
git revert <hash>        Revert specific commit

## Ignoring
.gitignore
git status --ignored

cat .git/info/exclude

## Tags
Mark a specific commit in your project
Annnotate or Lightweight
git tag -a <tag> -m <message>   Add Tag
git tag -d <tag>                Delete Tag

git tag                         Check all tags
git show <tag>                  Show Tag details

## Branches
Should use protected master/main branch
git branch <branch>             Create a new branch
git checkout <branch>           Switch to another branch
git checkout -b <branch>        Create and checkout a branch
git switch
git branch -d <branch>
git status

HEAD                            pointer to current branch being worked on
cat .git/HEAD
cat .git/refs/heads/master
git log --oneline

## Merging
Invoke merge from destination branch (i.e branch you want to merge into)
git merge <branch>
git branch -d <branch>

git merge --abort
or
vi <file>     fix merge conflict
git add <file>
git commit -m "<message>"

## Rebasing
Merge - combine diffrences of branches then creating a new commit pointer
Rebase - replay changes made to one branch over the top of another
Merge shows accurate history of project, rebase provides a falsified history which is cleaner
git rebase <branch>

## Diffing
git diff                Unstaged changes in working tree
git diff --summary      View summary only
git diff HEAD^ HEAD     Diff changes to current head
git diff <hash> <hash>

## Gargbage Collection
git gc                  Cleans old objects that cannot be ref by db
git gc --prune          GC older than 2 weeks
git gc --auto           Highlights if gc is required

## Logs
git log --oneline       Sparse output
git log --graphs        Graphing
git log -S <keyword>    Keyword Search

## Cloning
Local - Useful for testing commit reverts
git clone <local repo> <new repo>

Remote Repository
git clone <remote URL>

## Forking
Copy of project to be worked on, can then ask for a pull request from original

## Remote Repositories
git remote              shows the alias used for the remote repo (origin)
git remote -v           shows the actual remote servers tracked by current repo and the alias
git remote show origin  remote branches being tracked
git fetch origin        pulls down remote commits to local copy of remote, does not commit anything
git pull                pulls down commits and attempts to automatically merge

## Pushing
Bad form to push directly to main branch:
git push -u <remote> <local-branch>

## Pulling
Create an merge request or pull request from UI
git fetch origin        pulls down remote commits to local copy of remote, does not commit anything
git merge origin

git branch -a

git pull                pulls down commits and attempts to automatically merge