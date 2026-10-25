```
if re.fullmatch(pattern, s):
    print('Number accepted.')
else:
    print('Incorrect format.')
```

So far, this chapter has only scratched the surface of what regular-expression syntax can do. Section 6.5 explains the syntax in greater detail. But in mastering this syntax, there are several principles to keep in mind.

+   - A number of characters have special meaning when placed in a regular-expression pattern. It's a good idea to become familiar with all of them. These include most punctuation characters, such as + and *.
+   - Any characters that do not have special meaning to the Python regular-expression interpreter are considered literal characters. The regular-expression interpreter attempts to match these exactly.
+   - The backslash can be used to “escape” special characters, making them into literal characters. The backslash can also add special meaning to certain ordinary characters—for example, causing \d to mean “any digit” rather than a “d”.

Admittedly, this might be a little confusing at first. If a character (such as *) is special to begin with, escaping it (preceding it with a backslash) takes away that special meaning. But in other cases, escaping a character _gives_ it special meaning.

Yes, both those things are true! But if you look at enough examples, it should make sense.

Here’s a short program that tests for the validity of Social Security numbers. It’s similar, but not identical, to that for