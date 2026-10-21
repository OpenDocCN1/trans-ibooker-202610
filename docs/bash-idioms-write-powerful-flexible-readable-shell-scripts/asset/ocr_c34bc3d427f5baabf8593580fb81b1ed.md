On the other hand, if you are submitting a patch or maintaining code, it’s best to follow the conventions of that code unless you’re going to restyle and possibly refactor the whole thing.

# Comments

A lot has been written about comments: what, when, where, and so on. Following are style guidelines related to comments:

+   - *Always use a header.*
+   - Write your comments for the new person on the team a year from now.
+   - Comment your functions.
+   - Do not comment on what you did. Comment on why you did, or did not do, something.
+     — Exception: comment on what you did when bash itself is obscure.
+   - Consider adding comments about external program options, especially if they are short or obscure.
+   - Use an initial capital on the first word of the comment, but omit ending punctuation unless the comment is more than one sentence.

Add *useful* comments that explain *why* you did something and your intent! For example:

```
continue # To the next file in for loop
```

In theory it should not be necessary to explain *what* something does if you write clear code. But sometimes bash itself is just convoluted or obscure, e.g., variable substitution (see “Path or Prefix Removal” on page 33), like:

```
PROGRAM=${0##*/}  # Basename in bash
```

Separators are often useful for delimited logical blocks of code. But don’t add a closing separator at the bottom to “box” it out; that just adds clutter and reduces your on-screen lines of code by 1. Whatever you do, do *not* build boxes with characters on the righthand side! It’s totally unnecessary, and you’ll waste a lot of time getting it to “look right” up front. What’s worse is that it’s a strong disincentive to fixing or updating comments later, because then you have to go fix the box again.

Don’t do this: