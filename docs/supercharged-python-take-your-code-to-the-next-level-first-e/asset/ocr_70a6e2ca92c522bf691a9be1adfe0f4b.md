# Let's break down what these symbols mean.

+   - The _fill_ and _align_ characters are `*` and `<`, respectively. The `<` symbol specifies left justification, so asterisks are used for padding on the right, if needed.
+   - The _width_ character is 6, so any string shorter than 6 characters in length is padded after being left justified.
+   - The _precision_ (the character after the dot) is also 6, so any string longer than 6 characters is truncated.

# Let's apply this format to several strings.

```
```python
print(fss.format('Tom'))
print(fss.format('Mike'))
print(fss.format('Rodney'))
print(fss.format('Hannibal'))
print(fss.format('Mortimer'))
```
```

These statements could have easily been written by using the global `format` function. Notice the similarities as well as the differences; the previous examples involved the format string `'{ :*<6.6 }'`.

```
```python
print(format('Tom', '*<6.6'))
print(format('Mike', '*<6.6'))
print(format('Rodney', '*<6.6'))
print(format('Hannibal', '*<6.6'))
print(format('Mortimer', '*<6.6'))
```
```

In either case—that is, for either block of code just shown— the output is