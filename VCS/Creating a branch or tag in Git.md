# Creating a branch or tag in Git

### Tags

Tags are used for creating stable releases. To create a tag for using with the Git Drupal Repository, first, ensure that 
you're [following the tag naming convention](http://drupal.org/node/1015226) if you're using this tag for making a release. 
From inside the directory of the project, an example is:

`git tag 7.x-1.0`

Once the tag is created, you need to push the tag up to the master repository. By itself, push doesn't send the tags up, 
you also need to tell it to include the tags in the push by appending the --tags flag:

`git push --tags`

If you don't want to push all your tags, you can also be specific:

**Example:**

`git push origin tag 7.x-1.0`

To check and confirm remote tags, the command is

`git tag -l`

### Branches

Working with branches is similar to working with tags, but branches are used for development releases. First create the 
new branch and check it out:

`git checkout -b 7.x-2.x`

Once the branch is created locally, it can be pushed up to the remote repository:

`git push origin 7.x-2.x`

To work with this branch:

`git checkout 7.x-2.x`

To see what branches you currently have:

`git branch -v`

The branch with the asterisk next to it is the active branch:

```bash
% git branch -v
* 7.x-2.x 170eb10 Initial commit.
  master  170eb10 Initial commit.
```

In order for your module to work correctly with the Drupal.org testing system and supporting projects like Git Deploy, 
development branches should always be named using the [Drupal version]-[Module version] pattern, never as master or develop. 
See here for master branch cleanup info.

### Newer Git Commands

If you're running Git 1.7.0.9 or later:

When pushing a branch, you can specify `-u` and your local branch will be automatically set up to track the remote branch. 
For example:

```bash
git checkout -b 7.x-1.x
git push -u origin 7.x-1.x
```

### Deleting a tag/branch

If you mistakenly added a tag or branch, and want to remove it (assuming you haven't created a release with the tag, 
or committed anything to the branch), you can remove it by running the following commands.

Branch with name "branchname" (the second command is only needed if you already pushed it to the drupal.org repository):

```bash
git branch -d branchname
git push origin :branchname
```

Tag with name "tagname" (again, the second command is only necessary if you already pushed it to the drupal.org repository):

```bash
git tag -d tagname
git push origin :tagname
```

If for some reason you have a branch and a tag that are named the same (you shouldn't, but we're talking about a mistake here anyway), you can push with the following commands:

```bash
git push origin :refs/heads/branchname
git push origin :refs/tags/tagname
```

To check and/or confirm available remote branches command is

`git branch -r`

# 链接

[Creating a branch or tag in Git](https://www.drupal.org/node/1066342)


