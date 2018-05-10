# GitNotes

Here is my place to make notes about git and test git functions. Notes would include what I learned from [Version Control with Git](https://www.coursera.org/learn/version-control-with-git/home/info) on Coursera.

## Documentation

- [Reference Manual](https://git-scm.com/docs)
- [Pro Git](https://git-scm.com/book/en/v2)

## Git syntax and Getting help

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

The precedence is: local > global > system.
