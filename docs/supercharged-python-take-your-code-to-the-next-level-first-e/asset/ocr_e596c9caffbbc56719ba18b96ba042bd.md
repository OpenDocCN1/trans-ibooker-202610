```
pt3 = pt1 + pt2
```

Assuming addition (+) is supported in this way—adding a Point instance to another Point instance—then __add__ will be called through the left operand in this case (pt1); so therefore an __radd__ would never be called through the right operand.

Symmetrical operations (a point added to a point) never invoke any of the right-side r magic methods.

Instead, the r methods are useful in asymmetrical situations —when an integer, for example, may be multiplied with a Point object. Suppose you want to support both of the following expressions:

```
pt3 = pt1 * 5
pt3 = 10 * pt1
```

The first of these two expressions results in invocation of the __mul__ method, automatically called through the left operand, pt1.

The second statement is more problematic, because 10 is an integer, and the integer class (int) does not support multiplication with Point objects. Therefore, this statement requires that the Point class __rmul__ method be implemented.

> Click here to view code image

```
def __rmul__(self, n):
    ''' Return point * a scalar number, n '''
    newx = self.x * n
```