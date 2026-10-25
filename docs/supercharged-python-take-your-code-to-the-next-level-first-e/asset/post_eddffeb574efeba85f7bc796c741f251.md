

## 7.6 NEGATIVE LOOK-AHEAD

An alternative to the look-ahead capability is the negative look-ahead capability.

The former says, “Consider this pattern a match only if the next characters to be read (ahead of the current position) match a certain subpattern; but in any case, don’t consume those look-ahead characters but leave them to be read.”

The negative look-ahead capability does the same thing but checks to see whether the next characters fail to match a certain subpattern. Only if there is a fail does the overall match succeed.

This is less complicated than it may sound.

![](img/6e45221a8147f2442c653a3d4a866570_439_0.png)

```
(?!expr)
```

This negative look-ahead syntax says, “Permit a match only if the next characters to be read are not matched by expr; but in any case, do not consume the look-ahead characters but leave them to be read again by the next match attempt.”

Here’s a simple example. The following pattern matches abc but only if not followed by another instance of abc.

```
pat = r'abc(?!abc)'
```

If used with `re.findall` to search the following string, it will find exactly one copy of abc:

```
s = 'The magic of abcabc.'
```

In this case, the second instance of abc will be found but not the first. Note also that because this is a look-ahead operation, the second instance of abc is not consumed, but remains to be read; otherwise, that instance would not be found either.

Here’s the code that implements the example:

```
import re
pat = r'abc(?!abc)'
s = 'The magic of abcabc.'
m = re.findall(pat, s)
print(m)
```

Remember what this (admittedly strange) pattern says:
“Match ‘abc’ but only if it’s not immediately followed by another instance of ‘abc.’”
As expected, this example prints a group with just one instance of “abc,” not two.

```
['abc']
```

Here’s an even clearer demonstration. We can distinguish between instances of abc by putting the second instance in capital letters and then turning on the IGNORECASE flag (`re.I`).

```
pat = r'abc (?!abc)'
s = 'The magic of abcABC.'
m = re.findall(pat, s, flags=re.I)
print(m)
```

Notice that the key characters, indicating negative look-ahead, are in bold.
The following text is printed, confirming that only the second instance of “abc” (this one in capital letters) is matched. The first group failed to match not because it was lowercase, but because there was a negative look-ahead condition (“Don’t find another occurrence of ‘abc’ immediately after this one”). So this example prints

```
['ABC']
```

Now let’s return to the use of positive look-ahead in the previous section and see how it’s used to read complete sentences, while distinguishing between abbreviations and decimal points rather than mistaking them for full stops. Here again is some sample test data that we need our sentence scanner to read correctly:

```
s = '''See the U.S.A. today. It's right here, not
a world away. Average temp. is 70.5.'''
```

Instead of reaching the end of a sentence and looking for a positive look-ahead condition, we can specify a negative condition to achieve similar results. To represent the end of a sentence, a period (.) must not be followed by either of these:

- A space and then a lowercase letter or digit
- Any alphanumeric character

A sentence pattern using a negative look-ahead condition could be written the following way, in which the key characters (indicating negative look-ahead) are bold:

```
r'[A-Z].*?[.!?](?! [a-z0-9]|\w)'
```

The negative look-ahead component of this pattern is (?! [a-z0-9]|\w), which says, “Don’t match a space followed by a lowercase letter or digit, and don’t match any alphanumeric character, right after the current position.”

We can use this pattern in the context of a complete example. To better test the pattern, we’ve added another sentence.

```
import re # Use if you haven't put this in
 # the source file yet.
pat = r'[A-Z].*?[.!?](?! [a-z]|\w)'
s = """See the U.S.A. today. It's right here, not
a world away. Average temp. is 70.5. It's fun!"""
m = re.findall(pat, s, flags=re.DOTALL)
for i in m:
 print('->', i)
```

This example prints the following results:

```
-> See the U.S.A. today.
-> It's right here, not
a world away.
-> Average temp. is 70.5.
-> It's fun!
```

These are the same results we’d get with positive look-ahead, although, of course, the look-ahead condition was phrased in a negative, rather than positive, way.

There are a number of ways available to get rid of the newline if it isn't desired. If you've just read all the text from a text file into a single string, for example, you can remove all the newlines with the following statement:

```
s = re.sub(r'\n', '', s)
```

If you remove newlines this way and run the example again, you'll get this output:

```
-> See the U.S.A. today.
-> It's right here, not a world away.
-> Average temp. is 70.5.
-> It's fun!
```

## 7.7 NAMED GROUPS

As we explained in Chapter 6, tagged groups are available by number. The overall string matched is available through the match object as

```
match_obj.group(0)
```

Individual tagged groups are available by using the numbers 1, 2, 3, and so on. For example, the following refers to the first tagged group:

```
match_obj.group(1)
```

But if you’re dealing with a particularly complex regular expression, you may want to refer to tagged groups not by number but by name. In that case, you may want to use named groups.

![](img/6e45221a8147f2442c653a3d4a866570_444_0.png)

```
(?P<name>expr) # Tags the matching group, using name.
(?P=name) # Attempt to match repeat of named group.
```

Let’s look at an example that’s practical but simple. A common action for a program is to take a name entered in one format and save it in another. For example, names might be entered as follows:

```
Brian R. Overland
John R. Bennett
John Q. Public
```

A common operation is to take these names and store them by last name (surname) rather than first, so you get

```
Overland, Brian R.
Bennett, John R.
```

