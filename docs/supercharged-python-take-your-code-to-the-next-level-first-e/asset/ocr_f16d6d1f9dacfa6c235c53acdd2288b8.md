When you’re dealing with business applications, particularly
in the area of banking, it would be useful to be able to store a
number like 44.31 precisely, with no errors of any kind.
Fortunately, Python provides a class that solves this problem:
the `Decimal` class.

# 10.3 INTRODUCING THE DECIMAL CLASS

From within IDLE, execute the following `import` statement.

![](bbox=[0.195, 0.459, 0.842, 0.515])

We can now define any number of instances of the `Decimal`
class, which—like the floating-point class, `float`—can hold
fractional portions.

![](bbox=[0.195, 0.662, 0.842, 0.751])

As you can see, the default value of a `Decimal` instance is
zero (0). But you can assign any decimal value you like, and it’s
stored precisely.