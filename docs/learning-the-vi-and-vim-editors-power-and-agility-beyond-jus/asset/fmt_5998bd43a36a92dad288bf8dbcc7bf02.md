
## Using Search Patterns
Another way that `ex` can address lines is by using search patterns. For example:

`:/pattern/d`

:
Delete the next line containing `pattern`.

`:/pattern/+d`

:
Delete the line below the next line containing `pattern`. (You could also use `+1` instead of `+` alone.)

`:/pattern1/,/pattern2/d`

:
Delete from the first line containing `pattern1` through the first line containing `pattern2`.

`:.,/pattern/m23`

:
Take the text from the current line (`.`) through the first line containing `pattern` and put it after line 23.

Note that a pattern is delimited by a slash both *before* and *after*. Any spaces or tab characters between the slashes are interpreted as part of the pattern to search for.

Use `?` as the delimiter instead of `/` if you want to search backward in the file.

If you make deletions by pattern with `vi` and `ex`, there is a difference in the way the two editors operate. Suppose your file `practice` contains the lines:

```
With a screen editor you can scroll the
page, move the cursor, delete lines, insert
characters, and more, while seeing results
of your edits as you make them.
```

To delete through the word `while`, do the following:

| Keystrokes | Results |
|---|---|
| `d/while` | `With a screen editor you can scroll the page, move the cursor, while seeing results of your edits as you make them.` The `vi` delete to `pattern` command deletes from the cursor up to the word `while` but leaves the remainder of both lines. |
| `:.,/while/d` | `With a screen editor you can scroll the of your edits as you make them.` The `ex` command deletes the entire range of addressed lines—in this case, both the current line and the line containing the `pattern`. All lines are deleted in their entirety. |

## Redefining the Current Line Position
Sometimes, using a relative line address in a command can give you unexpected results. For example, suppose the cursor is on line 1 and you want to print line 100 plus the five lines below it. If you type:

```
:100,+5 p
```

you’ll get an error message from Vim saying, “E16: Invalid range.” `vi` will tell you, “First address exceeds second.” The reason the command fails is that the second address is calculated relative to the current cursor position (line 1), so your command is really saying this:

```
:100,6 p
```

What you need is some way to tell the command to think of line 100 as the “current line,” even though the cursor is on line 1.

`ex` provides such a way. When you use a semicolon instead of a comma, the first line address is recalculated as the current line. For example, the command:

```
:100;+5 p
```

prints the desired lines. The `+5` is now calculated relative to line 100. A semicolon is useful with search patterns as well as with absolute addresses. For example, to print the next line containing `pattern`, plus the 10 lines that follow it, enter the command:

```
:/pattern/;+10 p
```

## Global Searches
You already know how to use `/` (slash) to search for patterns of characters in your files. `ex` has a global command, `g`, that lets you search for a pattern and display all lines containing the pattern when it finds them. The command `:g!` does the opposite of `:g`. Use `:g!` (or its synonym, `:v`) to search for all lines that do *not* contain `pattern`.

You can use the global command on all lines in the file, or you can use line addresses to limit a global search to specified lines or to a range of lines:

`:g/pattern`

:
Find (move to) the last occurrence of `pattern` in the file.

`:g/pattern/p`

:
Find and display all lines in the file containing `pattern`. Vim displays the line and then prompts you, “Press ENTER or type command to continue.”

`:g!/pattern/nu`

:
Find and display all lines in the file that don’t contain `pattern`; also display the line number for each line found.

`:60,124g/pattern/p`

:
Find and display any lines between lines 60 and 124 containing `pattern`.

As you might expect, `g` can also be used for global replacements. We’ll talk about that in [Chapter 6, “Global Replacement”](#ch06.html#vi8-ch-6).

## Combining ex Commands
You don’t always need to type a colon to begin a new `ex` command. In `ex`, the vertical bar (`|`) is a command separator, allowing you to combine multiple commands from the same `ex` prompt (in much the same way that a semicolon separates multiple commands at the shell prompt). When you use the `|`, keep track of the line addresses you specify. If one command affects the order of lines in the file, the next command does its work using the new line positions. For example:

`:1,3d | s/thier/their/`

:
Delete lines 1 through 3 (leaving you now on the top line of the file), and then make a substitution on the current line (which was line 4 before you invoked the `ex` prompt).

`:1,5 m 10 | g/pattern/nu`

:
Move lines 1 through 5 after line 10, and then display all lines (with numbers) containing `pattern`.

Note the use of spaces to make the commands easier to read.

# Saving and Exiting Files
![](assets/lvv8_0501.png)

Unlike IAmDevloper (see [Figure 5-1](#ch05.html#vi8-ch-5-fig-1)), you have learned the `vi` command `ZZ` to write (save) your file and quit. But you will frequently want to exit a file using `ex` commands, because these commands give you greater control. We’ve already mentioned some of these commands in passing. Now let’s take a more formal look:

`:w`

:
Write (save) the buffer to the file but do not exit. You can (and should) use `:w` throughout your editing session to protect your edits against system failure or a major editing error.

`:q`

:
Quit the editor (and return to the shell prompt).

`:wq`

:
Write the file and then quit the editor. The write happens unconditionally, even if the file was not changed. This updates the modification time of the file.

`:x`

:
Write the file and then quit (exit) the editor. The file is written only if it has been modified.^([3](#ch05.html#idm46601106286416))

The editor protects existing files and your edits in the buffer. For example, if you want to write your buffer to an existing file, you will get a warning. Likewise, if you have invoked `vi` on a file, made edits, and want to quit without saving the edits, `vi` gives you an error message, such as:

```
No write since last change.
```

These warnings can prevent costly mistakes, but sometimes you want to proceed with the command anyway. An exclamation point (`!`) after your command overrides the warning:

```
:w!
:q!
```

Vim helpfully tells you this when you try to quit without saving the file:

```
E37: No write since last change (add ! to override)
```

`:w!` can also be used to save edits in a file that was opened in read-only mode with `vi -R` or `view` (assuming you have write permission for the file).

`:q!` is an essential editing command that allows you to quit without affecting the original file, regardless of any changes you made in the session.^([4](#ch05.html#idm46601106252784)) The contents of the buffer are discarded.

## Renaming the Buffer
You can also use `:w` to save the entire buffer (the copy of the file you are editing) under a new filename.

Suppose you have a file `practice`, which contains 600 lines. You open the file and make extensive edits. You want to quit but also save both the old version of `practice` and your new edits for comparison. To save the edited buffer in a file called `practice.new`, give the command:

```
:w practice.new
```

Your old version, in the file `practice`, remains unchanged (provided that you didn’t previously use `:w`). You can now quit editing the new version by typing `:q`.

## Saving Part of a File
While editing, you will sometimes want to save just part of your file as a separate, new file. For example, you might have entered formatting codes and text that you want to use as a header for several files.
