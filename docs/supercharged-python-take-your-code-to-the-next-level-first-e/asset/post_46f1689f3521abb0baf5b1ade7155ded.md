

## 4.2.4 Use Multiple Assignment

Multiple assignment is one of the most commonly used coding shortcuts in Python. You can, for example, create five different variables at once, assigning them all the same value—in this case, 0:

```python
a = b = c = d = e = 0
```

Consequently, the following returns **True**:

```python
a is b
```

This statement would no longer return **True** if either of these variables was later assigned to a different object.

Even though this coding technique may look like it is borrowed from C and C++, you should not assume that Python follows C syntax in most respects. Assignment in Python is a statement and not an expression, as it is in C.

## 4.2.5 Use Tuple Assignment

Multiple assignment is useful when you want to assign a group of variables the same initial value.

But what if you want to assign different values to different variables? For example, suppose you want to assign 1 to a, and 0 to b. The obvious way to do that is to use the following statements:

```python
a = 1
b = 0
```

But through *tuple assignment*, you can combine these into a single statement.

```python
a, b = 1, 0
```

In this form of assignment, you have a series of values on one side of the equals sign (=) and another on the right. They must match in number, with one exception: You can assign a tuple of any size to a single variable (which itself now represents a tuple as a result of this operation).

Click here to view code image

```python
a = 4, 8, 12 # a is now a tuple containing three values.
```

Tuple assignment can be used to write some passages of code more compactly. Consider how compact a Fibonacci-generating function can be in Python.

```python
def fibo(n):
    a, b = 1, 0
    while a <= n:
        print(a, end=' ')
        a, b = a + b, a
```

In the last statement, the variable `a` gets a new value: `a + b`;
the variable `b` gets a new value—namely, the old value of `a`.

Most programming languages have no way to set `a` and `b`
simultaneously. Setting the value of `a` changes what gets put
into `b`, and vice versa. So normally, a temporary variable would
be required. You could do that in Python, if you wanted to:

Click here to view code image

```python
temp = a    # Preserve old value of a
a = a + b   # Set new value of a
b = temp    # Set b to old value of a
```

But with tuple assignment, there’s no need for a temporary variable.

```python
a, b = a + b, a
```

Here’s an even simpler example of tuple assignment.
Sometimes, it’s useful to swap two values.

```python
x, y = 1, 25
print(x, y)   # prints 1 25
x, y = y, x
print(x, y)   # prints 25 1
```

The interesting part of this example is the statement that performs the swap:

```python
x, y = y, x
```

In another language, such an action would require three separate statements. But Python does not require this, because—as just shown—it can do the swap all at once. Here is what another language would require you to do:

```python
temp = x
x = y
y = temp
```

## 4.2.6 Use Advanced Tuple Assignment

Tuple assignment has some refined features. For example, you can unpack a tuple to assign to multiple variables, as in the following example.

Click here to view code image

```python
tup = 10, 20, 30
a, b, c = tup
print(a, b, c) # Produces 10, 20, 30
```

It’s important that the number of input variables on the left matches the size of the tuple on the right. The following statement would produce a runtime error.

Click here to view code image

```python
tup = 10, 20, 30
a, b = tup # Error: too many values to unpack
```

Another technique that’s occasionally useful is creating a tuple that has one element. That would be easy to do with lists.

```python
my_list = [3]
```

This is a list with one element, 3. But the same approach won't work with tuples.

```python
my_tup = (3)
print(type(my_tup))
```

This **print** statement shows that `my_tup`, in this case, produced a simple integer.

```python
<class 'int'>
```

This is not what was wanted in this case. The parentheses were treated as a no-op, as would any number of enclosing parentheses. But the following statement produces a tuple with one element, although, to be fair, a tuple with just one element isn’t used very often.

Click here to view code image

```python
my_tup = (3,)   # Assign tuple with one member, 3.
```

The use of an asterisk (*) provides a good deal of additional flexibility with tuple assignment. You can use it to split off parts of a tuple and have one (and only one) variable that becomes the default target for the remaining elements, which are then put into a list. Some examples should make this clear.

```python
a, *b = 2, 4, 6, 8
```

In this example, `a` gets the value 2, and `b` is assigned to a list:

```python
2
[4, 6, 8]
```

```python
a, *b, c = 10, 20, 30, 40, 50
```

In this case, `a` and `c` refer to 10 and 50, respectively, after this statement is executed, and `b` is assigned the list `[20, 30, 40]`.

You can, of course, place the asterisk next to a variable at the end.

Click here to view code image

```python
big, bigger, *many = 100, 200, 300, 400, 500, 600
```

Printing these variables produces the following:

Click here to view code image

```python
>>> print(big, bigger, many, sep='\n')
100
200
[300, 400, 500, 600]
```

## 4.2.7 Use List and String “Multiplication”

Serious programs often deal with large data sets—for example, a collection of 10,000 integers all initialized to 0. In languages such as C and Java, the way to do this is to first declare an array with a large dimension.

Because there are no data declarations in Python, the only way to create a large list is to construct it on the right side of an assignment. But constructing a super-long list by hand is impractical. Imagine trying to construct a super-long list this way:

Click here to view code image

```python
my_list = [0, 0, 0, 0, 0, 0, 0, 0...]
```

As you can imagine, entering 10,000 zeros into program code would be very time-consuming! And it would make your hands ache.

Applying the multiplication operator provides a more practical solution:

```python
my_list = [0] * 10000
```

This example creates a list of 10,000 integers, all initialized to 0.

Such operations are well optimized in Python, so that even in the interactive development environment (IDLE), such interactions are handled quickly.

