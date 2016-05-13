# Configuring a remote for a fork

To [sync changes you make in a fork](https://help.github.com/articles/syncing-a-fork) with the original repository, 
you must configure a remote that points to the upstream repository in Git.

- Open Terminal.

- List the current configured remote repository for your fork.
```bash
git remote -v
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
```

- Specify a new remote upstream repository that will be synced with the fork.
```bash
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```

- Verify the new upstream repository you've specified for your fork.
```bash
git remote -v
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```
