
When you type commands, text jumps around on the screen and nothing works the way it’s supposed to.

Make sure you’re not typing the `J` command when you mean `j`.

You may have hit the `CAPS LOCK` key without noticing it; `vi` and `Vim` are case sensitive—that is, uppercase commands (e.g., `I`, `A`, `J`) are different from lowercase commands (`i`, `a`, `j`)—and if you hit this key, all your commands are interpreted not as lowercase but as uppercase commands. Press the `CAPS LOCK` key again to return to lowercase, press `ESC` to ensure that you are in command mode, and then type either `U` to restore the last line changed or `u` to undo the last command. You’ll probably also have to do some additional editing to fully restore the garbled part of your file.

# Mode Indicators

As you know by now, the editor has two modes—command mode and insert mode. Usually, you can’t tell by looking at the screen which mode you’re in. Furthermore, it’s often useful to know where in the file you are, without having to use the `CTRL-G` or `ex` `:.=` commands.

Two options address these issues: `showmode` and `ruler`. `Vim` has both, while the “Heirloom” and Solaris `/usr/xpg7/bin` versions of `vi` have the `showmode` option.

[Table 2-1](#ch02.html#vi8-ch-2-tab-1) lists the special features in each editor.

Table 2-1. Position and mode indicators

| Editor || With ruler, displays || With showmode, displays |
| --- | --- | --- |
| `vi` || N/A || Separate mode indicators for open, input, insert, append, change, replace, replace one character, and substitute modes |
| --- | --- | --- |
| `Vim` || Row and column || Insert, replace, and visual mode indicators |

# Review of Basic vi Commands

[Table 2-2](#ch02.html#vi8-ch-2-tab-2) presents a few of the commands you can perform by combining the commands `c`, `d`, and `y` with various text objects. The last two rows show additional commands for editing. Tables [2-3](#ch02.html#vi8-ch-2-tab-3) and [2-4](#ch02.html#vi8-ch-2-tab-4) list some other basic commands. [Table 2-5](#ch02.html#vi8-ch-2-tab-5) summarizes the rest of the commands described in this chapter.

Table 2-2. Edit commands

| Text object || Change || Delete || Copy (yank) |
| --- | --- | --- | --- |
| One word || `cw` || `dw` || `yw` |
| --- | --- | --- | --- |
| Two words, whitespace separated || `2cW` or `c2W` || `2dW` or `d2W` || `2yW` or `y2W` |
| Three words back || `3cb` or `c3b` || `3db` or `d3b` || `3yb` or `y3b` |
| One line || `cc` || `dd` || `yy` or `Y` |
| To end of line || `c$` or `C` || `d$` or `D` || `y$` |
| To beginning of line || `c0` || `d0` || `y0` |
| Single character || `r` || `x` or `X` || `yl` or `yh` |
| Five characters || `5s` || `5x` || `5yl` |

Table 2-3. Movement

| Movement || Commands |
| --- | --- |
| ←, ↓, ↑, → || `h`, `j`, `k`, `l` |
| --- | --- |
| ←, ↓, ↑, → || `BACKSPACE`, `CTRL-N` and `ENTER`, `CTRL-P`, `space bar` |
| To first character of next line || `+` |
| To first character of previous line || `-` |
| To end of word || `e` or `E` |
| Forward by word || `w` or `W` |
| Backward by word || `b` or `B` |
| To end of line || `$` |
| To beginning of line || `0` |
| To a particular line || `G` |

Table 2-4. Other operations

| Operations || Commands |
| --- | --- |
| Place text from register || `p` or `P` |
| --- | --- |
| Start `vi`, open file if specified || `vi` `file` |
| Start `Vim`, open file if specified || `vim` `file` |
| Save edits, quit file || `ZZ` |
| No saving of edits, quit file || `:q!` `ENTER` |

Table 2-5. Text creation, deletion, and manipulation commands

| Editing action || Command |
| --- | --- |
| Insert text at current position || `i` |
| --- | --- |
| Insert text at beginning of line || `I` |
| Append text at current position || `a` |
| Append text to end of line || `A` |
| Open new line below cursor for new text || `o` |
| Open new line above cursor for new text || `O` |
| Put deleted text after cursor or below current line || `p` |
| Put deleted text before cursor or above current line || `P` |
| Replace the character under the cursor || `r` |
| Overstrike existing characters with new text || `R` |
| Delete the current character and enter insert mode || `s` |
| Delete line and substitute text || `S` |
| Delete the character under the cursor || `x` |
| Delete the character in front of the cursor || `X` |
| Join current and next line || `J` |
| Toggle case || `~` |
| Repeat last action || `.` |
| Undo last change || `u` |
| Restore line to original state || `U` |

You can get by in `vi` and `Vim` using only the commands listed in these tables. However, to harness the real power of the editor (and increase your own productivity), you will need more tools. The following chapters describe those tools.

^([1](#ch02.html#idm46601097429040-marker)) Some versions show that you’re in input mode in the status line. This is discussed in the section [“Mode Indicators”](#ch02.html#vi8-ch-2-sect-6).

^([2](#ch02.html#idm46601097400672-marker)) If you’ve muted the sound on your system, you’ll have to work harder to determine what mode you’re in by watching how the editor responds to what you type.

^([3](#ch02.html#idm46601097779888-marker)) `Vim`, with `nocompatible` set, allows you to “space past” the end of the line to the next one with `l` or the `space bar`. This is likely to be the default.

^([4](#ch02.html#idm46601097761952-marker)) Pictures of this terminal’s keyboard can be found easily by searching on the internet. —ADR

^([5](#ch02.html#idm46601097930784-marker)) Robert P. J. Day points out that, unlike `dw` versus `de`, the commands `cw` and `ce` do the same thing.

^([6](#ch02.html#idm46601097868208-marker)) The mnemonic for `x` is that it is supposedly like “x-ing out” mistakes with a typewriter. Of course, who uses a typewriter anymore?

^([7](#ch02.html#idm46601097836832-marker)) Older `vi` documentation calls this the *deletion buffer*. We use `Vim`’s term, *register*, to avoid confusion with the buffers that hold file contents.

# Chapter 3. Moving Around in a Hurry

You will not, of course, just create new files. You’ll spend a lot of your time editing existing files. You rarely want to simply open to the first line in the file and move through it line by line; you sometimes want to get to a specific place in a file and start working.

All edits start with you moving the cursor to where you want to begin the edit (or, with `ex` line editor commands, by identifying the line numbers to be edited). This chapter shows you how to think about movement in a variety of ways (by screens, by text, by patterns, or by line numbers). There are many ways to move around in `vi` and `Vim`, since editing speed depends on getting to your destination with only a few keystrokes.

This chapter covers:
* Movement by screens
* Movement by text blocks
* Movement by searches for patterns
* Movement by line number

## Movement by Screens

When you read a book, you think of “places” in the book in terms of pages: the page where you stopped reading, or the page number in an index. You don’t have this convenience when you’re editing files. Some files take up only a few lines, and you can see the whole file at once. But many files have hundreds (or thousands!) of lines.

You can think of a file as text on a long roll of paper. The screen is a window of (often) 24 lines of text on that long roll.^([1](#ch03.html#idm46601107890864))

In insert mode, as you fill up the screen with text, you will end up typing on the bottom line of the screen. When you reach the end and press `ENTER`, the top line rolls out of sight, and a blank line appears on the bottom of the screen for new text. This is called *scrolling*.

In command mode, you can move through a file to see any text in it by scrolling the screen ahead or back. And since cursor movements can be multiplied by numeric prefixes, you can move quickly to anywhere in your file.

### Scrolling the Screen
