# Git Notes

Here is my place to make notes about Git. Most notes were organized from [Version Control with Git](https://www.coursera.org/learn/version-control-with-git/home/info) on Coursera. Figures below are all taken from the course.

## Table of contents

- [Documentation](#documentation)
- [Git syntax](#git-syntax)
- [Getting help](#getting-help)
- [Configuration](#configuration)
- [Git references](#git-references)
- [Branches](#branches)
- [Merging](#merging)
- [Merge conflicts](#merge-conflicts)
- [Tracking branches](#tracking-branches)
- [Network commands](#network-commands)
- [Rebasing](#rebasing)
- [Rewriting history](#rewriting-history)

----

## Documentation

Git models the relationship of commits with directed acyclic graph (DAG). The arrows point to a commit's parent(s).

- [Reference Manual](https://git-scm.com/docs)
- [Pro Git](https://git-scm.com/book/en/v2)

### Common works on GitHub

- [Syncing a fork](https://help.github.com/articles/syncing-a-fork/)
- [Closing issues using keywords](https://help.github.com/articles/closing-issues-using-keywords/)
- [Comparing workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)

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

Fast-forward merges and merge commits will be discussed in this section. The other two will be respectively included in [Rebasing](#rebasing) and [Rewriting history](#rewriting-history) sections.

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

## Merge conflicts

Merge conflicts occur only when two branches change the same hunk in different ways.

Two options one has when s/he encounters a merge conflict:

1. Fix conflicts and run `git commit`
2. `git merge --abort` to abort the merge

### Resolving conflicts

Resolving a merge conflict involves three commit:

1. The tip of the current branch - "ours" or "mine"
2. The tip of the branch to be merged - "theirs"
3. A common ancestor - "merge base"

### Reading conflict markers

- Git creates files with in the working tree containing conflict markers when a conflict occurs
- Text from the `HEAD` commit (ours) is between <<<<<<< and =======
- Text from the branch to be merged (theirs) is between ======= and >>>>>>>

## Tracking branches

A tracking branch is a local branch that represent a remote one. It looks like `<remote>/<branch>`, e.g. `origin/master` locally, where `origin` is a shortcut for the URL to the remote repository, and `master` is the name of the branch on the remote repository.

- Related but decoupled (no longer in sync)
- `git branch --all` displays local and tracking branch names
- `remotes/origin/HEAD` specifies the default remote tracking branch, allowing `<remote>` to be specified instead of `<remote>/<branch>` in Git commands
- The default tracking branch (`remotes/origin/HEAD`) can be changed with `git remote set-head <remote> <branch>`

## Network commands

### Clone

- Copies a remote repository

### Fetch

- Retrieves new objects and references from the remote repository
- Tracking branches are updated

  ![alt text][fetch]

  [fetch]: figures/fetch.png "fetch"

- After `git fetch`, `git status` will inform us that our current branch is behind the tracking branch

### Pull

- Fetches and merges commits locally
- `git pull` = `git fetch` + `git merge FETCH_HEAD`
- Merging options
  - `--ff` (default): fast-forward if possible, otherwise perform a merge commit
	
    ![alt text][pull_ff]

    [pull_ff]: figures/pull_ff.png "pull with a fast-forward merge"

  - `--no-ff`: always include a merge commit
	
    ![alt text][pull_commit]

    [pull_commit]: figures/pull_merge_commit.png "pull with a merge commit"
	
  - `--ff-only`: cancel instead of doing a merge commit when it is not fast-forwardable
  - `--rebase [--preserve-merges]` (See [Rebasing](#rebasing))
- If there are uncommitted changes when `git pull`ing
  - Aborted automatically if they are conflicting
  - Keeping them untouched otherwise

### Push

- Adds new objects and references to the remote repository
- `git push [-u] [<repository>] [<branch>]`, where `-u` (`--set-upstream`) tracks this branch
- `fetch`ing or `pull`ing before `push`ing is suggested - the push will be rejected if the tracking branch is not current

## Rebasing

- __Rewrites the history__ so do not do this to the shared history
- Moves commits to a new parent, when `rebase`ing, Git applies the differences to the new parent commit

  ![alt text][rebase]

  [rebase]: figures/rebase.png "rebase"

- `Rebase`ing is a merge so conflicts can happen, commit by commit

  ![alt text][conflict_merge_rebase]

  [conflict_merge_rebase]: figures/conflict_merge_rebase.png "resolving conflicts when merging and rebasing"

- `git rebase <upstream>` changes the parent of the currently checked out branch to `<upstream>`
- `git rebase <upstream> <branch>` checks out `<branch>` and changes its parent `<upstream>`

  ```shell
  git checkout featureX
  git rebase master
  
  # Equivalent to
  git rebase master featureX
  ```

## Rewriting history

### Amending a commit

- Changes the most recent commit and creates a new SHA-1 with either or both of
  - Changed commit message
  - Changed project files
- `git commit --amend [-m] [<new_commit_message>]` to amend the most recent commit
- `git commit --amend --no-edit` to reuse the previous commit message

### Interactive rebase

- `git rebase -i <after_this_commit>` lists commits in the current branch after `<after_this_commit>` in an editor and they can all be modified
- Interactive rebase options include
  - Use the commit as is
  - Edit the commit message
  - Stop and edit the commit
  - Drop (delete) the commit
  - Squash
  - Fixup
  - Reorder commits
  - Execute shell commands

### Squash merge

- Merges the tip of the topic branch onto the tip of the base branch and commits the result

  ![alt text][squash_merge]

  [squash_merge]: figures/squash_merge.png "squash merge"

- After deleting the topic branch label, the commits in the topic branch are no longer part of any named branch and will be garbage collected by Git
- `git merge --squash <topic_branch>`
