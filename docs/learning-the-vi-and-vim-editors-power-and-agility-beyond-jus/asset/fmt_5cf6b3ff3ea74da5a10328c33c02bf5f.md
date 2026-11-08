
### `^`
When used at the start of a regular expression, require that the following regular expression be found at the beginning of the line. For example, `^Part` matches *Part* when it occurs at the beginning of a line, and `^...` matches the first three characters of a line. When not at the beginning of a regular expression, `^` stands for itself.

### `$`
When used at the end of a regular expression, require that the preceding regular expression be found at the end of the line; for example, `here:$` matches only when *here:* occurs at the end of a line. When not at the end of a regular expression, `$` stands for itself.

The `^` and `$` are referred to as *anchors*, since they anchor the match to the beginning or end of the line, respectively.

### `\`
Treat the following special character as an ordinary character. For example, `\.` matches an actual period instead of “any single character,” and `\*` matches an actual asterisk instead of “any number of a character.” The `\` (backslash) prevents the interpretation of a special character. This prevention is called “escaping the character.” Use `\\` to get a literal backslash.

### `[ ]`
Match any *one* of the characters enclosed between the brackets. For example, `[AB]` matches either *A* or *B*, and `p[aeiou]t` matches *pat*, *pet*, *pit*, *pot*, or *put*. You specify a range of consecutive characters by separating the first and last characters in the range with a hyphen. For example, `[A-Z]` matches any uppercase letter from *A* to *Z*, and `[0-9]` matches any digit from *0* to *9*.

You can include more than one range inside brackets, and you can specify a mix of ranges and separate characters. For example, `[:;A-Za-z()]` matches four different punctuation marks, plus all the English letters.

#### Note
When regular expressions and `vi` were first developed, they were meant to work only with the ASCII character set. In today’s global market, modern systems support *locales*, which provide different interpretations of the characters that lie between `a` and `z`. To get accurate results, you should use POSIX bracket expressions (discussed shortly) in your regular expressions, and avoid ranges of the form `a-z`.

Most metacharacters lose their special meaning inside brackets, so you don’t need to escape them if you want to use them as ordinary characters. Within brackets, the three metacharacters you still need to escape are `\`, `-`, and `]`. The hyphen (`-`) acquires meaning as a range specifier; to use an actual hyphen, you can also place it as the first character inside the brackets. (This is also true of `]`.) 

A caret (`^`) has special meaning only when it is the first character inside the brackets, but in this case the meaning differs from that of the normal `^` metacharacter. As the first character within brackets, the `^` reverses their sense: the brackets match any one character *not* in the list. For example, `[^0-9]` matches any character that is not a digit.

### `\( \)`
Save the subpattern enclosed between `\(` and `\)` into a special holding space, or a *hold buffer*.^([3](#ch06.html#idm46601104624912)) Up to nine subpatterns can be saved in this way on a single line. For example, the pattern

```
\(That\) or \(this\)
```

saves *That* in hold buffer number 1 and saves *this* in hold buffer number 2. The text matching the held subpatterns can be “replayed” in substitutions by the sequences `\1` to `\9`. For example, to rephrase *That or this* to read *this or That*, you could enter:

```
:%s/\(That\) or \(this\)/\2 or \1/
```

You can also use the `\`*`n`* notation within a search or substitute string. For example:

```
:s/\(abcd\)\1/alphabet-soup/
```

changes *abcdabcd* into *alphabet-soup*.

### `\< \>`
Match characters at the beginning (`\<`) or at the end (`\>`) of a word. The end or beginning of a word is determined either by a punctuation mark or by a space. For example, the expression `\<ac` matches only words that begin with *ac*, such as *action*. The expression `ac\>` matches only words that end with *ac*, such as *maniac*. Neither expression matches *react*. Note that unlike `\(…\)`, these do not have to be used in matched pairs.

In the original `vi`, there is an additional metacharacter:

### `~`
Match whatever regular expression was used in the *last* search. For example, if you searched for *The*, you could search for *Then* with `/~n`. Note that you can use this pattern only in a regular search (with `/`). It won’t work as the pattern in a substitute command. It does, however, have a similar meaning in the replacement portion of a substitute command. (This is described shortly, in the section [“Metacharacters Used in Replacement Strings”](#ch06.html#vi8-ch-6-sect-5.3).)

This use of `~` is a rather flaky feature of the original `vi`. After using it, the saved search pattern is set to the *new* text typed after the `~`, *not* the combined new pattern, as one might expect. While this feature exists, it has little to recommend its use. Also, `Vim` does not behave this way.

Note that `Vim` supports an extended regular expression syntax. See the section [“Extended Regular Expressions”](#ch08.html#vi8-ch-8-sect-7) for more information.

## POSIX Bracket Expressions
We have just described the use of brackets for matching any one of the enclosed characters, such as `[a-z]`. The POSIX standard introduced additional facilities for matching characters that are not in the English alphabet. For example, the French *è* is an alphabetic character, but the typical character class `[a-z]` would not match it. Additionally, the standard provides for sequences of characters that should be treated as a single unit when matching and collating (sorting) string data.

POSIX also formalizes the terminology. Groups of characters within brackets are called *bracket expressions* in the POSIX standard. Within bracket expressions, alongside literal characters such as *a*, *!*, and so on, you can have additional components. These components are:

### Character classes
A POSIX character class consists of keywords bracketed by `[:` and `:]`. The keywords describe different classes of characters, such as alphabetic characters, control characters, and so on (see [Table 6-1](#ch06.html#vi8-ch-6-tab-1)).

### Collating symbols
A collating symbol is a multicharacter sequence that should be treated as a unit. It consists of the characters bracketed by `[.` and `.]`.

### Equivalence classes
An equivalence class lists a set of characters that should be considered equivalent, such as *e* and *è*. It consists of a named element from the locale, bracketed by `[=` and `=]`.

All three of these constructs *must* appear inside the square brackets of a bracket expression. For example, `[[:alpha:]!]` matches any single alphabetic character or the exclamation point, `[[.ch.]]` matches the collating element *ch* but does not match just the letter *c* or the letter *h*. In a French locale, `[[=e=]]` might match any of *e*, *è*, or *é*. Classes and matching characters are shown in [Table 6-1](#ch06.html#vi8-ch-6-tab-1).

Table 6-1. POSIX character classes

| Class || Matching characters |
| --- | --- |
| `[:alnum:]` || Alphanumeric characters |
| --- | --- |
| `[:alpha:]` || Alphabetic characters |
| `[:blank:]` || Space and tab characters only |
| `[:cntrl:]` || Control characters |
| `[:digit:]` || Numeric characters |
| `[:graph:]` || Printable and visible (nonspace) characters |
| `[:lower:]` || Lowercase characters |
| `[:print:]` || Printable characters (includes whitespace) |
| `[:punct:]` || Punctuation characters |
| `[:space:]` || All whitespace characters (space, tab, newline, vertical tab, etc.) |
| `[:upper:]` || Uppercase characters |
| `[:xdigit:]` || Hexadecimal digits |
