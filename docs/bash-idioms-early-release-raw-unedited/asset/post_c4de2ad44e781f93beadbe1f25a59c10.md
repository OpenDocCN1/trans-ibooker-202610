
- Prefer using a “here” document (with leading TABs) to a bunch of echo lines
- Use `source` (instead of `.`) to include config files, which should end in `.cfg` (or `.conf` or whatever your standard is)
- If at all possible use ISO-8601 dates for everything
- If at all possible keep lists of things (e.g., ip addresses, hostnames, packages to install, whatever) in alphabetical order; this prevents duplication and makes it easier to add or remove items
  - This applies to `case` statements, contents of variables or arrays/lists, and wherever it makes sense
- If possible, use long arguments to commands for readability, e.g., use `diff --quiet` instead of `diff -q`, though watch out for portability to non GNU/Linux systems
  - If any options are short or obscure, add comments.
  - Also strongly consider documenting why you chose or needed the options you chose, and even options you considered but didn’t use for some reason.
  - Definitely document any options that might seem like a good idea but that actually can cause problems, especially if they are options you commonly use elsewhere.

For variable naming, watch out for generic variable names like:

```
`${GREP} ${pattern} ${file}`
```

That’s very abstracted out, and maybe reusable, but it’s also pretty context free. We’ve replaced the older `` with newer and more

Add *useful* comments that explain *why* you did something and your intent! Like:

```
continue # to the next file in for loop
```

In theory it should not be necessary to explain *what* something does if you write clear code. But sometimes bash itself is just convoluted or obscure, e.g., variable substitution like:

```
PROGRAM=${0##*/}   # Basename in bash, see "Prefix Removal" in Chapter 4
```

Separators are often useful for delimited logical blocks of code. But don’t add a closing separator at the bottom to “box” it out, that just adds clutter and reduces your on-screen lines-of-code by 1.

Whatever you do, do *not* build boxes with characters on the right-hand-side! It’s totally unnecessary and you’ll waste a lot of time getting it to “look right” up front. What’s worse is that it’s a strong disincentive to fixing or updating comments later, because then you have to go fix the box again.

Don’t do this:

```
################################################################################################
######
# Please don't build boxes like this!
#
#
#
# They make it VERY painful to edit the comments later,
because #
# now you have to worry about the closing character on the
#
# right-hand-side.  This example isn't too bad, but it gets
#
# out of hand quickly.
#
################################################################################################
######
```

Do this:

```
######################################
######
# Please DO build boxes like this!
#
# This one is easier to edit, because while you still have to
# worry about text wrapping, it's just a simple leading "#" when
# do do re-wrap.
```

## Names

Naming is **important**. We can’t stress that enough. The difference between `$file` and `$critical_process_daily_log_file` doesn’t seem like anything except some extra typing *now*, when all the details are in your head. But we guarantee if you take the extra time to really think about what you are naming and how it will read in the code, that will pay off soon, by reducing coding errors, and in the future when re-reading, debugging, and enhancing. Following are style guidelines related to names:

- Good names are critical!
- Global variables and constants are in UPPER case
  - Prefer not to make changes to global variables, but sometimes that’s just much simpler
  - Use `readonly` or `declare -r` for constants
- Other variables are in lower case
- Functions are in Mixed_Case
- Use "_" in place of space or CamelCase (remember "-" is not allowed in variable names)

- Functions must be defined before they are used
  - Group them at the top and use two blank lines and a function separator between each function.
  - do NOT intersperse code between functions!
- Use Camel_Case and "_" to make function names stand out from variable names.
- Use `function My_Func_Name {` instead of `My_Func_Name() {` because it’s more clear and easier to `grep -P '^\s*function '`.
- Each function should have comments defining: what it does, inputs (including GLOBALS), and outputs
- When you have useful, standalone pieces of code, make them a function.
- Use a function any time you use the same (or substantially similar) block of code more than once. If they are very common, like logging or emailing, consider creating a "library." that is, a single common file you can source as needed.
  - Prefix "library" functions with "_", like `_Log`, or some other prefix.
- Consider using "filler" words for readability in arguments if it makes sense, then define them as `local junk1="$2" # unused filler`, e.g.:
  - `_Create_File_If_Needed "/path/to/$file" containing important value`
- Do use the `local` builtin when setting variables in functions.

- But be aware that it will mask a return code, so declare and assign it on separate lines if using command substitution, like `local my_input` and then `my_input="$(some-command)"`.
- For any function longer than about 25 lines, close it with `}  # end of function MyFuncName` to make it easier to track where you are in the code on your screen. For functions shorter than 25 lines this is optional but encouraged unless it gets too cluttered.
- Don’t use a `main` function, it’s almost always just an unnecessary layer.
  - That said, using “main” makes sense to Python and C programmers, or if the code is also used as a library, and it may be required if you do a lot of unit testing.
- Consider using 2 blank lines and a main separator above the main section, especially when you have a lot of functions and definitions at the top.

Also, define a single logging function in your library (e.g., `_Log`), and use it! Otherwise you'll end up with a wild mix of logging functions, styles, and destinations. Ideally, as we said above, log to syslog and let the OS worry about final destination(s), log rotation, etc.

## Quoting

Quoting is pretty simple, until it’s not and then you get a headache. We know how this works, and we still end up doing trial-and-error sometimes, especially when trying to create a one-liner to run as another user via `sudo` or on another node via `ssh`. Add some `echo` lines, and be careful. Following are style guidelines related to quoting:

- Do put quotes around variables and strings because it makes them stand out a little and clarifies your intent
  - Unless it gets too cluttered
  - Or they need to be unquoted for expansion
- Don’t quote integers
- Use single quotes unless interpolation is required
- Don’t use `${var}` unless needed, it’s too cluttered
  - But that IS needed sometimes, like
    `${variable}_suffix` or
    `${being_lower_cased,,}`
- Do quote command substitutions, like: `var="$(command)"`
- ALWAYS quote both sides of any test statement like [[
  "$foo" == bar ]]
  - FIXME-TOOLS: AsciiDoc missing single quotes again!
  - Unless you are using ~= in which case you can’t quote the regular expression!
- Consider single quoting variables inside echo statements, like echo "cd to $DIR failed." because it’s more clear when a variable is unexpectedly undefined or empty
  - FIXME-TOOLS: AsciiDoc missing single quotes again!
  - Or echo "cd to [$DIR] failed." as you like
- Prefer single quotes around printf formats (see Chapter 6)

and an IDE. First, depending on the team and person, that may not be true, and second even if it is, when it *really* breaks and you end up debugging under fire in `vi` on an 80x24 console, you will not appreciate 200+ column code.

Break your lines.

Break them just *before* the important part so when you scan down the left side of the column the continuations jump out. We also like to use half the usual indent for broken lines. We can’t do a good (bad?) long line here in this book because it will just break in odd places anyway. But we can provide a simple, and we think readable, example:

```
... Lots of code, indented a ways...
    /long/path/to/some/interesting/command \
      | grep "$re_stuff_we_care_about \
      | grep -v "$re_stuff_to_ignore \
      | /path/to/email_or_log_command ...
...more code
```

## Syntax

Following are style guidelines related to syntax:

- Use `#!/bin/bash` or `#!/usr/bin/env bash` when writing bash code, not `#!/bin/sh`
  - FIXME-TOOLS: there’s a Markdown/Asciidoctor rendering issue with the three # ! above
- Use `$@` unless you are *really* sure you need `$*`
- Use `==` instead of `=` for equality, to reduce confusion with assignment
- Use `$()` instead of backticks/backquotes
