decoration with @classmethod and @staticmethod, which
create class and static methods, respectively.

A class method has access to the symbols defined within the
class, and it starts with an extra argument, which by default is
cls and refers to the class itself.

A static method is defined in a class but has no access to class
or instance variables.

For example, the following code defines a class method,
set_xy, and a static method, bar. Both are methods of class
foo, and both are called through the class name. They can also
be called through any instance of foo.

```
>>> class foo:
... x = y = 0 # class vars
...
... @classmethod
... def set_xy(cls, n, m):
... cls.x = n
... cls.y = m
...
... @staticmethod
... def bar():
... return 100
...
>>> foo.set_xy(10, 20)
>>> foo.x, foo.y
(10, 20)
>>> foo.bar()
100
```

See Chapter 9, “Classes and Magic Methods,” for more
information.