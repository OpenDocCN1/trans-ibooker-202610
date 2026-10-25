To employ this regular-expression pattern successfully with
re.findall, so that you can find multiple numbers, two
things need to be done.

First, the pattern needs to end with a word boundary (\b).
Otherwise, it matches two numerals stuck together, an outcome
that, unfortunately, compromises one long number that is not
valid.

This number would be incorrectly accepted, because
findall accepts 1,200 and then accepts 10, given the current
pattern.

The solution is to use \b, the end-of-word meta character. To
get a correct match, the regular-expression evaluator must find
an end-of-word transition: This can be a space, a punctuation
mark, end of line, or the end of the string.

There also remains the issue of tagged groups. The problem is
that with the following string (which now includes the word
boundary), grouping is necessary to express all the subpatterns.

Click here to view code image

```
r'\d{1,3} (, \d{3})* (\.\d*)?\b'
```

Let’s review what this means.

+   • The characters \d{1, 3} say, “Match between 1 and 3 digits.”