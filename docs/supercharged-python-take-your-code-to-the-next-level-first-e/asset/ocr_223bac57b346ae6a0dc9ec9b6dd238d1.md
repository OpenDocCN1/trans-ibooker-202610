Click here to view code image

```
s = 'The United States of of America.'
m = re.search(r'(\w+) \\1', s)
print(m.group(), '...found at', m.span())

```

This example prints the result

```
of of ...found at (18, 23)
```

As with all other regular-expression matches and searches,
the Python implementation makes it easy to make comparisons
case-insensitive, which can be useful in this case. Consider this
text string:

Click here to view code image

```
s = 'The the cow jumped over the the moon.'
```

Can we do a search that indicates a repeated word at the
beginning of this sentence, or not? Yes, because all we have to
do is specify the **re.IGNORECASE** flag, or **re.I**, for short.

Click here to view code image

```
m = re.search(r'(\w+) \\1', s, flags=re.I)
print(m.group(), '...found at', m.span())

```

This example prints

```
The the ...found at (0, 7)
```