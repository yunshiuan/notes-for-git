# Git Notes

Here is my place to make notes about git and test git functions. Notes would include what I learned from [Version Control with Git](https://www.coursera.org/learn/version-control-with-git/home/info) on Coursera. Figures below are all taken from the course.

Git models the relationship of commits with directed acyclic graph (DAG). The arrows point to a commit's parent(s).

## Table of contents

- [Documentation](#documentation)
- [Git syntax](#git-syntax)
- [Getting help](#getting-help)
- [Configuration](#configuration)
- [Git references](#git-references)
- [Branches](#branches)
- [Merging](#merging)

----

## Documentation

- [Reference Manual](https://git-scm.com/docs)
- [Pro Git](https://git-scm.com/book/en/v2)

## Git syntax

`git [command] [--flags] [arguments]`

## Getting help

- "full" help: `git help [command]` (will be the same with the online documentation)
- concise help: `git <command> -h`

### Reading help

Take `git fakecommand (-p|--patch) [<id>] [--] [<paths>...]` as an example.

- `-f` or `--flag` changes the command's behavior
- `|` represents "or"
- `[optional]`
- `<placeholder>`
- `[<optional placeholder>]`
- `()` is used for grouping
- `--` disambiguates the command
- `...` indicates that multiple occurrences are possible

## Configuration

`git config [--local|--global|--system] <key> [<value>]`

The precedence is: local > global > system

## Git references

Git references can be substituted for SHA-1 hashes. For example, `git show HEAD` can be used instead of `git show <commit>`.

### Branch label and HEAD

- Branch label points to the most recent commit in the branch
- `HEAD` is the reference to the current commit and usually points to the branch label of the current branch
- There is only one `HEAD` per repository

### Prior commits

- `~` refers to a prior commit
	- `~` or `~1` is "parent"
	- `~~` or `~2` is "parent's parent"
- `^` refers to a parent in a merge commit
	- `^` or `^1` is "first parent of the commit"
	- `^2` is "second parent of a merge commit"
	- `^^` is first parent's first parent

### Tags

- `git tag` is used to view all tags in the repository
- Lightweight tags
	- A simple reference to a commit
	- `git tag <tagname> [<commit>]` (`<commit>` defaults to `HEAD`)
- Annotated tags
	- A full Git object that references a commit
	- `git tag -a [-m <msg> | -F <file>] <tagname> [<commit>]` (`<commit>` defaults to `HEAD`)
	- `git show <tagname>` displays the tag object information followed by the commit information
- `git push` does not automatically transfer tags
	- To transfer a single tag: `git push <remote> <tagname>`
	- To transfer all of the tags: `git push <remote> --tags`

## Branches

### Viewing branches

- `git branch` lists all local branches
- `git branch -a` lists local and remote tracking branches

### Creating branches

`git branch <name>` creates a new branch label reference and remains us on the original branch.

### Checkout

Use `git checkout <branch_or_commit>` to checkout a branch or a commit. The latter detaches `HEAD`, which makes `HEAD` point directly to a commit instead of a branch label.

Checkout does:

1. Updating the `HEAD` reference
2. Updating the working tree with the commit files

`git checkout -b <name>` to create and checkout a new branch named `name`.

### Deleting branches

- `git branch -d <name>` deletes the `name` branch
- `git branch -D <name>` deletes the `name` branch __regardless of unmerged works__

### Dangling commits

Dangling commits do not belong to any branch. They are the unmerged commits from forced-deleted branches. Git periodically does garbage collecting and deletes them.


`git reflog` is able to undo an accidental branch delete if the dangling commits are not yet deleted. It returns a __local__ list of recent `HEAD` commits.

![alt text][git_reflog]

[git_reflog]: figures/git_reflog.png "git reflog"

## Merging

Commits belong to both branches after merging.

![alt text][merge]

[merge]: figures/merge.png "merge"

There are four main types of merges:

1. Fast-forward merge
2. merge commit
3. Squash merge
4. Rebase

### Fast-forward merge

- Moves the base branch label to the tip of the topic branch

![alt text][ff_merge]

[ff_merge]: figures/ff_merge.png "fast-forward merge"

- `git merge <topic_branch>` in the base branch attempts a fast-forward merge by default
- Results in a linear commit history

### Merge commit

- Combines the commits at the tips of the merged branches
- Places the result in the merge commits

![alt text][merge_commit]

[merge_commit]: figures/merge_commit.png "merge commit"

- `git merge <topic_branch>` creates a merge commit if it is not fast-forwardable
- `git merge --no-ff <topic_branch>` always creates a merge commit even when it is fast-forwardable
- A merge commit has multiple parents