
## exrc

Create a `.exrc` file containing `set` commands for changed `ex` options and key mappings. This saves the current option settings, allowing you to restore them later. `file` defaults to `.exrc` in the current directory if not specified. {Vim}

## move

```
[address] m destination
```

Move the lines specified by `address` to the `destination` address.

### Example

```
:.,/Note/m /END/       Move a text block to after the line containing “END”
```

## new

```
[count] new
```

Create a new window `count` lines high with an empty buffer. {Vim}

## next

```
n[!] [[+num] filelist]
```

Edit the next file from the command-line argument list. Use `args` to list these files. If `filelist` is provided, replace the current argument list with `filelist` and begin editing on the first file. With the `+` `num` argument, begin editing on line `num`. Alternatively, `num` may be a pattern, of the form /`pattern`/.

### Example

```
:n chap*               Start editing all “chapter” files
```

## nohlsearch

```
noh
```

Temporarily stop highlighting all matches to a search when using the `hlsearch` option. Highlighting is resumed with the next search. {Vim}

## number

```
[address] nu [count]
```
```
[address] # [count]
```

Print each line specified by `address`, preceded by its buffer line number. Use `#` as an alternate abbreviation for `number`. `count` specifies the number of lines to show, starting with `address`.

## only

```
on [!]
```

Make the current window be the only one on the screen. Windows open on modified buffers are not removed from the screen (hidden), unless you also use the `!` character. {Vim}

## open

```
[address] o [/pattern/]
```

Enter open mode (`vi`) at the lines specified by `address`, or at the lines matching `pattern`. Exit open mode with `Q`. Open mode lets you use the regular `vi` commands, but only one line at a time. It may be useful on very distant internet `ssh` connections.

## packadd

```
pa[!] packagename...
```

Search for a plugin directory matching `packagename` and load the plugin. See the Vim help for details. {Vim}

## preserve

```
pre
```

Save the current editor buffer as though the system were about to crash.

## previous

```
prev[!]
```

Edit the previous file from the command-line argument list. {Vim}

## print

```
[address] p [count]
```

Print the lines specified by `address`. `count` specifies the number of lines to print, starting with `address`. `P` is another abbreviation.

### Example

```
:100;+5p               Show line 100 and the next five lines
```

## put

```
[address] pu [char]
```

Place previously deleted or yanked lines from the named register specified by `char` to the line specified by `address`. If `char` is not specified, the last deleted or yanked text is restored.

## qall

```
qa[!]
```

Close all windows and terminate the current editing session. Use `!` to discard changes made since the last save. {Vim}

## quit

```
q[!]
```

Terminate the current editing session. Use `!` to discard changes made since the last save. If the editing session includes additional files in the argument list that were never accessed, quit by typing `q!` or by typing `q` twice. Vim closes the editing window only if there are still other windows open on the screen.

## read

```
[address] r filename
```

Copy the text of `filename` after the line specified by `address`. If `filename` is not specified, the current filename is used.

### Example

```
:0r $HOME/data         Read the named file in at the top of the current file
```

## read !command

```
[address] r !command
```

Read the output of `command` into the text after the line specified by `address`.

### Example

```
:$r !spell %           Place the results of spellchecking at the end of the file
```

## recover

```
rec [file]
```

Recover `file` from the system save area.

## redo

```
red
```

Restore last undone change. Same as CTRL-R. {Vim}

## resize

```
res [[±]num]
```

Resize the current window to be `num` lines high. If `+` or `-` is specified, increase or decrease the current window height by `num` lines. {Vim}

## rewind

```
rew[!]
```

Rewind the argument list and begin editing the first file in the list. Add a `!` to rewind the list even if the current file has not been saved since the last change.

## right

```
[address] ri [width]
```

Right-align lines specified by `address`, or the current line if no address is specified, to column `width`. Use the value of the `textwidth` option if no `width` is specified. {Vim}

## sbnext

```
[count] sbn [count]
```

Split the current window and begin editing the `count` next buffer from the buffer list. If no count is specified, edit the next buffer in the buffer list. {Vim}

## sbuffer

```
[num] sb [num]
```

Split the current window and begin editing buffer `num` from the buffer list in the new window. The buffer to be edited may also be specified by filename. If no buffer is specified, open the current buffer in the new window. {Vim}

## set

```
se parameter1 parameter2...
```

Set a value to an option with each `parameter`, or if no `parameter` is supplied, print all options that have been changed from their defaults. For Boolean options, each `parameter` can be phrased as `option` or `no` `option`; other options can be assigned with the syntax `option` `=` `value`. Specify `all` to list the current settings. The form `set` `option?` displays the value of `option`. See the tables that list `set` options in [Appendix B](#app02.html#vi8-app-b).

### Examples

```
:set nows wm=10
:set all
```

## shell

```
sh
```

Create a new shell. Resume editing when the shell terminates.

## snext

```
[count] sn [[+num] filelist]
```

Split the current window and begin editing the next file from the command-line argument list. If `count` is provided, edit the `count` next file. If `filelist` is provided, replace the current argument list with `filelist` and begin editing the first file. With the `+n` argument, begin editing on line `num`. Alternatively, `num` may be a pattern of the form /`pattern`/. {Vim}

## source

```
so file
```
