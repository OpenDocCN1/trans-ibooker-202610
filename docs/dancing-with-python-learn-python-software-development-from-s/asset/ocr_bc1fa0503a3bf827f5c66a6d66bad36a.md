Here we lost precision when we had too many digits to the right of the decimal point for Python to store. If there are too many on the left, Python uses an exponential form.

```
100000000000000000000000000000000000000000000000000000000000.1
```

```
1e+52
```

That result means 1 × 10⁵². The ".1" was lost.

Python allows you to insert underscores in numbers to make them more readable.

```
1_000_000_000
```

```
1000000000
```

# 2.2 Strings

We use strings for text. They are a sequence of characters.

```
'Better a witty fool, than a foolish wit.'
```

```
'Better a witty fool, than a foolish wit.'
```

If I am manipulating the text of Shakespeare’s Twelfth Night, I can represent it this way. I delimited the string at its beginning and end with single quotes. Notice that Python displayed single quotes in its output. You may use either single or double quotes as delimiters; they are not part of the data representing the string and its contained characters.

```
"Some are born great, others achieve greatness."
```

```
'Some are born great, others achieve greatness.'
```

If you want to create a string that includes a single quote, use double quotes as the delimiters. Reverse this if your string includes a double quote. If I can use either, my convention is to use single quotes for strings of length one (for example, 'Z') and double quotes otherwise (for example, "hola" or "").

```
"I use single quotes around single characters like 'Z'."
```

```
"I use single quotes around single characters like 'Z'."
```