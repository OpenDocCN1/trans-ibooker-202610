![](bbox=[0.13, 0.09, 0.82, 0.53])

Figure 2-17. A + (B + C)

To code point addition, we're going to split it up into three steps:

+   1. Where the points are in a vertical line or using the identity point
+   2. Where the points are not in a vertical line, but are different
+   3. Where the two points are the same

# Coding Point Addition

We first handle the identity point, or point at infinity. Since we can't easily use infinity in Python, we'll use the None value instead. What we want is this to work:

```
>>> from ecc import Point
>>> p1 = Point(-1, -1, 5, 7)
>>> p2 = Point(-1, 1, 5, 7)
>>> inf = Point(None, None, 5, 7)
>>> print(p1 + inf)
Point(-1, -1)_5_7
```