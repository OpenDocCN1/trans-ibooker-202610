```
break
```

However, you can instead test for an input string this way:

```
if not s:
    break
```

Here are the general guidelines for Boolean conversions.

+   • Nonempty collections and nonempty strings evaluate as **True**; so do nonzero numeric values.

+   • Zero-length collections and zero-length strings evaluate to **False**; so does any number equal to 0, as well as the special value **None**.

# ## 4.2.11 Treat Strings as Lists of Characters

When you’re doing complicated operations on individual characters and building a string, it’s sometimes more efficient to build a list of characters (each being a string of length 1) and use list comprehension plus **join** to put it all together.

For example, to test whether a string is a palindrome, it’s useful to omit all punctuation and space characters and convert the rest of the string to either all-uppercase or all-lowercase. List comprehension does this efficiently.

Click here to view code image

```
test_str = input('Enter test string: ')
a_list = [c.upper() for c in test_str if
c.isalnum()]
print(a_list == a_list[::-1])
```