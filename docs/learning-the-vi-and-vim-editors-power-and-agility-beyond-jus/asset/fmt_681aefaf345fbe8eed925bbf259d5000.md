
Even though you will be invoking `ex` commands from `vi` and will not be using them directly, it is worthwhile to spend a few minutes in `ex` itself. You will get a feel for how you need to tell the editor which line (or lines) to work on, as well as which command to execute.

After you have given a few `ex` commands in your `practice` file, you should invoke `vi` on that same file, so that you can see it in the more familiar visual mode. When working in `ex`, the command `:vi` gets you from `ex` to `vi`.

To invoke an `ex` command from `vi`, you must type the special bottom-line character `:` (colon). Then type the `ex` command and press ENTER to execute it. So, for example, in the `ex` editor you move to a line simply by typing the number of the line at the colon prompt. To move to line 6 of a file using this command from within `vi`, enter:

```
:6
```

And then press ENTER.

After the following exercise, we discuss `ex` commands only as they are executed from `vi`.

## Exercise: The ex Editor

This exercise should be run from inside a terminal emulator window:

| At the shell prompt, invoke the `ex` editor on a file called `practice`: || `ex practice` |
| --- | --- |
| A message appears: || `"practice" 8L, 261B Entering Ex mode. Type "visual" to go to Normal mode.` |
| Go to and print (display) the first line: || `:1` |
| Print (display) lines 1 through 3: || `:1,3` |
| Substitute `line` for `screen` on line 1: || `:1s/screen/line/` |
| Invoke the `vi` editor on file: || `:vi` |
| Go to the first line: || `:1` |

## Problem Getting to Visual Mode

While editing in `vi`, you accidentally end up in the `ex` editor.

A `Q` in the command mode of `vi` invokes `ex`. Any time you are in `ex`, the command `vi` returns you to the `vi` editor.

## Editing with ex

Many `ex` commands that perform normal editing operations have an equivalent in `vi` that does the job more simply. Obviously, you will use `dw` or `dd` to delete a single word or line rather than using the `delete` command in `ex`. However, when you want to make changes that affect numerous lines, you will find the `ex` commands more useful. They allow you to modify large blocks of text with a single command.

These `ex` commands are listed here, along with abbreviations for those commands. Remember that in `vi`, each `ex` command must be preceded with a colon. You can use the full command name or the abbreviation, whichever is easier to remember:

| Full name || Abbreviation || Meaning |
| --- | --- | --- |
| `delete` || `d` || Delete lines |
| --- | --- | --- |
| `move` || `m` || Move lines |
| `copy` || `co` || Copy lines |
|  || `t` || Copy lines (a synonym for `co`; short for “to”) |

You can separate the different elements of an `ex` command with spaces, if you find the command easier to read that way. For example, you can separate line addresses, patterns, and commands in this way. You cannot, however, use a space as a separator inside a pattern or at the end of a substitute command. (We show some examples later, in the section [“Combining ex Commands”](#ch05.html#vi8-ch-5-sect-2.7).)

### Line Addresses

For each `ex` editing command, you have to tell `ex` which line number(s) to edit. And for the `ex` `move` and `copy` commands, you also need to tell `ex` where to move or copy the text to.

You can specify line addresses in several ways:

* With explicit line numbers
* With symbols that help you specify line numbers relative to your current position in the file
* With search patterns as addresses that identify the lines to be affected

Let’s look at some examples.

#### Defining a Range of Lines

You can use line numbers to explicitly define a line or range of lines. Addresses that use explicit numbers are called absolute line addresses. For example:

`:3,18d`

Delete lines 3 through 18, inclusive.

`:160,224m23`

Move lines 160 through 224 to follow line 23. (Like `delete` and `put` in `vi`.)

`:23,29co100`

Copy lines 23 through 29 and put them after line 100. (Like `yank` and `put` in `vi`.)

To make editing with line numbers easier, you can also display all line numbers on the left of the screen. The command:

```
:set number
```

or its abbreviation

```
:set nu
```

displays line numbers. The file `practice` then appears:

```
1  With a line editor                  "screen" changed to "line" earlier
2  you can scroll the page,
3  move the cursor, delete lines,
4  insert characters, and more
```

The displayed line numbers are not saved when you write a file, and they do not print if you print the file. Remember that long lines wrap on the screen but still count as a single line with respect to their numbers. Line numbers are displayed either until you quit the editing session or until you disable the `set` option:

```
:set nonumber
```

or:

```
:set nonu
```

Vim lets you toggle the setting with:

```
:set nu!
```

To temporarily display the line numbers for a set of lines, you can use the `#` sign as a command. For example:

```
:1,10#
```

displays the line numbers from line 1 to line 10.

As described in the section [“Movement by Line Number”](#ch03.html#vi8-ch-3-sect-4), you can also use the `CTRL-G` command to display the current line number. You can thus identify the line numbers corresponding to the start and end of a block of text by moving to the start of the block, typing `CTRL-G`, and then moving to the end of the block and typing `CTRL-G` again.

Yet another way to identify line numbers is with the `=` command in `ex`:

`:=`

Print the total number of lines.

`:.=`

Print the line number of the current line. (The period is a shorthand that means “the current line”; we discuss this next.)

`:/pattern/=` 

Print the line number of the next line that matches `pattern`. (The search starts from the current line. The use of search patterns is described shortly, in the section [“Search Patterns”](#ch05.html#vi8-ch-5-sect-2.4).)

#### Line-Addressing Symbols

Besides line numbers, you can also use symbols for line addresses. A dot (`.`) stands for the current line, and `$` stands for the last line of the file. `%` stands for every line in the file; it’s the same as the combination `1,$`. These symbols can also be combined with absolute line addresses. For example:

`:.,$d`

Delete from the current line to the end of the file.

`:20,.m$`

Move from line 20 through the current line (inclusive) to the end of the file.

`:%d`

Delete all the lines in the file.

`:%t$`

Copy all the lines and place them at the end of the file (making a consecutive duplicate).

In addition to an absolute line address, you can specify an address relative to the current line. The symbols `+` and `-` work like arithmetic operators. When placed before a number, these symbols add or subtract the value that follows. For example:

`:.,.+20d`

Delete from the current line through the next 20 lines.

`:226,$m.-2`

Move lines 226 through the end of the file to two lines above the current line.

`:.,+20#`

Display line numbers from the current line to 20 lines further on in the file.

In fact, you don’t need to type the dot (`.`) when you use `+` or `-` because the current line is the assumed starting position.

Without a number following them, `+` and `-` are equivalent to +1 and –1, respectively.^([2](#ch05.html#idm46601106525168)) Similarly, `++` and `--` each extend the range by an additional line, and so on. Thus `:+++` moves you forward by three lines. The `+` and `-` can also be used with search patterns, as shown in the next section.

The number `0` stands for the top of the file (imaginary line zero). `0` is equivalent to `1-`, and both allow you to move or copy lines to the very start of a file, before the first line of existing text. For example:

`:-,+t0`

Copy three lines (the line above the cursor through the line below the cursor) and put them at the top of the file.

#### Search Patterns
