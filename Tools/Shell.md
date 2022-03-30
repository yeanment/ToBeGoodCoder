# The Shell in Computers
The shell is the textual interface with computers, which allows you to run programs, give them input, and inspect their output in a semi-structured way. The **bash** is one of the most widely used shells, and its syntax is similar to what you will see in many other shells. 


## Shell Basis
Examples on interacting with the shell.
```console
simon:~$ date
Fri 10 Jan 2020 11:49:31 AM EST
simon:~$ echo hello
hello
simon:~$ pwd
/home/simon
simon:~$ pwd ..
/home
simon:~$ ls /
bin boot dev etc home ...
simon:~$ ls -l / | tail -n1
drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
simon:~$ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
219
```

1. The shell parses the command by splitting it by whitespace, and then runs the program indicated by the first word, supplying each subsequent word as an argument that the program can access. If you want to provide an argument that contains spaces or other special characters (e.g., a directory named "My Photos"), you can either quote the argument with `'` or `"` (`"My Photos"`), or escape just the relevant characters with `\` (`My\ Photos`).

2. The shell search program in `$PATH` when given a command, where the paths in `$PATH` are seperated by `:`.

3. A path that starts with `/` is called an _absolute_ path. Any other path is a _relative_ path relative to the current working directory. In a path, `.` refers to the current directory, and `..` to its parent directory.

4. The permissions of the file/dirs can be described by a groups of three characters (`rwx`), i.e. read, write, execute. A `-` indicates that the given principal does not have the given permission. 

5. In the shell, programs have two primary "streams" associated with them: input and output stream, and those streams can be redirected. The simplest form of redirection is `< file` and `> file`, which let you rewire the input and output streams of a program to a file respectively. You can also use `>>` to append to a file. The `|` operator lets you "chain" programs such that the output of one is the input of another.

## Shell Scripting
Most shells have their own scripting language with variables, control flow and its own syntax. What makes shell scripting different from other scripting programming language is that it is optimized for performing shell-related tasks.

1. Assign and access variables, `foo=bar` and `$foo`. Note that the space in shell normally acts as splitting markers.

1. Strings in bash can be defined with `'` and `"` delimiters, but they are not equivalent. Strings delimited with `'` are literal strings and will not substitute variable values whereas `"` delimited strings will.

    ```bash
    foo=bar
    echo "$foo"
    # prints bar
    echo '$foo'
    # prints $foo
    ```

1. Bash uses a variety of special variables to refer to arguments, error codes, and other relevant variables. A more comprehensive list can be found [here](https://tldp.org/LDP/abs/html/special-chars.html).
    - `$0` - Name of the script
    - `$1` to `$9` - Arguments to the script. `$1` is the first argument and so on.
    - `$@` - All the arguments
    - `$#` - Number of arguments
    - `$?` - Return code of the previous command
    - `$$` - Process identification number (PID) for the current script
    - `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing `sudo !!`
    - `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by `.` or `Alt+.`

    ```bash
    # Function example
    mcd () {
        mkdir -p "$1"
        cd "$1"
    }
    ```
    
1. Commands will often return output using `STDOUT`, errors through `STDERR`, and a Return Code to report errors in a more script-friendly manner. A value of 0 usually means everything went OK; anything different from 0 means an error occurred.

1. Exit codes can be used to conditionally execute commands using `&&` (and operator) and `||` (or operator), both of which are [short-circuiting](https://en.wikipedia.org/wiki/Short-circuit_evaluation) operators. The `true` program will always have a 0 return code and the `false` command will always have a 1 return code. Commands can also be separated within the same line using a semicolon `;`.

    ```bash
    false || echo "Oops, fail"
    # Oops, fail
    true || echo "Will not be printed"
    #
    true && echo "Things went well"
    # Things went well
    false && echo "Will not be printed"
    #
    true ; echo "This will always run"
    # This will always run
    false ; echo "This will always run"
    # This will always run
    ```

1. Use the output of a command as a variable with _command substitution_, e.g. `$( CMD )` which will execute `CMD`, get the output of the command and substitute it in place. A lesser known similar feature is _process substitution_, `<( CMD )` will execute `CMD` and place the output in a temporary file and substitute the `<()` with that file's name. This is desired when commands expect values to be passed by file instead of by STDIN. For example, `diff <(ls foo) <(ls bar)` will show differences between files in dirs  `foo` and `bar`.

    ```bash
    #!/bin/bash

    echo "Starting program at $(date)" # Date will be substituted

    echo "Running program $0 with $# arguments with pid $$"

    for file in "$@"; do
        grep foobar "$file" > /dev/null 2> /dev/null
        # When pattern is not found, grep has exit status 1
        # We redirect STDOUT and STDERR to a null register 
        if [[ $? -ne 0 ]]; then
            echo "File $file does not have any foobar, adding one"
            echo "# foobar" >> "$file"
        fi
    done
    ```

1. When performing comparisons in bash, try to use double brackets `[[ ]]` in favor of simple brackets `[ ]`. Chances of making mistakes are lower although it won't be portable to `sh`. A more detailed explanation can be found [here](http://mywiki.wooledge.org/BashFAQ/031).

1. Expanding expressions by carrying out filename expansion. These techniques are often referred to as shell _globbing_.
    - Wildcards - Use `?` and `*` to match one or any amount of characters respectively. 
    - Curly braces `{}` - Use curly braces for bash to expand this automatically. 

    ```bash
    cp /path/to/project/{foo,bar,baz}.sh /newpath
    # Will expand to
    cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

    # Globbing techniques can also be combined
    mv *{.py,.sh} folder
    # Will move all *.py and *.sh files

    # This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h
    touch {foo,bar}/{a..h}

    # Show differences between files in foo and bar
    diff <(ls foo) <(ls bar)
    ```

1. Tools like [shellcheck](https://github.com/koalaman/shellcheck) help you find errors in your sh/bash scripts.

1. Sometimes manpages can provide overly detailed descriptions of the commands, making it hard to decipher what flags/syntax to use for common use cases. [TLDR pages](https://tldr.sh/) are a nifty complementary solution that focuses on giving example use cases of a command so you can quickly figure out which options to use.

1. Differences between shell functions and scripts that you should keep in mind are:
    - Functions have to be in the same language as the shell, while scripts can be written in any language. 
    - Functions are loaded once when their definition is read. Scripts are loaded every time they are executed. 
    - Functions are executed in the current shell environment whereas scripts execute in their own process. Thus, functions can modify environment variables, e.g. change your current directory, whereas scripts can't. Scripts will be passed by value environment variables that have been exported using [`export`](https://www.man7.org/linux/man-pages/man1/export.1p.html)
    - As with any programming language, functions are a powerful construct to achieve modularity, code reuse, and clarity of shell code. Often shell scripts will include their own function definitions.


## Shell Examples 

### Finding files
All UNIX-like systems come packaged with [`find`](https://www.man7.org/linux/man-pages/man1/find.1.html), a great shell tool to find files. `find` will recursively search for files matching some criteria. Some examples:

```bash
# Find all directories named src
find . -name src -type d 
# Find all directories named src (case insensitive)
find . -iname src -type d 
# Find all python files that have a folder named test in their path
find . -path '*/test/*.py' -type f
# Find all files modified in the last day
find . -mtime -1
# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.tar.gz'
# Delete all files with .tmp extension
find . -name '*.tmp' -exec rm {} \;
# Find all PNG files and convert them to JPG
find . -name '*.png' -exec convert {} {}.jpg \;
```

Most would agree that `find` are good, but some of you might be wondering about the efficiency of looking for files every time versus compiling some sort of index or database for quickly searching. Therefore one trade-off between the two is speed vs freshness. [`locate`](https://www.man7.org/linux/man-pages/man1/locate.1.html) uses a database that is updated using [`updatedb`](https://www.man7.org/linux/man-pages/man1/updatedb.1.html). In most systems, `updatedb` is updated daily via [`cron`](https://www.man7.org/linux/man-pages/man8/cron.8.html). `find` and similar tools can also find files using attributes such as file size, modification time, or file permissions, while `locate` just uses the file name. A more in-depth comparison can be found [here](https://unix.stackexchange.com/questions/60205/locate-vs-find-usage-pros-and-cons-of-each-other).

### Finding code
A common scenario is wanting to search for all files that contain some pattern, along with where in those files said pattern occurs. To achieve this, most UNIX-like systems provide [`grep`](https://www.man7.org/linux/man-pages/man1/grep.1.html), a generic tool for matching patterns from the input text.
    - `-C` for getting **C**ontext around the matching line and `-v` for in**v**erting the match, i.e. print all lines that do **not** match the pattern. For example, `grep -C 5` will print 5 lines before and after the match.
    - `-R` will **R**ecursively go into directories and look for files for the matching string.

### Finding shell commands
The `history` command will let you access your shell history programmatically. It will print your shell history to the standard output. In most shells, you can make use of `Ctrl+R` to perform backwards search through your history by typeing a substring you want to match for commands in your history.

You can modify your shell's history behavior, like preventing commands with a leading space from being included. This comes in handy when you are typing commands with passwords or other bits of sensitive information. To do this, add `HISTCONTROL=ignorespace` to your `.bashrc` or `setopt HIST_IGNORE_SPACE` to your `.zshrc`.

### Debugging output
Write a bash script that runs the following script until it fails and captures its standard output and error streams to files and prints everything at the end.
```bash
#!/usr/bin/env bash
n=$(( RANDOM % 100 ))
if [[ n -eq 42 ]]; then
   echo "Something went wrong"
   >&2 echo "The error was using magic numbers"
   exit 1
fi
echo "Everything went according to plan"
```

```bash
#!/usr/bin/env bash
count=0
until [[ "$?" -ne 0 ]];
do
  count=$((count+1))
  ./random.sh &> out.txt
done

echo "found error after $count runs"
cat out.txt
```

## Shell Tools
### Job Control
- `SIGINT`: `Ctrl-C`
- `SIGQUIT`: `Ctrl-\`
- `SIGTERM`: `kill -TERM <PID>` or `kill -TERM %<JOBID>`
- `SIGSTOP`: `Ctrl-Z` (pause, `bg` (or run program with suffix `&`) or `fg` to resume a process in background / foreground).
    + To refer to the last backgrounded job you can use the `$!` special parameter.
- `SIGHUB`: close the terminal, and the backgrounded processes which are still children processes of your terminal, will die if you close the terminal.
    + `nohub`: run the program to ignore `SIGHUB`
    + `disown`: if the process has already been started
- `SIGKILL`: a special signal that cannot be captured by the process and it will always terminate it immediately.

**Note**: You can learn more about these and other signals [here](https://en.wikipedia.org/wiki/Signal_(IPC)) or typing [`man signal`](https://www.man7.org/linux/man-pages/man7/signal.7.html) or `kill -l`.

### Terminal Multiplexers
Terminal multiplexers like [`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html) allow you to multiplex terminal windows using panes and tabs so you can interact with multiple shell sessions. Moreover, terminal multiplexers let you detach a current terminal session and reattach at some point later in time. This can make your workflow much better when working with remote machines since it avoids the need to use `nohup` and similar tricks.
`tmux` expects you to know its keybindings, and they all have the form `<C-b> x` where that means (1) press `Ctrl+b`, (2) release `Ctrl+b`, and then (3) press `x`. `tmux` has the following hierarchy of objects:
- **Sessions** - a session is an independent workspace with one or more windows
    + `tmux` starts a new session.
    + `tmux new -s NAME` starts it with that name.
    + `tmux ls` lists the current sessions
    + Within `tmux` typing `<C-b> d`  detaches the current session
    + `tmux a` attaches the last session. You can use `-t` flag to specify which

- **Windows** - Equivalent to tabs in editors or browsers, they are visually separate parts of the same session
    + `<C-b> c` Creates a new window. To close it you can just terminate the shells doing `<C-d>`
    + `<C-b> N` Go to the _N_ th window. Note they are numbered
    + `<C-b> p` Goes to the previous window
    + `<C-b> n` Goes to the next window
    + `<C-b> ,` Rename the current window
    + `<C-b> w` List current windows

- **Panes** - Like vim splits, panes let you have multiple shells in the same visual display.
    + `<C-b> "` Split the current pane horizontally
    + `<C-b> %` Split the current pane vertically
    + `<C-b> <direction>` Move to the pane in the specified _direction_. Direction here means arrow keys.
    + `<C-b> z` Toggle zoom for the current pane
    + `<C-b> [` Start scrollback. You can then press `<space>` to start a selection and `<enter>` to copy that selection.
    + `<C-b> <space>` Cycle through pane arrangements.

For further reading, [here](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) is a quick tutorial on `tmux` and [this](http://linuxcommand.org/lc3_adv_termmux.php) has a more detailed explanation that covers the original `screen` command. You might also want to familiarize yourself with [`screen`](https://www.man7.org/linux/man-pages/man1/screen.1.html), since it comes installed in most UNIX systems.

### Aliases
A shell alias is a short form for another command that your shell will replace automatically for you. For instance, an alias in bash has the following structure:
```bash
alias alias_name="command_to_alias arg1 arg2"
# To ignore an alias run it prepended with \
\ls
# Or disable an alias altogether with unalias
unalias alias_name
# To get an alias definition just call it with alias
alias ll
```
Note that aliases do not persist shell sessions by default.
To make an alias persistent you need to include it in shell startup files, like `.bashrc` or `.zshrc`, which we are going to introduce in the next section.

### Dotfiles
- `bash` - `~/.bashrc`, `~/.bash_profile`
- `git` - `~/.gitconfig`
- `vim` - `~/.vimrc` and the `~/.vim` folder
- `ssh` - `~/.ssh/config`
- `tmux` - `~/.tmux.conf`
They should be in their own folder,
under version control, and **symlinked** into place using a script. This has
the benefits of: **Easy installation**,  **Portability**, **Synchronization**, **Change tracking**.

Resources of popular dotfiles:
- The most popular one on [github](https://github.com/mathiasbynens/dotfiles)
- [Your unofficial guide to dotfiles on GitHub](https://dotfiles.github.io/).


### SSH
Key-based authentication exploits public-key cryptography to prove to the server that the client owns the secret private key without revealing the key. 
- Generation of key: `ssh-keygen`
    + `ssh-keygen -o -a 100 -t ed25519 -f~/.ssh/id_ed25519`
- Authenrication of key: `ssh` will look into `.ssh/authorized_keys` to determine which clients it should let in.
    + `cat .ssh/id_ed25519.pub | ssh foobar@remote 'cat >> ~/.ssh/authorized_keys'`
    + `ssh-copy-id -i .ssh/id_ed25519 foobar@remote`
- File transfer: 
    + `ssh+tee`, the simplest is to use `ssh` command execution and STDIN input by doing `cat localfile | ssh remote_server tee serverfile`. 
    + [`scp`](https://www.man7.org/linux/man-pages/man1/scp.1.html) when copying large amounts of files/directories, the secure copy `scp` command is more convenient since it can easily recurse over paths. The syntax is `scp path/to/local_file remote_host:path/to/remote_file`
    + [`rsync`](https://www.man7.org/linux/man-pages/man1/rsync.1.html) improves upon `scp` by detecting identical files in local and remote, and preventing copying them again. It also provides more fine grained control over symlinks, permissions and has extra features like the `--partial` flag that can resume from a previously interrupted copy. `rsync` has a similar syntax to `scp`.
- Port forwarding: Local Port Forwarding and Remote Port Forwarding (see the pictures for more details, credit of the pictures from [this StackOverflow post](https://unix.stackexchange.com/questions/115897/whats-ssh-port-forwarding-and-whats-the-difference-between-ssh-local-and-remot)).
    + **Local Port Forwarding**: `-L` Specifies that the given port on the local (client) host is to be forwarded to the given host and port on the remote side.
        * `ssh -L sourcePort:forwardToHost:onPort connectToHost` means: connect with ssh to connectToHost, and forward all connection attempts to the local sourcePort to port onPort on the machine called forwardToHost, which can be reached from the connectToHost machine.
        *  The most common scenario is local port forwarding, where a service in the remote machine listens in a port and you want to link a port in your local machine to forward to the remote port. For example, if we execute  `jupyter notebook` in the remote server that listens to the port `8888`. Thus, to forward that to the local port `9999`, we would do `ssh -L 9999:localhost:8888 foobar@remote_server` and then navigate to `locahost:9999` in our local machine.
    + **Remote Port Forwarding**: `-R` Specifies that the given port on the remote (server) host is to be forwarded to the given host and port on the local side.
        * `ssh -R sourcePort:forwardToHost:onPort connectToHost` means: connect with ssh to connectToHost, and forward all connection attempts to the remote sourcePort to port onPort on the machine called forwardToHost, which can be reached from your local machine.
- SSH configuration (`~/.ssh/config` & `/etc/ssh/sshd_config`)
    + Using the ~/.ssh/config file over aliases makes other programs like `scp`, and `rsync` are able to read it as well and convert the settings into the corresponding flags.
    + Here you can make changes like disabling password authentication, changing ssh ports, enabling X11 forwarding, etc.

### `sed`

### `awk`
`awk` is a programming language that just happens to be really good at processing text streams. `awk` programs take the form of an optional pattern plus a block saying what to do if the pattern matches a given line. The default pattern (which we used above) matches all lines. Inside the block, `$0` is set to the entire line's contents, and `$1` through `$n` are set to the `n`th _field_ of that line, when separated by the `awk` field separator (whitespace by default, change with `-F`). 

```bash
awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }' | wc -l
history | awk '{$1="";print substr($0,2)}' | sort | uniq -c | sort -n | tail -n 10
```

```awk
BEGIN { rows = 0 }
$1 == 1 && $2 ~ /^c[^ ]*e$/ { rows += $1 }
END { print rows }
```
`BEGIN` is a pattern that matches the start of the input (and `END` matches the end). Now, the per-line block just adds the count from the first field (although it'll always be 1 in this case), and then we print it out at the end. In fact, we _could_ get rid of `grep` and `sed` entirely, because `awk` [can do it all](https://backreference.org/2010/02/10/idiomatic-awk/).

## References
+ [Missing Semester MIT 2020S](https://missing.csail.mit.edu/)