# Adding And Removing Remote Branches – Git Branch

### Commands discussed in this section

- git branch

- git checkout

- git push

- git remote

### Creating Remote Branches

One way to add a new branch to the remote repository is to first add the branch to your local repository 
and then push that local branch to the remote repository. Let’s see what branches we have now:

```bash
amy$ git branch
* master
```

We have just one branch. Not much to shake a stick at. So Amy creates a new branch named v0:

```bash
amy$ git branch v0
```

She then pushes the new branch named v0 to the remote repository named origin.

The git push syntax is: `git push [remote-repository-name] [branch-or-commit-name]`:

```bash
amy$ git push origin v0
Total 0 (delta 0), reused 0 (delta 0)
To file:///home/gitadmin/project1.git
 * [new branch]      v0 -> v0
 ```
 
Currently the master and v0 branches are identical, but they will diverge (the whole point of branches is to diverge)
as users make different commits to each branch.



