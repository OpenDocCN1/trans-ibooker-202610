
Real numbers are easy to plot on a graph. For example, $y^2 = x^3 + 7$ can be plotted like Figure 3-1.

![](img/6c915536bcfd43e412f7adef942edf23_67_0.png)

Figure 3-1. secp256k1 over real numbers

It turns out we can use the point addition equations over any field, including the finite fields we learned about in Chapter 1. The only difference is that we have to use the addition/subtraction/multiplication/division as defined in Chapter 1, not the “normal” versions that the real numbers use.

## Elliptic Curves over Finite Fields

So what does an elliptic curve over a finite field look like? Let’s look at the equation $y^2 = x^3 + 7$ over $F_{103}$. We can verify that the point $(17,64)$ is on the curve by calculating both sides of the equation:

```
y² = 64² % 103 = 79
x³ + 7 = (17³ + 7) % 103 = 79
```

We’ve verified that the point is on the curve using finite field math.

Because we're evaluating the equation over a finite field, the plot of the equation looks vastly different (Figure 3-2).

![](img/6c915536bcfd43e412f7adef942edf23_68_0.png)

Figure 3-2. Elliptic curve over a finite field

As you can see, it's very much a scattershot of points and there's no smooth curve here. This is not surprising since the points are discrete. About the only pattern is that the curve is symmetric right around the middle, because of the $y^2$ term. The graph is not symmetric over the x-axis as in the curve over reals, but about halfway up the y-axis due to there not being negative numbers in a finite field.

What's amazing is that we can use the same point addition equations with the addition, subtraction, multiplication, division, and exponentiation as we defined them for finite fields, and everything still works. This may seem surprising, but abstract math has regularities like this despite being different from the traditional modes of calculation you may be familiar with.

## Exercise 1

Evaluate whether these points are on the curve $y^2 = x^3 + 7$ over $F_{223}$:

$(192,105), (17,56), (200,119), (1,193), (42,99)$

## Coding Elliptic Curves over Finite Fields

Because we defined an elliptic curve point and defined the +, -, * and / operators for finite fields, we can combine the two classes to create elliptic curve points over a finite field:

```
>>> from ecc import FieldElement, Point
>>> a = FieldElement(num=0, prime=223)
>>> b = FieldElement(num=7, prime=223)
>>> x = FieldElement(num=192, prime=223)
>>> y = FieldElement(num=105, prime=223)
>>> p1 = Point(x, y, a, b)
>>> print(p1)
Point(192,105)_0_7 FieldElement(223)
```

When initializing Point, we will run through this part of the code:

```
class Point:

    def __init__(self, x, y, a, b):
        self.a = a
        self.b = b
        self.x = x
        self.y = y
        if self.x is None and self.y is None:
            return
        if self.y**2 != self.x**3 + a * x + b:
            raise ValueError('({}, {}) is not on the curve'.format(x, y))
```

The addition (+), multiplication (*), exponentiation (**), and not equals (!=) operators here use the `__add__`, `__mul__`, `__pow__`, and `__ne__` methods from `FieldElement`, respectively, and not the integer equivalents. Being able to do the same equation but with different definitions for the basic arithmetic operators is how we construct an elliptic curve cryptography library.

We’ve already coded the two classes that we need to implement elliptic curve points over a finite field. However, to check our work, it will be useful to create a test suite. We will do this using the results of Exercise 2:

```
class ECCTest(TestCase):

    def test_on_curve(self):
        prime = 223
        a = FieldElement(0, prime)
```

```
b = FieldElement(7, prime)
valid_points = ((192, 105), (17, 56), (1, 193))
invalid_points = ((200, 119), (42, 99))
for x_raw, y_raw in valid_points:
    x = FieldElement(x_raw, prime)
    y = FieldElement(y_raw, prime)
    Point(x, y, a, b) ①
for x_raw, y_raw in invalid_points:
    x = FieldElement(x_raw, prime)
    y = FieldElement(y_raw, prime)
    with self.assertRaises(ValueError):
        Point(x, y, a, b) ①
```

① We pass in `FieldElement` objects to the `Point` class for initialization. This will, in turn, use all the overloaded math operations in `FieldElement`.

We can now run this test like so:

```
>>> import ecc
>>> from helper import run ①
>>> run(ecc.ECCTest('test_on_curve'))
.
----------------------------------------------------------------------
Ran 1 test in 0.001s

OK
```

① `helper` is a module with some very useful utility functions, including the ability to run unit tests individually.

## Point Addition over Finite Fields

We can use all the same equations over finite fields, including the linear equation:

y = mx + b

It turns out that a “line” in a finite field is not quite what you’d expect (Figure 3-3).

## Coding Point Addition over Finite Fields

Because we coded `FieldElement` in such a way as to define `__add__`, `__sub__`, `__mul__`, `__truediv__`, `__pow__`, `__eq__`, and `__ne__`, we can simply initialize `Point` with `FieldElement` objects and point addition will work:

```python
>>> from ecc import FieldElement, Point
>>> prime = 223
>>> a = FieldElement(num=0, prime=prime)
>>> b = FieldElement(num=7, prime=prime)
>>> x1 = FieldElement(num=192, prime=prime)
>>> y1 = FieldElement(num=105, prime=prime)
>>> x2 = FieldElement(num=17, prime=prime)
>>> y2 = FieldElement(num=56, prime=prime)
>>> p1 = Point(x1, y1, a, b)
>>> p2 = Point(x2, y2, a, b)
>>> print(p1+p2)
Point(170,142)_0_7 FieldElement(223)
```

## Exercise 2

For the curve $y^2 = x^3 + 7$ over $F_{223}$, find:

- $(170,142) + (60,139)$
- $(47,71) + (17,56)$
- $(143,98) + (76,66)$

## Exercise 3

Extend `ECCTest` to test for the additions from the previous exercise. Call this `test_add`.

## Scalar Multiplication for Elliptic Curves

Because we can add a point to itself, we can introduce some new notation:

$(170,142) + (170,142) = 2 \cdot (170,142)$

Similarly, because we have associativity, we can actually add the point again:

$2 \cdot (170,142) + (170,142) = 3 \cdot (170, 142)$

We can do this as many times as we want. This is what we call *scalar multiplication*. That is, we have a *scalar* number in front of the point. We can do this because we have defined point addition and point addition is associative.

One property of scalar multiplication is that it’s really hard to predict without calculating (see Figure 3-4).

![](img/6c915536bcfd43e412f7adef942edf23_73_0.png)

Figure 3-4. Scalar multiplication results for $y^2 = x^3 + 7$ over $F_{223}$ for point $(170,142)$

Each point is labeled by how many times we’ve added the point. You can see that this is a complete scattershot. This is because point addition is nonlinear and not easy to calculate. Performing scalar multiplication is straightforward, but doing the opposite, point division, is not.

This is called the discrete log problem and is the basis of elliptic curve cryptography.

Another property of scalar multiplication is that at a certain multiple, we get to the point at infinity (remember, the point at infinity is the additive identity or 0). If we imagine a point G and scalar-multiply until we get the point at infinity, we end up with a set:

{ G, 2G, 3G, 4G, ... nG } where nG = 0

It turns out that this set is called a group, and because n is finite, we have a finite group (or more specifically, a finite cyclic group). Groups are interesting mathematically because they behave well with respect to addition:

G + 4G = 5G or aG + bG = (a + b)G

## Scalar Multiplication Redux

Scalar multiplication is adding the same point to itself some number of times. The key to making scalar multiplication into public key cryptography is using the fact that scalar multiplication on elliptic curves is very hard to reverse. Note the previous exercise. Most likely, you calculated the point $s \cdot (47,71)$ in $F_{223}$ for $s$ from 1 until 21. Here are the results:

```
>>> from ecc import FieldElement, Point
>>> prime = 223
>>> a = FieldElement(0, prime)
>>> b = FieldElement(7, prime)
>>> x = FieldElement(47, prime)
>>> y = FieldElement(71, prime)
>>> p = Point(x, y, a, b)
>>> for s in range(1,21):
...     result = s*p
...     print('{}*(47,71)=({},{}).format(s,result.x.num,result.y.num)')
1*(47,71)=(47,71)
2*(47,71)=(36,111)
3*(47,71)=(15,137)
4*(47,71)=(194,51)
5*(47,71)=(126,96)
6*(47,71)=(139,137)
7*(47,71)=(92,47)
8*(47,71)=(116,55)
9*(47,71)=(69,86)
10*(47,71)=(154,150)
11*(47,71)=(154,73)
12*(47,71)=(69,137)
13*(47,71)=(116,168)
14*(47,71)=(92,176)
15*(47,71)=(139,86)
16*(47,71)=(126,127)
17*(47,71)=(194,172)
18*(47,71)=(15,86)
19*(47,71)=(36,112)
20*(47,71)=(47,152)
```

If you look closely at the numbers, there’s no real discernible pattern to the scalar multiplication. The x coordinates don’t always increase or decrease, and neither do the y coordinates. About the only pattern is that between 10 and 11, the x coordinates are equal (10 and 11 have the same x, as do 9 and 12, 8 and 13, and so on). This is due to the fact that $21 \cdot (47,71) = 0$.
