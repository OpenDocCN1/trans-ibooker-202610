subtraction, and so on, are all supported and can be smoothly combined with integers.

Click here to view code image

```
>>> fr1 = Fraction(1, 100)
>>> print(fr1, 'times 50 =', fr1 * 50)
1/100 times 50 = 1/2
```

Considering that you can initialize a **Fraction** object from a string specifying a floating-point expression, such as `'0.1'`, can you initialize from a string such as `'1/7'`, which is what we’d really like to do?

Yes. This is especially convenient, and we’ll use it in the upcoming application.

```
>>> fr1 = Fraction('1/7')
>>> print(fr1)
1/7
>>> fr1 += Fraction('3/4')
>>> print(fr1)
25/28
```

This conversion works only as long as there are no intervening spaces between the numerator, the forward slash (/), and the denominator. Few users are foolish enough to put in extra spaces. But if you’re worried about the user doing that, you can always eliminate spaces in this way:

```
s = s.replace(' ', '')
```

Finally, you can always access the **numerator** and