```
for i, name in enumerate(beat_list, 1):
    print(i, '. ', name, sep='')
```

This prints

```
1. John
2. Paul
3. George
4. Ringo
```

There are, of course, some cases in which it’s necessary to use indexing. That happens most often when you are trying to change the contents of a list in place.

# ## 4.2.3 Understand Combined Operator Assignment (+= etc.)

The combined operator-assignment operators are introduced in Chapter 1 and so are reviewed only briefly here. Remember that assignment (=) can be combined with any of the following operators: +, -, /, //, %, **, &, ^, |, <<, >>.

The operators &, |, and ^ are bitwise “and,” “or,” and “exclusive or,” respectively. The operators << and >> perform bit shifts to the left and to the right.

This section covers some finer points of operator-assignment usage. First, any assignment operator has low precedence and is carried out last.

Second, an assignment operator may or may not be in place, depending on whether the type operated on is mutable. _In place_ refers to operations that work on existing data in memory