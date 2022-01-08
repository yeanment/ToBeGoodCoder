# Notes for Editor Vim
## Introduction
As programmers, we spend most of our time editing code, so it’s worth investing time mastering an editor that fits your needs. Here’s how you learn a new editor:
- Start with a tutorial (i.e. this lecture, plus resources that we point out)
- Stick with using the editor for all your text editing needs (even if it slows you down initially)
- Look things up as you go: if it seems like there should be a better way to do something, there probably is

Vim has a rich history; it originated from the Vi editor (1976), and it’s still being developed today. When programming, you spend most of your time reading/editing, not writing. For this reason, Vim is a modal editor: it has different modes for inserting text vs manipulating text. Vim is programmable (with Vimscript and also other languages like Python), and Vim’s interface itself is a programming language: **keystrokes** (with mnemonic names) are commands, and these commands are composable. Vim avoids the use of the mouse, because it’s too slow; Vim even avoids using the arrow keys because it requires too much movement.

Vim’s design is based on the idea that a lot of programmer time is spent reading, navigating, and making small edits, as opposed to writing long streams of text. For this reason, Vim has multiple operating modes.
- Normal: for moving around a file and making edits
- Insert: for inserting text
- Replace: for replacing text
- Visual (plain, line, or block): for selecting blocks of text
- Command-line: for running a command

Keystrokes have different meanings in different operating modes. For example, the letter x in Insert mode will just insert a literal character ‘x’, but in Normal mode, it will delete the character under the cursor, and in Visual mode, it will delete the selection.

In its default configuration, Vim shows the current mode in the bottom left. The initial/default mode is Normal mode. You’ll generally spend most of your time between Normal mode and Insert mode. You change modes by pressing `<ESC>` (the escape key) to switch from any mode back to Normal mode. From Normal mode, enter Insert mode with `i`, Replace mode with `R`, Visual mode with `v`, Visual Line mode with `V`, Visual Block mode with `<C-v>` (`Ctrl-V`, sometimes also written `^V`), and Command-line mode with `:`. You use the `<ESC>` key a lot when using Vim: consider remapping Caps Lock to Escape.

## Basics
Vim maintains a set of open files, called “buffers”. A Vim session has a number of tabs, each of which has a number of windows (split panes). Each window shows a single buffer. Unlike other programs you are familiar with, like web browsers, there is not a 1-to-1 correspondence between buffers and windows; windows are merely views. **A given buffer may be open in multiple windows, even within the same tab.** By default, Vim opens with a single tab, which contains a single window.

### Movement
Movements in Vim are also called “nouns”, because they refer to chunks of text.
- Basic movement: `hjkl` (left, down, up, right)
- Words: `w` (next word), `b` (beginning of word), `e` (end of word)
- Lines: `0` (beginning of line), `^` (first non-blank character), `$` (end of line)
- Screen: `H` (top of screen), `M` (middle of screen), `L` (bottom of screen)
- Scroll: `Ctrl-u` (up), `Ctrl-d` (down)
- File: `gg` (beginning of file), `G` (end of file)
- Line numbers: `:{number}` or `{number}G` (line {number})
- Misc: `%` (corresponding item)
- Find: `f{character}`, `t{character}`, `F{character}`, `T{character}`
    + find/to forward/backward {character} on the current line
    + `,` `/` ; for navigating matches
- Search: `/{regex}`, n / N for navigating matches

### Selection
Enter in visual modes and use movement for selection
- Visual: `v`
- Visual Line: `V`
- Visual Block: `Ctrl-v`

### Edition
Vim’s editing commands are also called “verbs”, because verbs act on nouns.
- `i` enter Insert mode
- `o` / `O` insert line below / above
- `d{motion}` delete {motion}
    + e.g. `dw` is delete word, `d$` is delete to end of line, `d0` is delete to beginning of line
- `c{motion}` change {motion}
    + e.g. `cw` is change word like `d{motion}` followed by `i`
- `x` delete character (equal do `dl`)
- `s` substitute character (equal to `xi`)
- Visual mode + manipulation
    + select text, `d` to delete it or `c` to change it
- `u` to undo, `<C-r>` to redo
- `y` to copy / “yank” (some other commands like d also copy)
- `p` to paste
    
### Counts
You can combine nouns and verbs with a count, which will perform a given action a number of times, e.g. `3w` move 3 words forward, `5j` move 5 lines down, `7dw` delete 7 words.

### Modifiers
You can use modifiers to change the meaning of a noun. Some modifiers are `i`, which means “inner” or “inside”, and `a`, which means “around”.
- `ci(` change the contents inside the current pair of parentheses
- `ci[` change the contents inside the current pair of square brackets
- `da'` delete a single-quoted string, including the surrounding single quotes

### Command-line mode
Command mode can be entered by typing `:` in Normal mode. This mode has many functionalities, including opening, saving, and closing files, and quitting Vim.
- `:q` quit (close window)
- `:w` save (“write”)
- `:wq` save and quit
- `:e {name of file}` open file for editing
- `:ls` show open buffers
- `:help {topic}` open help
    + `:help :w` opens help for the `:w` command
    + `:help w` opens help for the `w` movement

## Advanced Vim
### Search and replace
- `:s` (substitute) command (documentation).
    + `%s/foo/bar/g` replace foo with bar globally in file
    + `%s/\[.*\](\(.*\))/\1/g` replace named Markdown links with plain URLs

### Multiple windows
- `:sp` / `:vsp` to split windows 

### Macros
- `q{character}` to start recording a macro in register {character}
- `q` to stop recording
- `@{character}` replays the macro
- `{number}@{character}` executes a macro {number} times
- Macros can be recursive
    + first clear the macro with `q{character}q`
    + record the macro, with `@{character}` to invoke the macro recursively (will be a no-op until recording is complete)
- `Gdd`, `ggdd` delete first and last lines

## Customizing Vim
Vim is customized through a plain-text configuration file in `~/.vimrc` (containing Vimscript commands). There are probably lots of basic settings that you want to turn on. A well-documented basic config is shown here [`.vimrc`](Tools/.vimrc). Vim is heavily customizable, and it’s worth spending time exploring customization options. You can look at people’s dotfiles on GitHub for inspiration. There are lots of good blog posts on this topic too. 

There are tons of plugins for extending Vim. Contrary to outdated advice that you might find on the internet, you do not need to use a plugin manager for Vim (since Vim 8.0). Instead, you can use the built-in package management system. Simply create the directory `~/.vim/pack/vendor/start/`, and put plugins in there (e.g. via git clone).

Here are some of our favorite plugins:
- 'ctrlp.vim': fuzzy file finder
- `ack.vim`: code search
- `nerdtree`: file explorer
- `vim-easymotion`: magic motions


## Resources
- vimtutor is a tutorial that comes installed with Vim - if Vim is installed, you should be able to run vimtutor from your shell
- [Vim Adventures](https://vim-adventures.com/) is a game to learn Vim
- [Vim Tips Wiki](http://vim.wikia.com/wiki/Vim_Tips_Wiki)
- [Vim Advent Calendar](https://vimways.org/2019/) has various Vim tips
- [Vim Golf](http://www.vimgolf.com/) where the programming language is Vim’s UI
- [Vi/Vim Stack Exchange](https://vi.stackexchange.com/)
- [Vim Screencasts](http://vimcasts.org/)
- [Practical Vim (book)](https://pragprog.com/titles/dnvim2/)

## Reference
- [Editors (Vim)](https://missing.csail.mit.edu/2020/editors/)
