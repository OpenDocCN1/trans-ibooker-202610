and expressions, which Python evaluates and prints the value
of.

You can follow along with this sample session, which shows
input for you to enter in bold. The nonbold characters represent
text printed by the environment.

```
>>> a = 10
>>> b = 20
>>> c = 30
>>> a + b + c
60
```

This “program” places the values 10, 20, and 30 into three
variables and adds them together. So far, so good, but not
amazing.

If it helps you in the beginning, you can think of variables as
storage locations into which to place values, even though that’s
not precisely what Python does.

What Python really does is make `a`, `b`, and `c` into names for
the values 10, 20, and 30. By this we mean “names” in the
ordinary sense of the word. These names are looked up in a
symbol table; they do not correspond to fixed places in
memory! The difference doesn’t matter now, but it will later,
when we get to functions and global variables. These
statements, which create `a`, `b`, and `c` as names, are _assignments_.

In any case, you can assign new values to a variable once it’s
created. So in the following example, it looks as if we’re
incrementing a value stored in a magical box (even though
we’re really not doing that).