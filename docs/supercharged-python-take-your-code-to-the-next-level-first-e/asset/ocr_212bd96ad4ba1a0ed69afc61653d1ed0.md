Given this class definition, the following statements are
perfectly valid and do exactly what you’d expect.

```
o = Odd()
o.x    # 10
o.y    # 20
```

But the following expression causes an exception to be raised:

```
o.__z   # Error!
```

This last expression raises an error because Python replaces __
z with a mangled name, generated from a combination of the
class name and the variable name.

But __z is still accessible, without mangling, within method
definitions of the same class, and that is why the pr method still
works. Variable and method names are always accessible within
the same class. But remember that in Python, such intraclass
references need to be qualified with self.

# 9.7 INHERITANCE

Python provides support for inheritance, also known as
“subclassing.” Suppose you have a class, Mammal, that contains
most of the methods you need to use in your program. However,
you need to add or change a few of these methods. For example,
you might want to create a Dog class whose instances can do
anything a Mammal instance can do, plus more things.

The syntax for single inheritance with one base class is shown