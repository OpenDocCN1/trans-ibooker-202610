newlines.

Click here to view code image

```
'''You can't get it at "Alice's Restaurant."'''
```

Alternatively, you can place embedded quotation marks into
a string by using the backslash (\) as an escape character.

Click here to view code image

```
s2 = 'It\'s not true, it just ain\'t!'
```

Chapter 2, “Advanced String Capabilities,” provides a nearly
exhaustive tour of string capabilities.

You can deconstruct strings in Python, just as you can in
Basic or C, by indexing individual characters, using indexes
running from 0 to N–1, where N is the length of the string.
Here’s an example:

```
s = 'Hello'
s[0]
```

This produces

```
'H'
```

However, you cannot assign new values to characters within
existing strings, because Python strings are immutable: They
cannot be changed.

How, then, can new strings be constructed? You do that by