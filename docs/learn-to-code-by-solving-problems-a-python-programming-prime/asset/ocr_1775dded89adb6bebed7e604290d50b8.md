We’ll first learn about a new type that lets us represent true or false values, and how we can build expressions of this type. Then, we’ll use such expressions to write if statements.

# The Boolean Type

Pass an expression to Python’s type function, and it’ll tell you the type of the expression’s value:

```
>>> type(14)
<class 'int'>
>>> type(9.5)
<class 'float'>
>>> type('hello')
<class 'str'>
>>> type(12 + 15)
<class 'int'>
```

One Python type we haven’t met yet is the Boolean (bool) type. Unlike integers, strings, and floats, which have billions of possible values, there are only two Boolean values: True and False. These are exactly the values we need to represent the result of a condition.

```
>>> True
True
>>> False
False
>>> type(True)
<class 'bool'>
>>> type(False)
<class 'bool'>
```

What can we do with these values? With numbers, we had mathematical operators like + and - that let us combine values into more complex expressions. We’ll need a new set of operators that work with Boolean values.

# Relational Operators

Is 5 greater than 2? Is 4 less than 1? We can make such comparisons using Python’s relational operators. They produce True or False and are therefore used to write Boolean expressions.