

earlier.
This solution—inheritance—works only when you choose to
build your collection class on top of an existing class, such as
`list` or `dict`.

# 9.10.10 Implementing “`__iter__`” and “`__next__`”

The `__iter__` and `__next__` methods, along with the
`next` function, require some elaboration. These methods enable
objects of your class to produce a _generator_ (or _iterator_), which
in turn makes it usable in certain special contexts, such as a `for`
loop. Let’s start with some terminology.

- An _iterable_ is an object that can be iterated—or rather, stepped
through—one element at a time. To be an iterable, an object must
return an object in response to `__iter__`.

- An _iterator_ is what a call to `__iter__` must return. An iterator
is an object that can be used to step through a collection object.

- This iterator, which can be the same class or a separate class
written just for this purpose, must respond to the `__next__`
method—that is, it must implement this magic method, even if it
does nothing else.

These abilities are important, because they enable instances
of a class to be used with the Python `for` keyword. For
example, suppose you have a four-dimensional Point object. If
the iterator for such an object gets one of the four coordinates at
a time, then the user of the class could use code like this:

```
my_point = Point()
for i in my_point:
    print(i)
```

This example would then print each of the four coordinates,
one at a time.

As mentioned, a collection may be its own iterator; to do so, it
returns `self` in response to the `__iter__` method, and it
implements the `__next__` method itself.

Depending on the complexity of your container and the
degree of flexibility you want, several approaches are possible to
implement these methods.

- Passing a call to `__iter__` along to a collection object contained
within the target. This is the simplest solution. It's essentially
letting someone else handle the job.

- Implementing both `__iter__` and `__next__` in the
collection class itself. The `__iter__` method returns `self` in
this case, as well as initializing the iteration settings. However, such
a solution makes it impossible to support more than one loop at a
time.

- Responding to the `__iter__` method by creating a custom
iterator object whose entire purpose is to support an iteration
through the collection class. This is the most robust, and
recommended, approach.

The next example illustrates the second approach, because it
is, in most cases, relatively simple. To use this approach for the
Stack class introduced earlier, add the following method
definitions:

```
def __iter__(self):
    self.current = 0
    return self

def __next__(self):
    if self.current < len(self):
        self.current += 1
        return self.my_list[self.current - 1]
    else:
        raise StopIteration
```

An important coding technique here is to refer to the variable, current, as self.current. This causes the variable to be an instance variable and not a class or global variable.

When all the elements in a sequence or iteration have been stepped through by incrementing self.current, over and over the __new__ method responds by raising the StopIteration exception. This has the effect of halting a for loop.

# ## 9.11 SUPPORTING MULTIPLE ARGUMENT TYPES

How do you write functions and methods that take more than one kind of argument? For example, suppose you want to write a Point class in such a way that a Point object can be multiplied either by a scalar number or by another Point object.

Python does not support overloading, but you can test the
type of the argument at run time and, depending on that type,
take a different set of actions. There are at least two ways to test
the type of an object. One technique is to call the **type** function.

![](img/6e45221a8147f2442c653a3d4a866570_595_0.png)

```
type(object)
```

For example, you can test a data object or variable directly to
see whether it has integer type.

```
n = 5
if type(n) == int:
    print('n is integer.')
```

A more reliable approach is to use the **isinstance** function.
You can use one of two different versions.

![](img/6e45221a8147f2442c653a3d4a866570_595_1.png)

Click here to view code image

```
isinstance(object, class)
isinstance(object, tuple_of_classes)
```

The first version determines the class of the object and then
returns **True** if the object’s class is either the same as the *class*
argument or is derived from this argument—that is, the object

must have a class identical to, or derived from, the second argument.

The second version of this syntax is the same, except that it enables you to include a tuple of (that is, an immutable list of) multiple classes.

Here’s an example of the first syntax:

Click here to view code image

```
n = 5
if isinstance(n, int):
    print('n is an integer or derived from it.')
```

Here’s an example of the second syntax. This technique enables you to test whether n contains any integer or floating-point number.

Click here to view code image

```
if isinstance(n, (int, float)):
    print('n is numeric.')
```

Remember that because of the use of isinstance, rather than type, n need not have int or float type; a type derived from int or float is sufficient. Such types are uncommon, but you could create one by subclassing.

So, for example, suppose you wanted to enable Point objects to support multiplication by both other Point objects and by numbers. You could do that by defining an __mul__ method as follows:

Click here to view code image

```
def __mul__(self, other):
    if type(other) == Point:
        newx = self.x * other.x
        newy = self.y * other.y
        return Point(newx, newy)
    elif type(other) == int or type(other) == float:
        newx = self.x * other
        newy = self.y * other
        return Point(newx, newy)
    else:
        return NotImplemented
```

It’s important to return **NotImplemented**, rather than raise an exception, if this class doesn’t know how to handle an operation involving the class of _other_. By returning **NotImplemented**, you enable Python to inquire of the right operand whether its class supports an __rmul__ method that would handle this situation.
This method could instead be defined by using the **isinstance** function to check on types rather than the **type** function.

Click here to view code image

```
def __mul__(self, other):
    if isinstance(other, Point):
        newx = self.x * other.x
        newy = self.y * other.y
        return Point(newx, newy)
    elif isinstance(other, (int, float)):
        newx = self.x * other
        newy = self.y * other
        return Point(newx, newy)
    else:
        return NotImplemented
```

In either case, support for multiplying by numbers is asymmetrical; such an operation may occur in an expression such as the following:

```
pt2 = 5.5 * pt1
```

The problem, of course, is that a magic method can’t be added for integers or floating point, because we did not write those classes. Therefore, a Point class __rmul__ method should be written.

Click here to view code image

```
def __rmul__(self, other):
    if isinstance(other, (int, float)):
        newx = self.x * other
        newy = self.y * other
        return Point(newx, newy)
    else:
        return NotImplemented
```

# 9.12 SETTING AND GETTING ATTRIBUTES DYNAMICALLY

A Python object can have many attributes; these include instance variables, methods, and properties. What all these attributes have in common is that they are hard-coded—that is, these names are fixed in the programming code.

But sometimes it’s useful to set an attribute _dynamically_, setting the attribute name at run time in response to runtime conditions. This enables the user to suggest an attribute name, for example, or for the attribute name to be taken from a database or other application. Chapter 15, “Getting Financial Data off the Internet,” uses this technique.

The **setattr** function has the syntax shown here.

![](img/6e45221a8147f2442c653a3d4a866570_599_0.png)

Click here to view code image

```
setattr(object, name_str, value)
```

In this syntax, _object_ is a reference to the object to be modified, _name_str is a string containing the name of the attribute, and _value_ is a data object or expression containing the value.

The **getattr** function has a complementary syntax.

![](img/6e45221a8147f2442c653a3d4a866570_599_1.png)

Click here to view code image

```
getattr(object, name_str [, default_val])
```

Here’s a simple example, entered in the IDLE environment. The attribute _breed_ is added dynamically and set to 'Great

