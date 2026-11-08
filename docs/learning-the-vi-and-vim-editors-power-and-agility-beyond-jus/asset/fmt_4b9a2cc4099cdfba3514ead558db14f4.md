
This calls our user-defined `LastMod()` function.

`|`:
Same as previous.

`'s`:
Return to the line we marked with the name `s`.

It’s worth verifying this script by editing a `*.html` file, adding the line:

```
Last modified:□
```

and issuing the `w` command.

### Note

This example is useful, but it’s not canonically correct in relation to its stated goal of substituting the HTML `<meta>` statement. More appropriately, if it were indeed meant to address a `<meta>` statement, the substitution should look for the `content=…` part of the `<meta>` statement. Still, this example is a good start toward solving that problem and is a useful example for other file types.

## More About Variables

We now discuss in more detail what makes up Vim variables and how they’re used. Vim has five variable types:

Number:
A signed 32-bit number. This number can be represented in decimal, hexadecimal (e.g., `0xffff`), or octal (e.g., `0177`). If supported by the compiler, Vim supports 64-bit numbers. The `ex` command `:version` shows whether 64-bit is supported or compiled into the editor. See [Figure 12-10](#ch12.html#vi8-ch-12-fig-10).

![](assets/lvv8_1210.png)

String:
A string of characters.

Funcref:
A reference to a function.

List:
This is Vim’s version of an array. It is an ordered list of values and can contain any mix of Vim values as elements.

Dictionary:
This is Vim’s version of a `hash`, often also referred to as an `associative array`. It is an unordered collection of value pairs, the first being a `key` that can be used to retrieve an associated `value`.

## Expressions

Vim evaluates expressions in a fairly straightforward way. An expression can be as simple as a number or literal string, or it can be as complex as a compound statement, itself composed of expressions.

It is important to note that Vim’s math functions work with integers only. If you want floating-point and precision, you need to use extensions, such as system calls to external math-capable routines.

## Extensions

Vim offers a number of extensions and interfaces to other scripting languages. Notably, these include Perl, Python, and Ruby, three of the most popular scripting languages. See Vim’s built-in documentation for details on usage.

## A Few More Comments About autocmd

In the section [“Dynamic File Type Configuration Through Scripting”](#ch12.html#vi8-ch-12-sect-2), we used Vim’s `autocmd` command to key on events from which our user-defined functions are called. This is very powerful, but do not discount simpler uses of `autocmd`. For example, you can use `autocmd` to tune specific Vim options for different file types.

A good example might be to change the `shiftwidth` option for different file types. File types with copious indentation and nesting levels may benefit from more modest indentation. You may want to define your `shiftwidth` as 2 for HTML to prevent code from “walking” off the right side of the screen, but use a `shiftwidth` of 4 for C programs. To accomplish this distinction, include these lines in your `*.vimrc` or `*.gvimrc` file:

```
autocmd BufRead,BufNewFile *.html set shiftwidth=2
autocmd BufRead,BufNewFile *.c,*.h set shiftwidth=4
```

## Internal Functions

In addition to all the Vim commands, you have access to about 200 built-in functions. It is beyond our scope to identify and document all of these functions, but knowing what categories or types of functions are available is useful. The following categories are taken from the Vim built-in help file, `usr_41.txt`:

String manipulation:
All of the standard string functions that programmers expect are included in these functions, from conversion routines to substring routines and more.

List functions:
This is an entire array of array functions. They mirror closely the array functions found in Perl.

Dictionary (associative array) functions:
These functions include extraction, manipulation, verification, and other types of functions. Again, these closely resemble Perl’s hash functions.

Variable functions:
These functions are “getters” and “setters” to move variables around in Vim windows and buffers. There is also a `type()` function to determine variable types.

Cursor and position functions:
These functions allow moving around in files and buffers, and creating marks so that positions can be remembered and returned to. There are also functions that give positional information (e.g., cursor line and column).

Text in current buffer functions:
These functions manipulate text within buffers—for example, changing a line, retrieving a line, and so on. There are also search functions.

System and file manipulation functions:
These include functions to navigate the operating system on which Vim is running, such as finding files within paths, determining the current working directory, and creating and deleting files. This group includes the `system()` function, which passes commands to the operating system for external execution.

Date and time functions:
These do a wide variety of manipulations of date and time formats.

Buffer, window, and argument list functions:
These functions provide mechanisms to gather information about buffers, and the arguments for each one. For example, when Vim starts, the list of files comprises its argument list, and the function `argc()` returns the count of that list. Argument list functions are specific to arguments passed from the Vim command line. The buffer functions provide information specific to buffers and windows. There are 25 functions. For more detailed information, search for `Buffers, windows, and the argument list` in the Vim’s `usr_41.txt` help file. Or you can quickly get to the types of Vim functions with the `ex` command:

```
:help function-list
```

Command-line functions:
These functions get command-line position, the command line, and the command-line type and set the cursor position within the command line.

QuickFix and location lists functions:
These functions retrieve and modify the QuickFix lists.

Insert mode completion functions:
These functions are used for command and insertion completion features.

Folding functions:
These functions give information for folds and expand text displayed for closed folds.

Syntax and highlighting functions:
These functions retrieve information about syntax highlighting groups and syntax IDs.

Spelling functions:
These functions find suspected misspelled words and offer suggested correct spellings.

History functions:
These functions get, add, and delete history items.

Interactive functions:
These functions provide an interface to the user for activities such as file selection.

GUI functions:
There are three simple functions here to get the name of the current font, get the GUI window `x` coordinate, and get the GUI window `y` coordinate.

Vim server functions:
These functions communicate with a (possibly) remote Vim server.

Window size and position functions:
These functions get window information and allow for saving and restoring window “views.”

Various functions:
These are the miscellaneous “other” functions that don’t fit nicely in the previous categories. They include functions such as `exists()`, which checks for the existence of a Vim item, and `has()`, which checks to see whether Vim supports a certain feature.

# Resources
