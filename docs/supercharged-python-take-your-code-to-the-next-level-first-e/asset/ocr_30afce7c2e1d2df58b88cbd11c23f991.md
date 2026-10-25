```
print(bin(10), oct(10), hex(10))
```

This prints the following:

```
0b1010 0o12 0xa
```

As you can see, these three functions automatically use the prefixes “ob,” “oo,” and “ox.”

# ## 2.9 SIMPLE BOOLEAN (“IS”) METHODS

These methods—all of which begin with the word “is” in their name—return either **True** or **False**. They are often used with single-character strings but can also be used on longer strings; in that case, they return **True** if and only if every character in the string passes the test. Table 2.3 shows the Boolean methods of strings.

# ### Table 2.3. Boolean Methods of Strings

| Method name/syntax | Returns True if string passes this test |
|---|---|