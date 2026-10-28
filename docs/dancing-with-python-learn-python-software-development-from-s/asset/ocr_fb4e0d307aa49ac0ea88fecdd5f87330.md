Using the index -1 is shorthand for accessing the last character in the string. It counts backward from the end. This counts backward to the first position:

```
quote[-quote_length]
```

'p'

You can’t go too far, though, or you will be out of the valid index range.

```
quote[-quote_length - 1]
```

IndexError: string index out of range

Figure 2.5 shows the valid indices of the characters in the quote.

![](bbox=[0.17, 0.4, 0.88, 0.5])

# Exercise 2.11

Let string_index be a variable holding an index you would like to access in an expression like quote[string_index]. What are x and y in

x ≤ string_index ≤ y

if string_index is a valid index?

# 2.7.2 Searching for substrings

Use find to locate specific text within a str.

```
quote

'perchance to dream'
```

```
quote.find("dream")

13
```