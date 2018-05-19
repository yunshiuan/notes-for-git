# GitNotes

Here is my place to make notes about git and test git functions. Notes would include what I learned from [Version Control with Git](https://www.coursera.org/learn/version-control-with-git/home/info) on Coursera.

## Documentation

- [Reference Manual](https://git-scm.com/docs)
- [Pro Git](https://git-scm.com/book/en/v2)

## Git syntax and Getting help

Git models the relationship of commits with directed acyclic graph (DAG). The arrows point to a commit's parent(s).

### Syntax

`git [command] [--flags] [arguments]`

### Getting help

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