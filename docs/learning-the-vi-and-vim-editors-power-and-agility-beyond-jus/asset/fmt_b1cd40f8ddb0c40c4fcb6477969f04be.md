
Passes Vim option information to the new window session (note that it must be preceded by two plus signs).

`cmd`

:

Passes a command for execution in the new window (note that it must be preceded by a single plus sign).

`file`

:

Specifies a file to edit in the new window.

For example, suppose you are editing a file and want to split the window to edit another file named `otherfile`. You want to ensure that the session uses a `fileformat` of `unix` (which ensures the use of a line feed to end each line instead of a carriage return and line feed combination). Finally, you want the window to be 15 lines tall. Enter:

```
:15split ++fileformat=unix otherfile
```

To simply split the screen, showing the same file in both windows and using all the current defaults, you can use the `vi` mode commands `CTRL-W S`, `CTRL-W SHIFT-S`, or `CTRL-W CTRL-S`.

### Tip

If you want windows to always split equally, set the `equalalways` option, preferably putting it in your `*.vimrc` to make it persistent over sessions. By default, setting `equalalways` splits both horizontal and vertical windows equally. Add the `eadirection` option (`hor`, `ver`, `both`, for horizontal, vertical, or both, respectively) to control which direction splits equally.

The following form of the `:split` command opens a new horizontal window as before, but with a slight nuance:

```
:[n]new [++opt] [+cmd] [file]
```

In addition to creating the new window, the `WinLeave`, `WinEnter`, `BufLeave`, and `BufEnter` autocommands execute. (For more information on autocommands, see the section [“Autocommands”](#ch12.html#vi8-ch-12-sect-2.1).)

Along with the horizontal split commands, Vim offers analogous vertical ones. So, for example, to split a vertical window, instead of `:split` or `:new`, use `:vsplit` and `:vnew`, respectively. The same optional parameters are available as for the horizontal split commands.

There are two horizontal split commands without vertical cousins:

`:sview` `filename`

:

Splits the screen horizontally to open a new window and sets the `readonly` option for that buffer. `:sview` requires the filename argument.

`:sfind [++opt] [+cmd] [filename]`

:

Works like `:split`, but looks for the `filename` in the `path`. If Vim does not find the file, it doesn’t split the window.

## Conditional Split Commands

Vim lets you specify a command that causes a window to open if a new file is found. `:topleft` `cmd` tells Vim to execute `cmd` and display a new window with the cursor at the top left if `cmd` opens a new file. The command can produce three different results:

* `cmd` splits the window horizontally, and the new window spans the top of the Vim window.
* `cmd` splits the window vertically, and the new window spans the left side of the Vim window.
* `cmd` causes no split but instead positions the cursor at the top left of the current window.

In addition to the conditional split command `:topleft`, Vim offers analogous commands `:vertical`, `:leftabove` and `:aboveleft`, `:rightbelow` and `:belowright`, and `:botright`. You can find detailed descriptions on their use with Vim’s `:help` command.

## Window Command Summary

[Table 10-1](#ch10.html#vi8-ch-10-tab-1) summarizes the commands for splitting windows.

Table 10-1. Summary of window commands

| `ex` command || `vi` command || Description |
| --- | --- | --- |
| 
```
:[n]split [++opt] [+cmd] [file]
```
 || 
`CTRL-W S`  
`CTRL-W SHIFT-S`  
`CTRL-W CTRL-S` 
 || 
Split the current window in two from side to side, placing the cursor in the new window. The optional `file` argument places that file in the newly created window. The windows are created as equal in size as possible, determined by free window space.
 |
| --- | --- | --- |
| 
```
:[n]new [++opt] [+cmd]
```
 || 
`CTRL-W N`  
`CTRL-W CTRL-N` 
 || 
Same as `:split`, but start the new window editing an empty file. Note that the buffer will have no name until one is assigned.
 |
| 
```
:[n]sview [++opt] [+cmd] [file]
```
 ||  || 
Read-only version of `:split`.
 |
| 
```
:[n]sfind [++opt] [+cmd] [file]
```
 ||  || 
Split the window and open `file` (if specified) in the new window. Look for `file` in the `path`.
 |
| 
```
:[n]vsplit [++opt] [+cmd] [file]
```
 || 
`CTRL-W V`  
`CTRL-W CTRL-V` 
 || 
Split the current window in two from top to bottom and open `file` (if specified) in the new window.
 |
| 
```
:[n]vnew [++opt] [+cmd]
```
 ||  || 
Vertical version of `:new`.
 |

# Moving Around Windows (Getting Your Cursor from Here to There)

It’s easy to move from window to window with a mouse in both `gvim` and Vim. `gvim` supports clicking with the mouse by default, whereas in Vim you can enable the behavior with the `mouse` option. A good default setting for Vim is `:set mouse=a`, which activates the mouse for all uses: command line, input, and navigation.

If you don’t have a mouse, or if you prefer to control your session from the keyboard, Vim provides a full set of navigation commands to move quickly and accurately among session windows. Happily, Vim uses the mnemonic prefix keystroke `CTRL-W` consistently for window navigation. The keystroke that follows defines the motion or other action, and the window navigation commands should be familiar to experienced `vi` and Vim users because they map closely to the same motion commands for editing.

Rather than describe each command and its behavior, we will consider an example. The command-synopsis table should then be self-explanatory.

To move from the current Vim window to the next one, type `CTRL-W J` (or `CTRL-W ↓` or `CTRL-W CTRL-J`). The `CTRL-W` is the mnemonic for “window” command, and the `j` is analogous to Vim’s `j` command, which moves the cursor to the next line.

[Table 10-2](#ch10.html#vi8-ch-10-tab-2) summarizes the window navigation commands.

### Note

As with many Vim and `vi` commands, these can be multiply executed by prefixing them with a count. For example, `3 CTRL-W J` tells Vim to jump to the third window down from the current window.

Table 10-2. Window navigation commands

| Command || Description |
| --- | --- |
| `CTRL-W W`  
`CTRL-W CTRL-W` || Move to the next window below or to the right. Note that this command, unlike `CTRL-W J`, cycles through all of the Vim windows. When the lowermost window is reached, Vim restarts the cycle and moves to the top leftmost window. |
| --- | --- |
| `CTRL-W ↓`  
`CTRL-W CTRL-J`  
`CTRL-W J` || Move to the next window down.Note that this command does not cycle through the windows; it simply moves down to the next window below the current window. If the cursor is in a window at the bottom of the screen, this command has no effect. Also, this command bypasses adjacent windows on its “way down”; for example, if there is a window to the right of the current window, the command does not jump across to the adjacent window. (Use `CTRL-W CTRL-W` to cycle through windows.) |
| `CTRL-W ↑`  
`CTRL-W CTRL-K`  
`CTRL-W K` || Move to the next window up. This is the opposite-direction counterpart to the `CTRL-W J` command. |
| `CTRL-W ←`  
`CTRL-W H`  
`CTRL-W BACKSPACE` || Move to the window to the left of the current window. |
| `CTRL-W →`  
`CTRL-W CTRL-L`  
`CTRL-W L` || Move to the window to the right of the current window. |
