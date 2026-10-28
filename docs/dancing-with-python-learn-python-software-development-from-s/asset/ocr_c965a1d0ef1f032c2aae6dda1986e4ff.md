Python stores a fraction in the lowest terms, which means the numerator and denominator have no common prime factors. Another way to say this is that the numerator and denominator have a greatest common divisor equal to 1.

```
f.numerator
```

2

```
f.denominator
```

3

```
```python
import math

math.gcd(f.numerator, f.denominator)
```
```

1

Look carefully at the last expression and note that I wrote `f.numerator` instead of `f.numerator()`. Instead of calling *numerator* as a method, I asked for the “numerator property” of the fraction. A *property* is an intrinsic part or characteristic of an object. We look at how to define properties within classes in [section 7.5](#).

Python normalizes a negative fraction so its numerator is negative.

```
Fraction(1, -5)
```

Fraction(-1, 5)

```
- Fraction(3, 7)
```

Fraction(-3, 7)

**Fraction** supports the usual algebraic operators.

```
a = Fraction(2, 3)
b = Fraction(7, 4)

2 * a
```

Fraction(4, 3)

```
a + b
```

Fraction(29, 12)