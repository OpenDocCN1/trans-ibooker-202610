```
fr1 = Fraction(1, 2)
fr2 = Fraction(2, 4)
fr3 = Fraction(100/200)
print('The fractions are %s, %s, & %s.' % (fr1, fr2,
fr3))
```

This example prints

+   Click here to view code image

```
The fractions are 1/2, 1/2, & 1/2.
```

All these **Fraction** objects are displayed as the same quantity, because they’re automatically reduced to their simplest form.

+   Click here to view code image

```
>>> if fr1 == fr2 and fr2 == fr3:
    print('They are all equal!')
They are all equal!
```

> Note
> By using one of the shortcuts pointed out in Chapter 4, you can replace the condition in this example by chaining the comparisons, producing a shorter version.
> Click here to view code image
> >>> if fr1 == fr2 == fr3:
>     print('They are all equal!')

Fractions can be specified in other ways. For example, if only