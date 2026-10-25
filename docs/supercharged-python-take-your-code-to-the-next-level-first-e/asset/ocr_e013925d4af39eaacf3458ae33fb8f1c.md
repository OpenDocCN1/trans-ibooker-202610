that can be used to illustrate the use of some of these magic methods. To do that, we need to add the following method definition to the existing Point class definition:

Click here to view code image

```
def __neg__(self):
    newx = -self.x
    newy = -self.y
    return Point(newx, newy)
```

The following expression, for example, is not meant to change the internal value of my_point. Rather, it’s intended to produce a new value that can be assigned or used.

```
-my_point
```

With this definition in place, we can now create Point class instances (Point objects) and apply arithmetic negation.

Click here to view code image

```
pt1 = Point(3, 4)
pt2 = -pt1
print(pt2.x, ', ', pt2.y, sep='')
```

This example prints

```
-3, -4
```

This result is, as you would hope, the arithmetic negation of a Point instance set equal to 3, 4. Its negation is a new instance of