Another way to make a string is to replace a substring with something else.

```
"My favorite guitar is a Fender.".replace("Fender", "Gibson")
```

'My favorite guitar is a Gibson.'

By default, this method replaces all occurrences. You can restrict the number of changes to a given maximum by using a third argument.

```
"My favorite guitar is a Fender.".replace("e", "E")
```

'My favoritE guitar is a FEndEr.'

```
"My favorite guitar is a Fender.".replace("e", "E", 1)
```

'My favoritE guitar is a Fender.'

Another use of replace is removing a substring. Just substitute an empty string "" for the substring.

```
"My favorite guitar is a Fender.".replace(" guitar", "")
```

'My favorite is a Fender.'

![](bbox=[0.14, 0.5, 0.22, 0.62])

# Exercise 4.1

Redo the last example with find to locate " guitar", and then slice and concatenate.

# 4.5 Strings and iterations

Another way to access the characters in a string is to iterate across it with a for loop.

```
characters = []

for c in "Fender":
    characters.append(c)

characters
```

['F', 'e', 'n', 'd', 'e', 'r']

```
guitars
```

'Fender Gibson Taylor'