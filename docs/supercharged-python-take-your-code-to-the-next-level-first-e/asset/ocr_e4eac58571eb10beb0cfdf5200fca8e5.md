Prints help documentation for the specified object's class. This is used often within IDLE. If no object is specified, then an introductory page for the help system for Python is printed.

# # HEX(N)

Hexadecimal conversion. Returns a string containing a hexadecimal-radix (base 16) representation of integer _n_, including the prefix, `0x`. Inputs other than integer cause Python to raise a `TypeError` exception.

For example:

```
>>> hex(23)
'0x17'
```

# # ID(OBJ)

Returns a unique identifier for the object, `obj`. If two variables (`obj1` and `obj2`) have the same identifier, then the expression `obj1 is obj2` returns `True`—meaning that they refer to the same object in memory. (Note: Do not confuse this with test for equality, `==`, which is a less restrictive condition.)

# # INPUT([PROMPT_STR])

Input function. Returns a string input by the user after waiting for the user to input zero or more characters and press Enter. The _prompt_str_, if specified, prints a string for prompting the user, such as “Enter name here: ”. No extra space is automatically printed after this prompt string, so you may want