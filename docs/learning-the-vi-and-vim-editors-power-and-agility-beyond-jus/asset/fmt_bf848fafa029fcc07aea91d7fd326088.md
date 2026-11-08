
### Print the next line in the file.
Print the next line in the file. (For `ex` only, not from the `:` prompt in `vi`.)

^[1](#app01.html#vimbehavior-marker) If you use Vim and set `*.viminfo` to restore the file’s last cursor location, the search starts in the direction mentioned from the restored cursor location. Also, depending on the setting of the `wrapscan` (`ws`) option in the `*.vimrc` configuration file, the search will stop (`:set nowrapscan`) or continue (`:set wrapscan`) past the top or bottom line in the file.

^[2](#app01.html#idm46601069972624-marker) This option is obsolete; the `crypt` command’s encryption is weak and you shouldn’t use it.

^[3](#app01.html#idm46601069862752-marker) If you are using the original `vi` or have set the Vim `compatible` option, the ESC key will execute the command. This can be unexpected. Vim (in `nocompatible` mode) simply cancels the command and takes no action.

# Appendix B. Setting Options
This appendix describes the important `set` command options for the “Heirloom” `vi`, Solaris `/usr/xpg7/bin/vi`, and Vim 8.2.

## Heirloom and Solaris vi Options
[Table B-1](#app02.html#vi8-app-b-tab-1) contains brief descriptions of the important `set` command options. In the first column, options are listed in alphabetical order; if the option can be abbreviated, that abbreviation is shown in parentheses. The second column shows the default setting that `vi` uses unless you issue an explicit `set` command (either manually or in the `*.exrc` file). The last column describes what the option does when enabled.

Table B-1. “Heirloom” and Solaris `vi set` options

| Option  || Default || Description |
| --- | --- | --- |
| `autoindent (ai)` || `noai` || In insert mode, indent each line to the same level as the line above or below. Use with the `shiftwidth` option. |
| --- | --- | --- |
| `autoprint (ap)` || `ap` || Change the display after each editor command. For global replacement, display the last replacement. |
| `autowrite (aw)` || `noaw` || Automatically write (save) the file if changed before opening another file with `:n` or before giving a Unix command with `:!`. |
| `beautify (bf)` || `nobf` || Ignore all control characters during input (except tab, newline, or form feed). |
| `directory (dir)` || `/var/tmp` || Names the directory in which `ex`/`vi` stores buffer files. The directory must be writable. |
| `edcompatible` || `noedcompatible` || Remember the flags used with the most recent substitute command (global, confirming), and use them for the next substitute command. Despite the name, no version of `ed` actually does this. |
| `errorbells (eb)` || `noerrorbells` || Sound the bell when an error occurs. |
| `exrc (ex)` || `noexrc` || Allow the execution of `*.exrc` files that reside outside the user’s home directory. |
| `flash (fp)` || `fp` || Flash the screen instead of ringing the bell. |
| `hardtabs (ht)` || `8` || Define boundaries for terminal hardware tabs. |
| `ignorecase (ic)` || `noic` || Disregard case during a search. |
| `lisp` || `nolisp` || Insert indents in appropriate Lisp format. `( )`, `{ }`, `[[`, and `]]` are modified to have meaning for Lisp. |
| `list` || `nolist` || Print tabs as `^I`; mark ends of lines with `$`. |
| `magic` || `magic` || Wildcard characters `.` (dot), `*` (asterisk), and `[]` (brackets) have special meaning in patterns. |
| `mesg` || `mesg` || Permit system messages to display on the terminal while editing in `vi`. |
| `novice` || `nonovice` || Require the use of long `ex` command names, such as `copy` or `read`. Solaris `vi` only. |
| `number (nu)` || `nonu` || Display line numbers on the left side of the screen during an editing session. |
| `open` || `open` || Allow entry to *open* or *visual* mode from `ex`. Although not in Solaris `vi`, this option has traditionally been in `vi`, and it may be in your Unix’s version of `vi`. |
| `optimize (opt)` || `noopt` || Abolish carriage returns at the end of lines when printing multiple lines; this speeds output on dumb terminals when printing lines with leading whitespace (spaces or tabs). |
| `paragraphs (para)` || `IPLPPPQP` `LIpplpipbp` || Define paragraph delimiters for movement by `{` or `}`. The pairs of characters in the value are the names of `troff` macros that begin paragraphs. |
| `prompt` || `prompt` || Display the `ex` prompt (:) when `vi`’s `Q` command is given. |
| `readonly (ro)` || `noro` || Any writes (saves) of a file fail unless you use `!` after the write (works with `w`, `ZZ`, or `autowrite`). |
| `redraw (re)` ||  || Redraw the screen whenever edits are made (in other words, insert mode pushes over existing characters, and deleted lines immediately close up). The default depends on line speed and terminal type. `noredraw` is useful at slow speeds on a dumb terminal: deleted lines show up as `@`, and inserted text appears to overwrite existing text until you press ESC. This option is essentially obsolete; let `vi` choose how to set it. |
| `remap` || `remap` || Allow nested map sequences. |
| `report` || `5` || Display a message on the status line whenever you make an edit that affects at least a certain number of lines. For example, `6dd` reports the message “6 lines deleted.” |
| `scroll` || [*½ window*] || Number of lines to scroll with `^D` and `^U` commands. |
| `sections (sect)` || `SHNHH HU` || Define section delimiters for `[[` and `]]` movement. The pairs of characters in the value are the names of `troff` macros that begin sections. |
| `shell (sh)` || `/bin/sh` || Pathname of the shell used for shell escapes (`:!`) and the shell command (`:sh`). The default value is derived from the shell environment, which varies on different systems but often ends up being `/bin/sh`. |
| `shiftwidth (sw)` || `8` || Define the number of spaces in backward (`^D`) tabs when using the `autoindent` option, and for the `<<` and `>>` commands. |
| `showmatch (sm)` || `nosm` || In `vi`, when `)` or `}` is entered, move the cursor briefly to the matching `(` or `{`. (If no match, ring the error message bell.) Very useful for programming. |
| `showmode` || `noshowmode` || In insert mode, display a message on the prompt line indicating the type of insert you are making, for example, “OPEN MODE” or “APPEND MODE.” |
| `slowopen (slow)` ||  || Hold off display during insert. The default depends on line speed and terminal type. |
| `sourceany` || `nosourceany` || Allow reading `*.exrc` files that are not owned by the current user. “Heirloom” `vi` only. |
| `tabstop (ts)` || `8` || Define the number of spaces that a tab indents during an editing session. (Printers still use the system tab of 8.) |
| `taglength (tl)` || `0` || Define the number of characters that are significant for tags. The default (zero) means that all characters are significant. |
| `tags` || `tags` `/usr/lib/tags` || Define the pathname of files containing tags. See the Unix `ctags` command. By default, `vi` searches the file `tags` in the current directory and `/usr/lib/tags`. |
| `tagstack` || `tagstack` || Enable stacking of tag locations on a stack. Solaris `vi` only. |
| `term` ||  || Set the terminal type. |
| `terse` || `noterse` || Display shorter error messages. Ironically, there is no abbreviation for this option. |
| `timeout (to)` || `timeout` || Keyboard maps time out after 1 second.^[a](#app02.html#idm46601068027280) |
| `ttytype` ||  || Set the terminal type. This is just another name for `term`. |
| `warn` || `warn` || Display the warning message “No write since last change.” |
| `window (w)` ||  || Show a certain number of lines of the file on the screen. The default depends on line speed and terminal type. |
| `wrapmargin (wm)` || `0` || Define the right margin. If greater than zero, automatically insert carriage returns to break lines. |
| `wrapscan (ws)` || `ws` || Wrap searches around either end of the file. |
| `writeany (wa)` || `nowa` || Allow saving to any file. |
| ^[a](#app02.html#idm46601068027280-marker) When you have mappings of several keys (for example, `:map zzz 3dw`), you probably want to use `notimeout`. Otherwise, you need to type `zzz` within one second. When you have an insert mode mapping for a cursor key (for example, `:map! ^[OB ^[ja`), you should use `timeout`. Otherwise, `vi` won’t react to ESC until you type another key. |
| --- |

## Vim 8.2 Options
In the preceding section, we listed all 46 “Heirloom” and Solaris `set` command options. Vim 8.2 has more than 400 (!) `set` command options. [Table B-2](#app02.html#vi8-app-b-tab-2) lists the options we consider to be most useful.

Most options described in [Table B-1](#app02.html#vi8-app-b-tab-1) are not repeated here.

The summaries in this table are by necessity very brief. Much more information about each option may be found in Vim’s online help file `options.txt`.

Table B-2. Vim 8.2 `set` options

| Option  || Default || Description |
| --- | --- | --- |
| `autoread (ar)` || `noautoread` || Detect whether a file inside Vim has been modified externally, not by Vim, and automatically refresh the Vim buffer with the changed version of the file. |
| --- | --- | --- |
| `background (bg)` || `dark` or `light` || Vim tries to use background and foreground colors that are appropriate to the particular terminal. The default depends on the current terminal or windowing system. |
| `backspace (bs)` || `0` || Control whether you can backspace over a newline and/or over the start of insert. Values are 0 for `vi` compatibility; 1 to backspace over newlines and indents; and 2 to backspace over the start of newlines, insert, and indents. |
| `backup (bk)` || `nobackup` || Make a backup before overwriting a file and then leave it around after the file has been successfully written. To have a backup file just while the file is being written, use the `writebackup` option. See also `writebackup`. |
| `backupdir (bdir)` || `., ~/tmp/, ~/` || A list of directories for the backup file, separated with commas. The backup file is created in the first directory in the list where this is possible. If empty, you cannot create a backup file. The name `.` (dot) means the same directory as where the edited file is. |
| `backupext (bex)` || `~` || The string that is appended to a filename to make the name of the backup file. |
| `binary (bin)` || `nobinary` || Change a number of other options to make it easier to edit binary files. The previous values of these options are remembered and restored when `bin` is switched back off. Each buffer has its own set of saved option values. This option should be set before editing a binary file. You can also use the `-b` command-line option. |
| `breakat (brk)` || `" ^I!@*-+;:,./?"` || Break line at any character in `breakat` string if the `set` option `linebreak` is on. See also options `breakindent`, `linebreak`, and `showbreak` for customizing this feature. |
| `breakindent (bri)` || `nobreakindent` || Indent lines wrapped by the `breakat` option. |
| `cdpath (cd)` || Same as value in environment variable `CDPATH` || A list of directories Vim will search with `ex` command `cd` or `lcd` in the same way `$CDPATH` works in the shell. If you use it in your shell, this will be familiar behavior. |
| `cindent (cin)` || `nocindent` || Enable automatic smart C program indenting. |
| `cinkeys (cink)` || `0{,0},:,0#,!^F, o,O,e` || A list of keys that, when typed in insert mode, cause reindenting of the current line. Only happens if `cindent` is on. |
| `cinoptions (cino)` ||  || Affects the way `cindent` reindents lines in a C program. See the online help for details. |
| `cinwords (cinw)` || `if`, `else`, `while`, `do`, `for`, `switch` || These keywords start an extra indent in the next line when `smartindent` or `cindent` is set. For `cindent`, this is done only at an appropriate place (inside `{…}`). |
| `cmdwinheight (cwh)` || number (default 7) || Number of lines in the command-line window. |
| `colorcolumn (cc)` || empty string || Highlight columns listed in comma-separated list. This is a useful visual for vertical text alignment. |
| `columns (co)` || `80` or terminal width || Usually set by Vim. Can be useful to define for your GUI instance at launch if you have a preference (as one of us does). See also `lines`. |
| `comments (com)` || `s1:/*,mb:*,ex:*/,://, b:#,:%,:XCOMM,n:>,fb:-` || A comma-separated list of strings that can start a comment line. See the online help for details. |
| `compatible (cp)` || `cp`; `nocp` when a `*.vimrc` or Vim runtime `defaults.vim` file is found || Makes Vim behave more like `vi` in too many ways to describe here. It is on by default, to avoid surprises. Having a `*.vimrc` turns off the `vi` compatibility; usually this is a desirable side effect.^[a](#app02.html#idm46601067922048) |
| `completeopt (cot)` || `menu,preview` || A comma-separated list of options for insert mode completion. |
| `cpoptions (cpo)` || `aABceFs` || A sequence of single character flags, each one indicating a different way in which Vim will or will not exactly mimic `vi`. When empty, the Vim defaults are used. See the online help for details. |
| `cursorcolumn (cuc)` || `nocursorcolumn` || Highlight the screen column of the cursor with `CursorColumn` highlighting. This is useful for lining up text vertically. Can slow down the screen display. |
| `cursorline (cul)` || `nocursorline` || Highlight the screen line of the cursor with `CursorRow` highlighting. Makes it easy to find the current line in the edit session. Use in conjunction with `cursorcolumn` for a crosshairs effect. Can slow down the screen display. |
| `cursorlineopt (culopt)` || `string, ""` || Define the behavior of `cursorline` (`cursorline` must be set for this to have any effect). The most useful effect is to set it to `number`. This highlights line numbers only. While it can be useful to highlight the entire line, doing so becomes confusing when used together with syntax coloring, as the highlight alters the color and background of lines. |
| `define (def)` || `^#\s*define` || A search pattern that describes macro definitions. The default value is for C programs. For C++, use `^\(#\s*define\ |[a-z]*\s*const\s*[a-z]*\)`. When using the `:set` command, you need to double the backslashes. |
| `dictionary (dict)` || empty string || Comma-separated list of filenames to use for `keyword` completion. |
| `digraph (dg)` || `nodigraph` || Useful for entering digraphs with *character1*, BACKSPACE, *character2*. See the discussion in [“Digraphs: Non-ASCII Characters”](#ch13.html#vi8-ch-13-sect-3). |
| `directory (dir)` || `., ~/tmp, /tmp` || A list of directory names for the swap file, separated with commas. The swap file will be created in the first directory where this is possible. If empty, no swap file will be used and recovery is impossible! The name `.` (dot) means to put the swap file in the same directory as the edited file. Using `.` first in the list is recommended so that editing the same file twice will result in a warning. |
| `equalprg (ep)` ||  || External program to use for `=` command. When this option is empty, the internal formatting functions are used. |
| `errorfile (ef)` || `errors.err` || Name of the error file for the quickfix mode. When the `-q` command-line argument is used, `errorfile` is set to the following argument. |
| `errorformat (efm)` || (Too long to print) || `scanf`-like description of the format for the lines in the error file. |
| `expandtab (et)` || `noexpandtab` || When inserting a tab, expand it to the appropriate number of spaces. |
| `fileformat (ff)` || `unix` || Describes the convention to terminate lines when reading/writing the current buffer. Possible values are `dos` (CR/LF), `unix` (LF), and `mac` (CR). Vim usually sets this automatically. |
| `fileformats (ffs)` || `dos,unix` || List the line-terminating conventions that Vim tries to apply to a file when reading. Multiple names enable automatic end-of-line detection when reading a file. |
| `fixendofline (fixeol)` || `boolean, on` || This ensures that a proper newline is appended to the last line of a file upon writing it out. If you don’t want this, be sure to turn it off. For example, you don’t want this if you are editing a binary file. |
| `formatoptions (fo)` || Vim default: `tcq`; `vi` default: `vt` || A sequence of letters that describes how automatic formatting is to be done. See the online help for details. |
| `gdefault (gd)` || `nogdefault` || Cause the substitute command to change all instances. |
| `guifont (gfn)` ||  || A comma-separated list of fonts to try when starting the GUI version of Vim. |
| `hidden (hid)` || `nohidden` || Hide the current buffer when it is unloaded from a window, instead of abandoning it. |
| `history (hi)` || Vim default: 20; `vi` default: 0 || Control how many `ex` commands, search strings, and expressions are remembered in the command history. Set this to a high number. Computer memory is cheap! See the section [“Moving into the Fast Lane”](#ch14.html#vi8-ch-14-sect-2) for an example of leveraging command-line history. |
| `hlsearch (hls)` || `nohlsearch` || Highlight all matches of the most recent search pattern. |
| `icon` || `noicon` || Vim attempts to change the name of the icon associated with the window where it is running. Overridden by the `iconstring` option. |
| `iconstring` ||  || String value used for the icon name of the window. |
| `ignorecase (ic)` || `noignorecase` || Ignore case in searches. See also `smartcase`. |
| `include (inc)` || `^#\s*include` || Define a search pattern for finding `include` commands. The default value is for C programs. |
| `incsearch (is)` || `noincsearch` || Enable incremental searching. |
| `isfname (isf)` || `@,48-57,/,.,-,_, +,,,$,:,~` || A list of characters that can be included in filenames and pathnames. Non-Unix systems have different default values. The @ character stands for any alphabetic character. It is also used in the other `is` *`XXX`* options, described next. |
| `isident (isi)` || `@,48-57,_,192-255` || A list of characters that can be included in identifiers. Non-Unix systems may have different default values. |
| `iskeyword (isk)` || `@,48-57,_,192-255` || A list of characters that can be included in keywords. Non-Unix systems may have different default values. Keywords are used in searching and recognizing with many commands, such as `w`, `[i`, and many more. |
| `isprint (isp)` || `@,161-255` || A list of characters that can be displayed. |
| `laststatus (ls)` || 2 || Controls when the last window will have a status line. Zero is never, one is only if there are at least two windows, and two is always. |
| `linebreak (lbr)` || `nolinebreak` || Break a long line at the characters defined in `breakat`. Vim wraps the line to keep the whole line visible. |
| `lines` || `24` or terminal height || Usually set by Vim. Can be useful if you use the GUI and prefer to define the number of lines when starting Vim. See also `columns`. |
| `listchars (lcs)` || eol:$ || Customize what Vim displays when the `list` option is set. Useful for defining spaces as dots. (Even more refined is defining leading and trailing spaces with the `lead:.` and `trail:.` definitions: `:set listchars+=lead:.,trail:.`.) |
| `makeef (mef)` || `/tmp/vim##.err` || The error filename for the `:make` command. Non-Unix systems have different default values. The `##` is replaced by a number to make the name unique. |
| `makeprg (mp)` || `make` || The program to use for the `:make` command. `%` and `#` in the value are expanded. |
| `matchpairs (mps)` || `(:),{:},[:]` || Comma-separated definitions of colon-separated matching character pairs. These must be two different characters. It is useful to add `<:>` for HTML matching. `:set matchpairs+="<:>"` |
| `modifiable (ma)` || `modifiable` || When turned off, do not allow any changes in the buffer. |
| `mouse` || `a` for GUI, MS-DOS, and Win32 || Enable the mouse in non-GUI versions of Vim. This works for MS-DOS, Win32, QNX `pterm`, and `xterm`. See the online help for details. |
| `mousehide (mh)` || `nomousehide` || Hide the mouse pointer during typing. Restores the pointer when the mouse is moved. |
| `numberwidth (nuw)` || `vi` default: `8`; Vim default: `4` || Define the number column width (set with `number` or `relativenumber`). Vim always uses the last position for a separating `space`. We recommend setting this to at least six. |
| `paste` || `nopaste` || Change a large number of options so that pasting into a Vim window with a mouse does not mangle the pasted text. Turning it off restores those options to their previous values. See the online help for details. |
| `relativenumber (rnu)` || `norelativenumber` || Number lines on the left side of the window relative to the current line. For example, the current line displays the correct line number, and all lines above and below show the offset from the current line. This can be useful for block commands, removing the need to count lines. |
| `ruler (ru)` || `noruler` || Show the line and column number of the cursor position. |
| `scrollbind (scb)` || `noscrollbind` || Bind the current window to scroll with other windows that also have `scrollbind` set. Useful for `diff` comparisons. |
| `scrolloff (s0)` || `0` (`5` in `defaults.vim`) || Set the minimum number of lines above or below the cursor when scrolling. Useful for forcing context lines around the current position. We like setting `scrolloff` to three. |
| `scrollopt (sbo)` || `ver,jump` || Define `scrollbind` behavior. `ver` binds vertical scrolling between `scrollbind` windows. See the Vim help for detailed discussion. |
| `secure` || `nosecure` || Disable certain kinds of commands in the startup file. Automatically enabled if you don’t own the `*.vimrc` and `*.exrc` files. |
| `shellpipe (sp)` ||  || The shell string to use for capturing the output from `:make` into a file. The default value depends on the shell. |
| `shellredir (srr)` ||  || The shell string for capturing the output of a filter into a temporary file. The default value depends on the shell. |
| `showbreak (sbr)` || empty string || Insert this string in front of wrapped lines. |
| `showcmd (sc)` || Vim `showcmd`, Unix `noshowcmd`, defined also in `defaults.vim` || Show `vi` command mode commands as they are entered. Vim displays the command on the righthand side of the `ex` command mode line. For example, the `vi` command `5cw` to change five words is progressively displayed as it is input. Useful for keeping track of a command as you build it. |
| `showmode (smd)` || Vim default: `smd`; `vi` default: `nosmd` || Put a message in the status line for insert, replace, and visual modes. |
| `sidescroll (ss)` || `0` || How many columns to scroll horizontally. The value zero puts the cursor in the middle of the screen. |
| `smartcase (scs)` || `nosmartcase` || Override the `ignorecase` option if the search pattern contains uppercase characters. |
| `spell` || `nospell` || Turn on spellchecking. |
| `spelllang (spl)` || `en` || A comma-separated list of spellchecking language files. |
| `suffixes` || `*.bak,~,.o,.h,.info,.swp` || When multiple files match a pattern during filename completion, the value of this variable sets a priority among them in order to pick the one Vim will use. |
| `taglength (tl)` || `0` || Define the number of characters that are significant for tags. Default (zero) means that all characters are significant. |
| `tagrelative (tr)` || Vim default: `tr`; `vi` default: `notr` || Filenames in a `tags` file from another directory are taken to be relative to the directory where the `tags` file is. |
| `tags (tag)` || `./tags,tags` || Filenames for the `:tag` command, separated by spaces or commas. The leading `./` is replaced with the full path to the current file. |
| `tildeop (top)` || `notildeop` || Make the `~` command behave like an operator. |
| `undolevels (ul)` || `1000` || The maximum number of changes that can be undone. A value of 0 means `vi` compatibility: one level of undo and `u` undoes itself. Non-Unix systems may have different default values. |
| `viminfo (vi)` ||  || Read the `viminfo` file upon startup, and write it upon exiting. The value is complex; it controls the different kinds of information that Vim will store in the file. See the online help for details. |
| `writebackup (wb)` || `writebackup` || Make a backup before overwriting a file. The backup is removed after the file is successfully written, unless the `backup` option is also on. |
| ^[a](#app02.html#idm46601067922048-marker) Since Vim 8.0, Vim sets `compatible` off if a Vim runtime file `defaults.vim` or system-wide `defaults.vim` exists. This is a much better default behavior and addresses long-standing complaints and confusion when neophytes try Vim but see no Vim-specific behaviors. |
| --- |

# Appendix C. The Lighter Side of vi
> *Sure, `vi` is user friendly. It’s just particular about who it makes friends with.*
> 
> Anonymous

This appendix touches on a broad range of `vi`-related subjects. It covers:
* Accessing the files described here and earlier in the book.
* The online `vi` tutorial referenced in [Part I](#part01.html#vi8-part-1).
* The *vi Powered* logo for your website (and other logos).
* `vi`-related swag.
* The Vim clutch.
* Some of the unusual and amazing things people have done with `vi` over the years.
* The [*Vi* Lovers Home Page](https://thomer.com/vi/vi.html).
* A *different* `vi` clone.
* A brief mention of `vi` versus Emacs.
* Some nice `vi`-related quotations.

## Accessing the Files
Many of the bits and pieces we present here were once freely available on the internet. Alas, that is no longer true. To remedy this, we have created a GitHub repository with the various files. Simply clone `https://www.github.com/learning-vi/vi-files` to make your own copy of the repository:

```
```
