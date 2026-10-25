![](bbox=[0.146, 0.088, 0.253, 0.168])

Click here to view code image

```
```
str.strip(extra_chars=' ')  # Strip leading &
trailing.
str.lstrip(extra_chars=' ')  # String leading chars.
str.rstrip(extra_chars=' ')  # String trailing
chars.
```
```

Each of these method calls produces a string that has trailing
or leading characters (or both) to be stripped out.

The **lstrip** method strips only leading characters, and the
**rstrip** method strips only trailing characters, but otherwise
all three methods perform the same job. The **strip** method
strips both leading and trailing characters.

With each method, if the *extra_chars* argument is
specified, the method strips all occurrences of each and every
character in the *extra_chars* string. For example, if the string
contains *+0, then the method strips all leading or trailing
asterisks (*) as well as all leading or trailing “0” digits and plus
signs (+).

Internal instances of the character to be stripped are left
alone. For example, the following statement strips leading and
trailing spaces but not the space in the middle.

```
```
name_str = '  Will Shakes  '
new_str = name_str.strip()
```
```