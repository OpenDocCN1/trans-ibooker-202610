# 2.9.1 Concatenating strings

You can concatenate strings using “+” within print. Use str on any non-string objects when joining strings in this way.

```
print("Last year's revenue was " + "3" + " million euros")
```

Last year's revenue was 3 million euros

```
print("Last year's revenue was " + 3 + " million euros")
```

TypeError: can only concatenate str (not "int") to str

```
print("Last year's revenue was " + str(3) + " million euros")
```

Last year's revenue was 3 million euros

# 2.9.2 f-strings

Even better, use an f-string.

```
print(f"Last year's revenue was {3} million euros")
```

Last year's revenue was 3 million euros

In this case, we could have coded an expression or function call inside the braces.

```
print(f"Last year's revenue was {revenue(2020)} million euros")
```

Last year's revenue was 3 million euros

I have not defined revenue here, but you may assume it accesses a database or gets its result from a list or another data structure.

We saw in section 2.2 that we can use a quote within a string that is the same quote type as the delimiter:

```
"Don't use \"odd\" quoted words in your prose."
```

'Don\'t use "odd" quoted words in your prose.'

Python inserted a backslash in the output to escape the single quote.