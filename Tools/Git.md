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
| `alias.<alias> '<>'`| `git config --global alias.unstage 'reset HEAD --'` | Make alias, '`git <alias>` is equivalently with `git <>` |

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

### Undoing Things
One of the common undos takes place when you commit too early and possibly forget to add some files, or you mess up your commit message. If you want to redo that commit, make the additional changes you forgot, stage them, and commit again using the --amend option: `git commit --amend`. This command takes your staging area and uses it for the commit. If you’ve made no changes since your last commit (for instance, you run this command immediately after your previous commit), then your snapshot will look exactly the same, and all you’ll change is your commit message.

**It’s important to understand that when you’re amending your last commit, you’re not so much fixing it as replacing it entirely with a new, improved commit that pushes the old commit out of the way and puts the new commit in its place. Effectively, it’s as if the previous commit never happened, and it won’t show up in your repository history.**


If you’ve changed two files and want to commit them as two separate changes, but you accidentally type git add * and stage them both. You can unstage one of the two with the git command: `git reset HEAD <file>...`. It’s true that git reset can be a dangerous command, especially if you provide the `--hard` flag. However, in the scenario that the file in your working directory is not touched, it’s relatively safe.

What if you realize that you don’t want to keep your changes to the CONTRIBUTING.md file? How can
you easily unmodify it — revert it back to what it looked like when you last committed (or initially
cloned, or however you got it into your working directory)? The git command `git checkout -- <file>..` discards changes in working directory. It’s important to understand that `git checkout -- <file> ` discards any local changes you made to that file are gone, and just replaced that file with the most recently-committed version. 

### Working with Remotes
The `git remote` lists the shortnames of each remote handle you’ve specified.
| `git remote` | Options (e.g.) | Description |
| :-----| :----: | :---- |
| `-v` |  | Shows the URLs that Git has stored for the shortname to be used when reading and writing to that remote |
| `add`| `git remote add <shortname> <url>` | Adds a new remote Git repository as a shortname you can reference easily|
| `show <remote>`| `git remote show <remote>` | Lists the URL for the remote repository as well as the tracking branch information |
| `rename <ori> <to>`| `git remote rename origin pb ` | Change a remote’s shortname form `<ori>` to `<to>`|
| `remove <remote>` or  `rm <remote>`| `git remote remove origin  ` | Removes a remote|


The `git fetch <remote>` fetch all the information from that remote repositories. It’s important to note that the `git fetch` command only downloads the data to your local repository — it doesn’t automatically merge it with any of your work or modify what you’re currently working on. You have to merge it manually into your work when you’re ready.

If your current branch is set up to track a remote branch, you can use the `git pull` command to automatically fetch and then merge that remote branch into your current branch.

The `git push <remote> <branch>` command pushs your master branch to your origin server. This command works only if you cloned from a server to which you have write access and if nobody has pushed in the meantime. If you and someone else clone at the same time and they push upstream and then you push upstream, your push will rightly be rejected. You’ll have to fetch their work first and incorporate it into yours before you’ll be allowed to push. 

### Tagging
Git supports two types of tags: lightweight and annotated. A **lightweight tag** is very much like a branch that doesn’t change — it’s just a pointer to a specific commit. **Annotated tags**, however, are stored as full objects in the Git database. They’re checksummed; contain the tagger name, email, and date; have a tagging message; and can be signed and verified with GNU Privacy Guard (GPG). It’s generally recommended that you create annotated tags so you can have all this information; but if you want a temporary tag or for some reason don’t want to keep the other information, lightweight tags are available too.

By default, the `git push` command doesn’t transfer tags to remote servers. You will have to explicitly push tags to a shared server after you have created them. This process is just like sharing remote branches — you can run `git push origin <tagname>` or  `git push origin --tags`. Note that pushing tags using `git push <remote> --tags` does not distinguish between lightweight and annotated tags. 

There are two common variations for deleting a tag from a remote server. The first variation is `git push <remote> :refs/tags/<tagname>`. The way to interpret the above is to read it as the null value before the colon is being pushed to the remote tag name, effectively deleting it. The second (and more intuitive) way to delete a remote tag is with: `git push origin --delete <tagname>`.

The `git tag` lists the existing tags.
| `git tag` | Options (e.g.) | Description |
| :-----| :----: | :---- |
| `-l <pattern>` or `--list <pattern>` |  | Lists tags that match a particular pattern|
| `-a <tag> <commit>`| `git tag -a v1.0 -m "V1.0"`| Creats an annotated tag|
| `<tag> <commit>`| `git tag v1.0` | Creats a lightweight tag, which do not support `-a`, `-m` or `-s` options|
| `-d <tag>`| `git tag -d v1.0` | Deletes a tag in local repositories|
| `remove <remote>` or  `rm <remote>`| `git remote remove origin  ` | Removes a remote|

If you want to view the versions of files a tag is pointing to, you can do a `git checkout` of that tag, although this puts your repository in “detached HEAD” state, which has some ill side effects. In “detached HEAD” state, if you make changes and then create a commit, the tag will stay the same, but your new commit won’t belong to any branch and will be unreachable, except by the exact commit hash. Thus, if you need to make changes — say you’re fixing a bug on an older version, for instance — you will generally want to create a branch: `git checkout -b version2 v2.0.0` and switch to a new branch 'version2'.  If you do this and make a commit, your version2 branch will be slightly different than your v2.0.0 tag since it will move forward with your new changes, so do be careful.

## Reference
- [Git --everything-is-local](https://git-scm.com/)
- [Pro Git by Scott Chacon and Ben Straub](https://git-scm.com/book/en/v2)