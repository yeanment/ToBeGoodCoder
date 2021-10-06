# Notes for Pro Git
## Introduction
Version Control System (VCS) is a system that records
changes to a file or set of files over time so that you can recall specific versions later. It
allows you to revert selected files back to a previous state, revert the entire project back to a
previous state, compare changes over time, see who last modified something that might be causing
a problem, who introduced an issue and when, and more. 

Conceptually, most other VCS store information as a list of file-based changes, i.e. a set of files and the changes made to each file over time (this is commonly described as delta-based version control).  Git thinks about its data more like a series of snapshots of a miniature filesystem. With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. To be efficient, if files have not changed, Git doesn’t store the file again, just a link to the previous identical file it has already stored.

### Three states
Each file in your working directory can be in one of two states: tracked or untracked. Tracked files are files that were in the last snapshot; they can be unmodified, modified, or staged. 
- **Modified** means that you have changed the file but have not committed it to your database yet.
- **Staged** means that you have marked a modified file in its current version to go into your next
commit snapshot.
- **Committed** means that the data is safely stored in your local database.

This leads us to the three main sections of a Git project: the working tree, the staging area, and the
Git directory.
- **The working tree** is a single checkout of one version of the project. These files are pulled out of the compressed database in the Git directory and placed on disk for you to use or modify.
- **The staging area** is a file, generally contained in your Git directory, that stores information about what will go into your next commit. Its technical name in Git parlance is the “index”, but the phrase “staging area” works just as well.
- **The Git directory** is where Git stores the metadata and object database for your project, and it is what is copied when you clone a repository from another computer.

The basic Git workflow goes something like this:
1. You modify files in your working tree.
2. You selectively stage just those changes you want to be part of your next commit, which adds
only those changes to the staging area.
3. You do a commit, which takes the files as they are in the staging area and stores that snapshot
permanently to your Git directory.

If a particular version of a file is in the Git directory, it’s considered committed. If it has been modified and was added to the staging area, it is staged. And if it was changed since it was checked out but has not been staged, it is modified. 

### Git Setup
Git comes with a tool called `git config` that lets you get and set configuration variables that control all aspects of how Git looks and operates. These variables can be stored in three different places:
1. `/etc/gitconfig` file: Contains values applied to every user on the system and all their repositories. If you pass the option `--system` to `git config`, it reads and writes from this file specifically. 
2. `~/.gitconfig` or `~/.config/git/config` file: Values specific personally to you, the user. You can make Git read and write to this file specifically by passing the `--global` option, and this affects all of the repositories you work with on your system.
3. `config` file in the Git directory (that is, `.git/config`) of whatever repository you’re currently using: Specific to that single repository. You can force Git to read from and write to this file with
the `--local option`, but that is in fact the default. 

Each level overrides values in the previous level, so values in `.git/config` trump those in
`/etc/gitconfig`. You can view all of your settings and where they are coming from using: `git config --list --show-origin`.
| `git config` | Options (e.g.) | Description |
| :-----| :----: | :---- |
| `user.name`  | "John Doe"| Name |
| `user.email` | johndoe@example.com | Email address |
| `core.editor`| emacs | Default text editors |

## Git Basics
### Getting a Git Repository
| Command | Description |
| :-----| :---- |
| `git init`  | Initializing a repository in an existing directory| 
| `git clone <url> <dir_name>` | Clone an existing git repository |

When you first clone a repository, all of your files will be tracked and unmodified. As you edit files, Git sees them as modified, because you’ve changed them since your last commit. As you work, you selectively stage these modified files and then commit all those staged changes, and the cycle repeats.

### Change the Status of Your Files
| Command | Description |
| :-----| :---- |
| `git status`  | Checking the status of files. <br> `-s` or `--short`: get a far more simplified output|
| `git diff` | Showing the exact lines added and removed — the patch, between working directory and the staging area. <br> `--staged` or `--cached`: check the difference between the staged area to last commit| 
| `git add <file>...` | Tracking new files/directiories, stage files, and to do other things like marking merge-conflicted files as resolved |
| `git rm <files/directories/file-glob patterns>...` | Removing a file from tracked files (more accurately, remove it from staging area) <br> `-f`: remove the modified file which had already been added to the staging area. <br> `--cached`: keep the file in working tree but remove it from staging area |
| `git mv file_from file_to`| Moving files, equivalently to renaming a file, and addressing the add/rm later before commit |
| `git commit -m "messgaes..` | Committing changes in staging area <br> `-a`: make Git automatically stage every file that is already tracked before doing the commit |
| `.gitignore` | Files to be ignored by Git|

The rules for the patterns you can put in the `.gitignore` file are as follows:
- Blank lines or lines starting with # are ignored.
- You can start patterns with a forward slash (/) to avoid recursivity.
- You can end patterns with a forward slash (/) to specify a directory.
- You can negate a pattern by starting it with an exclamation point (!).
- Standard glob patterns work, and will be applied recursively throughout the entire working tree. Glob patterns are like simplified regular expressions that shells use. An asterisk (`*`) matches zero or
more characters; `[abc]` matches any character inside the brackets (in this case a, b, or c); a question mark (`?`) matches a single character; and brackets enclosing characters separated by a hyphen (`[0-9]`) matches any character between them (in this case 0 through 9). You can also use two asterisks to match nested directories; `a/**/z` would match `a/z`, `a/b/z`, `a/b/c/z`, and so on.

### Viewing the Commit History
The `git log` lists the commits made in that repository in reverse chronological order.
| `git log` | Options (e.g.) | Description |
| :-----| :----: | :---- |
| `-p` or `--patch`  |  | Show the difference (the patch output) introduced in each commit |
| `--stat`|  | Show statistics for files modified in each commit |
| `--shortstat`|  | Display only the changed/insertions/deletions line from the `--stat` command. |
| `--name-only` |  | Show the list of files modified after the commit information|
| `--name-status` |  | Show the list of files affected with added/modified/deleted information as well|
| `--abbrev-commit`|  |  Show only the first few characters of the SHA-1 checksum instead of all 40|
| `--graph` | | Display an ASCII graph of the branch and merge history beside the log output |
| `--relative-date` | | Display the date in a relative format instead of using the full date format |
| `--pretty`| `--pretty=<option>` | Show commits in an alternate format, e.g. `oneline`, `short`, `full`, `fuller`, and `format` |
| | `--pretty=oneline` | Print each commit on a single line |
| | `--pretty=format:"%h - %an, %ar : %s"` | Print with self-specified log output format <br> `%H` Commit hash; `%h` Abbreviated commit hash; `%T` Tree hash; `%t` Abbreviated tree hash; `%P` Parent hashes; `%p` Abbreviated parent hashes; <br> `%an` Author name; `%ae` Author email; `%ad` Author date;  (format respects the --date=option); `%ar` Author date, relative; <br> `%cn` Committer name; `%ce` Committer email; `%cd` Committer date; `%cr` Committer date, relative `%s` Subject; |
| `--oneline` | | Shorthand for `--pretty=oneline` and `--abbrev-commit` used together |
| `-<n>` |  |  Limit the number of log entries displayed to show the last n commits |
|`--since`, `--after` | `--since=2.weeks` | Limit the commits to those made after the specified date |
|`--until`, `--before`| | Limit the commits to those made before the specified date |
|`--author`| | Only show commits in which the author entry matches the specified string |
| `--committer`| | Only show commits in which the committer entry matches the specified string|
|`--grep`  | |Only show commits with a commit message containing the string |
| `-S` || Only show commits adding or removing code matching the string |

## Reference
- [Git --everything-is-local](https://git-scm.com/)
- [Pro Git by Scott Chacon and Ben Straub](https://git-scm.com/book/en/v2)