Real programs wouldn’t evaluate expressions whose values we already know. We don’t need Python to tell us that 15 doesn’t equal 10. More typically, we’d use variables in these kinds of expressions. For example, number != 10 is an expression whose value depends on what number refers to.

The relational operators also work on strings. When checking equality, case matters:

```
>>> 'hello' == 'hello'
True
>>> 'Hello' == 'hello'
False
```

One string is less than another if it comes first in alphabetical order:

```
>>> 'brave' < 'cave'
True
>>> 'cave' < 'cavern'
True
>>> 'orange' < 'apple'
False
```

But things can be surprising when lowercase and uppercase characters are both involved:

```
>>> 'apple' < 'Banana'
False
```

Weird, right? It has to do with the way that characters are stored internally in a computer. Generally, uppercase characters come alphabetically before lowercase characters. And check this out:

```
>>> '10' < '4'
True
```

If these were numbers, then the result would be False. But strings are compared character by character from left to right. Python compares the '1' and '4', and because '1' is smaller, the < operator returns True. Be sure that your values have the types you think they have!

One relational operator that works on strings but not numbers is in. It returns True if the first string occurs at least once in the second, and False otherwise: