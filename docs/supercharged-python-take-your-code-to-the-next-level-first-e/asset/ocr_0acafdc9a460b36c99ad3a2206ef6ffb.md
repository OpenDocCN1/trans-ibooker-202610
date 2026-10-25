that match a given word. For example, the following pattern matches the word “cat”; no surprise there.

```
cat
```

But what if you wanted to match a larger set of words? For example, let’s say you wanted to match the following combination of letters:

+   - Match a “c” character.
+   - Match any number of “a” characters, but at least one.
+   - Match a “t” character.

Here’s the regular expression that implements these criteria:

```
ca+t
```

With regular expressions (as with formatting specifiers in the previous chapter), there’s a fundamental difference between literal and special characters.

Literal characters, such as “c” and “t” in this example, must be matched exactly, or the result is failure to match. Most characters are literal characters, and you should assume that a character is literal unless a special character changes its meaning. All letters and digits are, by themselves, literal characters; in contrast, punctuation characters are usually special; they change the meaning of nearby characters.

The plus sign (+) is a special character. It does not cause the regular-expression processor to look for a plus sign. Instead, it