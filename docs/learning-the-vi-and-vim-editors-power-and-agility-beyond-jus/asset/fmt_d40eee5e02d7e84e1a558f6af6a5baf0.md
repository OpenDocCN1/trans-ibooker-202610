
## 7. Replace one or more spaces following a colon with exactly two spaces

```
:%s/:□□*/:□□/g
```

## 8. Replace one or more spaces following a period or a colon with exactly two spaces

```
:%s/\([:.]\)□□*/\1□□/g
```

Either of the two characters within brackets can be matched. This character is saved into a hold buffer, using `\(` and `\)`, and restored on the righthand side by the `\1`. Note that within brackets a special character such as a dot does not need to be escaped.

## 9. Standardize various uses of a word or heading

```
:%s/^Note[□:s]*/Notes:□/g
```

The brackets enclose three characters: a space, a colon, and the letter `s`. Therefore, the pattern `Note[□s:]` matches `Note`□, `Notes`, or `Note:`. An asterisk is added to the pattern so that it also matches `Note` with zero spaces after it and `Notes:` the already correct spelling. Without the asterisk, `Note` would be missed entirely and `Notes:` would be incorrectly changed to `Notes:`□*:*. Simultaneously, this truncates multiple spaces down to one, so that `Note:`□□ becomes `Notes:`□.

## 10. Delete all empty lines

```
:g/^$/d
```

What you are actually matching here is the beginning of the line `^` followed by the end of the line `$`, with nothing in between.

## 11. Delete all empty lines, plus any lines that contain only whitespace

```
:g/^[□tab]*$/d
```

In the example, a tab is shown as `tab`. A line may appear to be empty but may in fact contain spaces or tabs. The previous example does not delete such a line. This example, like the previous one, searches for the beginning and end of the line. But instead of having nothing in between, the pattern tries to find any number of spaces or tabs. If no spaces or tabs are matched, the line is empty. To delete lines that contain whitespace but that aren’t empty, you would have to match lines with at least one space or tab:

```
:g/^[□tab][□tab]*$/d
```

## 12. Delete all leading spaces on every line

```
:%s/^□□*\(.*\)/\1/
```

Use `^□□*` to search for one or more spaces at the beginning of each line; then use `\(.*\)` to save the rest of the line into the first hold buffer. Restore the line without leading spaces, using `\1`. This can be done more simply with `s/^□□*//`.

## 13. Delete all spaces at the end of every line

```
:%s/□□*$//
```

For each line, remove one or more spaces at the end of the line.

Because of the `^` and `$` anchors, the substitutions in this example and the previous one happen only once on any given line, so the `g` option doesn’t need to follow the replacement string.

## 14. Insert a `//□` at the start of each line from the current line to the next line that starts with `}`

```
:.,/^}/s;^;//□;
```

What we’re really doing here is “replacing” the start of the line with `//□`. Of course, the start of the line being a logical construct, not an actual character, isn’t really replaced!

What this does is comment-out all lines from the current line dot to the next line that starts with a right closing brace, using C++ `//` comments. Typically, you’d use this by placing the cursor at the first line of a function definition to comment-out an entire function.

Note the use of a semicolon as the delimiter for the substitute command when the substitution text contains one or more slashes.

## 15. Add a period to the end of the next six lines

```
:.,+5s/$/./
```

The line address indicates the current line plus five lines. The `$` indicates the end of line. As in the previous example, the `$` is a logical construct. You aren’t really replacing the end of the line.

## 16. Reverse the order of all hyphen-separated items in a list

```
:%s/\(.*\)□-□\(.*\)/\2□-□\1/
```

Use `\(.*\)` to save text on the line into the first hold buffer, but only until you find □-□. Then use `\(.*\)` to save the rest of the line into the second hold buffer. Restore the saved portions of the line, reversing the order of the two hold buffers. The effect of this command on several items is shown here:

```
more - display files
```

becomes:

```
display files - more
```

and:

```
lp - print files
```

becomes:

```
print files - lp
```

This can be done even more succinctly:

```
:%s/\(.*\)\(□-□\)\(.*\)/\3\2\1/
```

## 17. Change every letter in a file to uppercase

```
:%s/.*/\U&/
```

or:

```
:%s/./\U&/g
```

The `\U` flag at the start of the replacement string tells the editor to change the replacement to uppercase. The `&` character replays the text matched by the search pattern as the replacement.

These two commands are equivalent; however, the first form is considerably faster, since it results in only one substitution per line `.*` matches the entire line, once per line, whereas the second form results in repeated substitutions on each line `.` matches only a single character, with the replacement repeated on account of the trailing `g`.

## 18. Reverse the order of lines in a file

```
:g/.*/mo0
```

The search pattern matches all lines a line contains zero or more characters. Each line is moved, one by one, to the top of the file that is, moved after imaginary line zero. As each matched line is placed at the top, it pushes the previously moved lines down, one by one, until the last line is on top. Since all lines have a beginning, the same result can be achieved more succinctly:

```
:g/^/mo0
```

## 19. In a text-file database, on all lines not marked Paid in full, append the phrase Overdue

```
:g!/Paid in full/s/$/ Overdue/
```

or the equivalent:

```
:v/Paid in full/s/$/ Overdue/
```

To affect all lines except those matching your pattern, you add a `!` to the `:g` command, or you can just use the `v` command.

## 20. For any line that doesn’t begin with a number, move the line to the end of the file

```
:g!/^[[:digit:]]/m$
```

or:

```
:g/^[^[:digit:]]/m$
```

As the first character within brackets, a caret negates the sense, so the two commands have the same effect. The first one says, “Don’t match lines that begin with a number,” and the second one says, “Match lines that don’t begin with a number.”

Note that there is a rather subtle difference between the commands. The first one affects empty lines; the second one does not. How so? `/^[[:digit:]]/` matches lines that start with a digit. The `!` after the `:g` negates that, matching lines that don’t start with a digit. This includes empty lines. However, `/^[^[:digit:]]/` matches lines that start with a nondigit character; in order to match, there has to be a character present on the line.

## 21. Change manually numbered section heads e.g., 1.1, 1.2, etc. to HTML `<h1>` heading markers

```
:%s;^[1-9]\.[1-9] \(.*\);<h1>\1</h1>;
```

The search string matches a digit other than zero, followed by a period, followed by another nonzero digit, followed by a space, followed by anything. The command just shown won’t find chapter numbers containing two or more digits. To do so, modify the command like this:

```
:%s;^[1-9][0-9]*\.[1-9] \(.*\);<h1>\1</h1>;
```

Now it matches chapters 10 to 99 digits 1 to 9, followed by a digit, 100 to 999 digits 1 to 9, followed by two digits, and so on. The command still finds chapters 1 to 9 digits 1 to 9, followed by no digit as well.

## 22. Remove numbering from section headings in a document. You want to change the sample lines

```
2.1 Introduction
10.3.8 New Functions
```

into the lines:

```
Introduction
New Functions
```

Here’s the command to do this:
