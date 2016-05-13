# Merging an upstream repository into your fork

If you don't have push (write) access to an upstream repository, then you can pull commits from that repository into your own fork.

- Open Terminal.

- Change the current working directory to your local project.

- Check out the branch you wish to merge to. Usually, you will merge into `master`.
```bash
git checkout master
```

- Pull the desired branch from the upstream repository. This method will retain the commit history without modification.
```bash
git pull https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git BRANCH_NAME
```

- If there are conflicts, resolve them. For more information, see "[Resolving a merge conflict from the command line](https://help.github.com/articles/resolving-a-merge-conflict-from-the-command-line)".

- Commit the merge.

- Review the changes and ensure they are satisfactory.

- Push the merge to your GitHub repository.
```bash
git push origin master
```

# 链接

[Merging an upstream repository into your fork](https://help.github.com/articles/merging-an-upstream-repository-into-your-fork/)
