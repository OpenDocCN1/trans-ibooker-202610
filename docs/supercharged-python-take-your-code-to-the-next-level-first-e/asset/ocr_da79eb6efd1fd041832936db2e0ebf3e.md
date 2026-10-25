will likely be out-of-date and unusable.)

```
pip3 install package_name
```

# 4.4 WRITING AND USING DOC STRINGS

Python _doc strings_ enable you to leverage the work you do writing comments to get free online help. That help is then available to you while running IDLE, as well as from the command line, when you use the **pydoc** utility.

You can write doc strings for both functions and classes. Although this book has not yet introduced how to write classes, the principles are the same. Here’s an example with a function, showcasing a doc string.

Click here to view code image

```
def quad(a, b, c):
    '''Quadratic Formula function.

    This function applies the Quadratic Formula
    to determine the roots of x in a quadratic
    equation of the form ax^2 + bx + c = 0.
    '''
    determin = (b * b - 4 * a * c) ** .5
    x1 = (-b + determin) / (2 * a)
    x2 = (-b - determin) / (2 * a)
    return x1, x2
```

When this doc string is entered in a function definition, you