# Git Stash – Temporarily Stashing Your Work

So there you are, working on a new feature, modifying files in the working directory and/or index and you find out you need to fix a bug on a different branch. You can’t just switch to a different branch and lose all your work.

git stash to the rescue!

**`git stash`**: 

- Saves your working directory and index to a safe place. 

- and Restores your working directory and index to the most recent commit.

You can then work on other branches, make commits, etc. and when you’re ready to get back to where you were, you type git stash pop and you’re back, working at full speed.

### Example: Working Normally

Before you start git stashing, make sure any new files added to the working directory have been added to the index: git stash will not stash (save) files in the working directory unless the files are being tracked (some version of the file has been added to the index).

Let’s create a repository, add a file, and make the first commit:

```bash
$ git init
Initialized empty Git repository ...
$ echo This is the README file. > README
$ git add README
$ git commit -m'Initial commit'
[master (root-commit) 35ede5c] Initial commit
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 README
 ```
 
Next we’ll make some changes to the working directory:

- Add a line to README

- Add a new file, file2

```bash
$ echo ADD TO THE WORKING DIRECTORY VERSION OF README >> README
$ echo file2 is here > file2
$ ls
file2  README
```

### We Get Interrupted

Now we find out about the bug we need to fix in another branch. Our goal is to stash (save) the changes that we had made to the working directory, go to the other branch and then eventually return to right before we heard about that bug.

### Add Files To The Index

We first need to add the new file, file2, to the index, so git will track the file and know to save the file during the git stash:

```bash
$ git add file2
```

We don’t need to add README to the index since that file path was already in the index: Git will notice that the working directory version is newer than the index and will stash it.

### Stash Time

Now we’re ready for the stashing:

```bash
$ git stash
Saved working directory and index state WIP on master: 8d8b865 Initial commit
HEAD is now at 8d8b865 Initial commit
$ ls
README
$ cat README
This is the README file.
```

The above shows:

- The changes we had made to the working directory, are squirreled away somewhere to somewhere safe

- The working directory is set back to its state before the modifications were made. In particular:
  - File file2 was removed
  - File README no longer has the second line.
  
### Work On Another Branch or Two

Now we can do anything we want, such as `git checkout other-branch`, make modifications, fix bugs, and commit the fix to that branch.

When we’re ready to continue where we left off, above, we simply type `git stash pop` and our “stashed” working directory is back where it was when we had typed git stash:

```bash
$ git stash pop
# On branch master
# Changes to be committed:
#   (use "git reset HEAD ..." to unstage)
#
#   new file:   file2
#
# Changed but not updated:
#   (use "git add ..." to update what will be committed)
#   (use "git checkout -- ..." to discard changes in working directory)
#
#   modified:   README
#
Dropped refs/stash@{0} (9c72eaeebdb712ad527f06f88a5cbec1098957f1)
$ ls
file2  README
$ cat README
This is the README file.
ADD TO THE WORKING DIRECTORY VERSION OF README
```

> Git stash: Just unbelievably convenient, easy and fast.

# 链接

[Git Stash – Temporarily Stashing Your Work](http://www.gitguys.com/topics/temporarily-stashing-your-work/)
