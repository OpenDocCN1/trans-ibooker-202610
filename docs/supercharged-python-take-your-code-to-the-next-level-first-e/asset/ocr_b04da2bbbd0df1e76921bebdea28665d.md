forms a subexpression, together with “a” that says, “Match one
or more ‘a’ characters.”
The pattern `ca+t` therefore matches any of the following:

```
cat
caat
caaat
caaaat
```

What if you wanted to match an _actual_ plus sign? In that
case, you’d use a backslash (`\`) to create an escape sequence.
One of the functions of escape sequences is to turn a special
character back into a literal character.
So the following regular expression matches `ca+t` exactly:

```
ca\+t
```

Another important operator is the multiplication sign (`*`),
which means “_zero or more_ occurrences of the preceding
expression.” Therefore, the expression `ca*t` matches any of the
following:

```
ct
cat
caat
caaaaaaat
```

Notably, this pattern matches “ct”. It’s important to keep in
mind that the asterisk is an expression modifier and should not
be evaluated separately. Instead, observe this rule.