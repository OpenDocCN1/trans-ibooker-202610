
##################################################
# Please don't build boxes like this!            #
#                                                #
# They make it VERY painful to edit the comments later, because #
# now you have to worry about the closing character on the      #
# righthand side. This example isn't too bad, but it gets       #
# out of hand quickly.                           #
##################################################

Do this:

##################################################
# Please DO build boxes like this!
#
# This one is easier to edit, because while you still have to
# worry about text wrapping, it's just a simple leading "#" when
# you do rewrap.

## Names

Naming is *important*. We can’t stress that enough. The difference between `$file` and `$critical_process_daily_log_file` doesn’t seem like anything except some extra typing *now*, when all the details are in your head. But we guarantee if you take the extra time to really think about what you are naming and how it will read in the code, that will pay off soon, by reducing coding errors, and in the future when rereading, debugging, and enhancing. Following are style guidelines related to names:

- Good names are critical!
- Global variables and constants are in UPPER case.
  - Prefer not to make changes to global variables, but sometimes that’s just much simpler (KISS).
  - Use `readonly` or `declare -r` for constants.
- Other variables are in lowercase.
- Functions are in Mixed_Case.
- Use "_", not CamelCase, in place of space (remember, "-" is not allowed in variable names).
- Use bash arrays carefully; they can be hard to read (see Chapter 7). `for var in $regular_var` often works as well.
- Replace $1, $2, .. $N with readable names ASAP.
  - That makes everything much more debuggable and readable, but it also makes it easy to have defaults and add or rearrange arguments.
- Distinguish between types of referents, like $input_file versus $input_dir.
- Use consistent “FIXME” and “TODO” labels, with names and ticket numbers if appropriate.

Consider how easy it would be to confuse these and use (or make an off-by-one typo) the wrong one:

```
file1='/path/to/input'
file2='/path/to/output'
```

This is much more intuitive to read and harder to type wrong:

```
input_file='/path/to/input'
output_file='/path/to/output'
```

Also, don’t leave out two characters to save typing; just spell out $filename so you don’t get it wrong later. Was it $filenm or $flname or $flnm or what?

## Functions

Following are style guidelines related to functions:

- *Always use a header.*
- Good names are critical!
- Functions must be defined before they are used.
  - Group them at the top, and use two blank lines and a function separator between each function.
  - Do *not* intersperse code between functions!
- Use Camel_Case and “_” to make function names stand out from variable names.
- Use function My_Func_Name { instead of My_Func_Name() { because it’s clearer and easier to grep -P '^\s*function '.
- Each function should have comments defining what it does, inputs (including GLOBALS), and outputs.
- When you have useful, standalone pieces of code, or any time you use the same (or substantially similar) block of code more than once, make them into functions. If they are very common, like logging or emailing, consider creating a “library,” that is, a single common file you can source as needed.
  - Prefix “library” functions with “_”, like _Log or some other prefix.
- Consider using “filler” words for readability in arguments if it makes sense, then define them as local junk1="$2" # Unused filler, e.g.:
  - _Create_File_If_Needed "/path/to/$file" containing *important value*
- Do use the local builtin when setting variables in functions.
  - But be aware that successfully being “local,” it will mask a failed return code, so declare and assign it on separate lines if using command substitution, like local my_input and then my_input="$(some-command)".
- For any function longer than about 25 lines, close it with } # End of function MyFuncName to make it easier to track where you are in the code on your screen. For functions shorter than 25 lines, this is optional but encouraged unless it gets too cluttered.
- Don’t use a main function; it’s almost always just an unnecessary layer.
  - That said, using “main” makes sense to Python and C programmers, or if the code is also used as a library, and it may be required if you do a lot of unit testing.
- Consider using two blank lines and a main separator above the main section, especially when you have a lot of functions and definitions at the top.

Also, define a single logging function in your library (e.g., _Log), and use it! Otherwise you’ll end up with a wild mix of logging functions, styles, and destinations. Ideally, as we said previously, log to syslog and let the OS worry about final destination(s), log rotation, etc.

## Quoting

Quoting is pretty simple, until it’s not, and then you get a headache. We know how this works, and we still end up doing trial-and-error sometimes, especially when trying to create a one-liner to run as another user via sudo or on another node via ssh. Add some echo lines, and be careful. Following are style guidelines related to quoting:

- Do put quotes around variables and strings because it makes them stand out a little and clarifies your intent.
  - Unless it gets too cluttered.
  - Or they need to be unquoted for expansion.
- Don’t quote integers.
- Use single quotes unless interpolation is required.
- Don’t use ${var} unless needed; it’s too cluttered.
  - But that is needed sometimes, like ${variable}_suffix or ${being_lower_cased,,}.
- Do quote command substitutions, like var="$(command)".
- *Always* quote both sides of any test statement, like [[ "$foo" == 'bar' ]].
  - Unless one side is an integer.
  - And unless you are using ~=, in which case you can’t quote the regular expression!
- Consider single-quoting variables inside echo statements, like echo "cd to '$DIR' failed." because it’s visible when a variable is unexpectedly undefined or empty.
  - Or echo "cd to [$DIR] failed." as you like.
  - If using set -u, you will get an error if the variable is not defined—but not if it is defined but is just unexpectedly empty.
- Prefer single quotes around printf formats (see “POSIX Output” on page 56 and the rest of “Time for printf” on page 55).

## Layout

Following are style guidelines related to layout:

- Line things up! Multiple spaces almost never matter in bash (except around =), and lining up similar commands makes it easier to read and to see both the similarities and differences.
- *Do not allow trailing white space!* This will later cause noise in the VCS (version control system) when removed.
- Indent using four spaces, but use TAB with here-documents as needed.
- Break long lines at around 78 columns, indent line continuations two spaces, and break just before | or > so those parts jump out as you scan down the code.
- The code to open a block goes on one line, like:
  - if expression; then
  - for expression; do
- List elements in case..esac are indented four spaces, and closing ;; are at that same indent level. Blocks for each item are also indented four spaces.
  - One-line elements should be closed with ;; on the same line.
  - Prefer lining up the ) in each element, unless it gets cluttered or out of hand.
  - See the example code in Example 8-4.

There’s an argument about not bothering to break lines at 70–80 columns that assumes everyone is using a wide graphical terminal and an IDE. First, depending on the team and person, that may not be true, and second, even if it is, when it *really*

breaks and you end up debugging under fire in vi on an 80 × 24 console, you will not appreciate 200+ column code.

Break your lines.

Break them just before the important part, so when you scan down the left side of the column, the continuations jump out. We also like to use half the usual indent for broken lines. We can’t do a good (bad?) long line here in this book because it will just break in odd places anyway. But we can provide a simple, and we think readable, example:

```
... Lots of code, indented a ways...
    /long/path/to/some/interesting/command \
    | grep "$re_stuff_we_care_about" \
    | grep -v "$re_stuff_to_ignore" \
    | /path/to/email_or_log_command ...
...more code
```

## Syntax

Following are style guidelines related to syntax:

- Use #!/bin/bash - or #!/usr/bin/env bash when writing bash code, not #!/bin/sh.
- Use $@ unless you are really sure you need $*.
- Use == instead of = for equality, to reduce confusion with assignment.
- Use $( ) instead of `` backticks/backquotes.
- Use [[ instead of [ (unless you need [ for portability, e.g., dash).
- Use (( )) and $(( )) as needed for integer arithmetic; avoid let and expr.
- Use [[ expression ]] && block or [[ expression ]] || block when it is simple and readable to do so. Do not use [[ expression ]] && block || block because that doesn’t do what you think it does; use if .. then .. (elif ..) else .. fi for that.
- Consider using “Unofficial bash Strict Mode” (see “Unofficial bash Strict Mode” on page 96).
  - set -euo pipefail will prevent or unmask many simple errors.
  - Watch out for this one, and use it carefully (if you use it at all): IFS=$'\n\t'.
