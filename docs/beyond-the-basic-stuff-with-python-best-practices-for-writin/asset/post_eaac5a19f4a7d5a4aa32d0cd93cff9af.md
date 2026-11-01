
the format() string method, or %s format specifiers in every case. The speed only significantly improves when you’re performing large numbers of string concatenations.

Python frees you from having to think about many underlying details. This allows programmers to write software quickly, and as mentioned earlier, programmer time is more valuable than CPU time. But there are cases when it’s good to understand details, such as the difference between immutable strings and mutable lists, to avoid tripping on a gotcha, like building strings through concatenation.

## Don't Expect sort() to Sort Alphabetically

Understanding sorting algorithms—algorithms that systematically arrange values by some established order—is an important foundation for a computer science education. But this isn’t a computer science book; we don’t need to know these algorithms, because we can just call Python’s sort() method. However, you’ll notice that sort() has some odd sorting behavior that puts a capital Z before a lowercase a:

```
>>> letters = ['z', 'A', 'a', 'Z']
>>> letters.sort()
>>> letters
['A', 'Z', 'a', 'z']
```

The American Standard Code for Information Interchange (ASCII, pronounced “ask-ee”) is a mapping between numeric codes (called code points or ordinals) and text characters. The sort() method uses ASCII-betical sorting (a general term meaning sorted by ordinal number) rather than alphabetical sorting. In the ASCII system, A is represented by code point 65, B by 66, and so on, up to Z by 90. The lowercase a is represented by code point 97, b by 98, and so on, up to z by 122. When sorting by ASCII, uppercase Z (code point 90) comes before lowercase a (code point 97).

Although it was almost universal in Western computing prior to and throughout the 1990s, ASCII is an American standard only: there’s a code point for the dollar sign, $ (code point 36), but there is no code point for the British pound sign, £. ASCII has largely been replaced by Unicode, because Unicode contains all of ASCII’s code points and more than 100,000 other code points.

You can get the code point, or ordinal, of a character by passing it to the ord() function. You can do the reverse by passing an ordinal integer to the chr() function, which returns a string of the character. For example, enter the following into the interactive shell:

```
>>> ord('a')
97
>>> chr(97)
'a'
```

If you want to make an alphabetical sort, pass the str.lower method to the key parameter. This sorts the list as if the values had the lower() string method called on them:

```
>>> letters = ['z', 'A', 'a', 'Z']
>>> letters.sort(key=str.lower)
>>> letters
['A', 'a', 'z', 'Z']
```

Note that the actual strings in the list aren't converted to lowercase; they're only sorted as if they were. Ned Batchelder provides more information about Unicode and code points in his talk “Pragmatic Unicode, or, How Do I Stop the Pain?” at https://nedbatchelder.com/text/unipain.html.

Incidentally, the sorting algorithm that Python's sort() method uses is Timsort, which was designed by Python core developer and “Zen of Python” author Tim Peters. It's a hybrid of the merge sort and insertion sort algorithms, and is described at https://en.wikipedia.org/wiki/Timsort.

## Don't Assume Floating-Point Numbers Are Perfectly Accurate

Computers can only store the digits of the binary number system, which are 1 and 0. To represent the decimal numbers we're familiar with, we need to translate a number like 3.14 into a series of binary ones and zeros. Computers do this according to the IEEE 754 standard, published by the Institute of Electrical and Electronics Engineers (IEEE, pronounced “eye-triple-ee”). For simplicity, these details are hidden from the programmer, allowing you to type numbers with decimal points and ignore the decimal-to-binary conversion process:

```
>>> 0.3
0.3
```

Although the details of specific cases are beyond the scope of this book, the IEEE 754 representation of a floating-point number won't always exactly match the decimal number. One well-known example is 0.1:

```
>>> 0.1 + 0.1 + 0.1
0.30000000000000004
>>> 0.3 == (0.1 + 0.1 + 0.1)
False
```

This bizarre, slightly inaccurate sum is the result of rounding errors caused by how computers represent and process floating-point numbers. This isn't a Python gotcha; the IEEE 754 standard is a hardware standard implemented directly into a CPU's floating-point circuits. You'll get the same results in C++, JavaScript, and every other language that runs on a CPU that uses IEEE 754 (which is effectively every CPU in the world).

The IEEE 754 standard, again for technical reasons beyond the scope of this book, also cannot represent all whole number values greater than 2^53. For example, 2^53 and 2^53 + 1, as float values, both round to 9007199254740992.0:

```
>>> float(2**53) == float(2**53) + 1
True
```

As long as you use the floating-point data type, there’s no workaround for these rounding errors. But don’t worry. Unless you’re writing software for a bank, a nuclear reactor, or a bank’s nuclear reactor, rounding errors are small enough that they’ll likely not be an important issue for your program. Often, you can resolve them by using integers with smaller denominations: for example, 133 cents instead of 1.33 dollars or 200 milliseconds instead of 0.2 seconds. This way, 10 + 10 + 10 adds up to 30 cents or milliseconds rather than 0.1 + 0.1 + 0.1 adding up to 0.30000000000000004 dollars or seconds.

But if you need exact precision, say for scientific or financial calculations, use Python’s built-in decimal module, which is documented at https://docs.python.org/3/library/decimal.html. Although they’re slower, Decimal objects are precise replacements for float values. For example, decimal.Decimal('0.1') creates an object that represents the exact number 0.1 without the imprecision that a 0.1 float value would have.

Passing the float value 0.1 to decimal.Decimal() creates a Decimal object that has the same imprecision as a float value, which is why the resulting Decimal object isn’t exactly Decimal('0.1'). Instead, pass a string of the float value to decimal.Decimal(). To illustrate this point, enter the following into the interactive shell:

```
>>> import decimal
>>> d = decimal.Decimal(0.1)
>>> d
Decimal('0.1000000000000000055511151231257827021181583404541015625')
>>> d = decimal.Decimal('0.1')
>>> d
Decimal('0.1')
>>> d + d + d
Decimal('0.3')
```

Integers don’t have rounding errors, so it’s always safe to pass them to decimal.Decimal(). Enter the following into the interactive shell:

```
>>> 10 + d
Decimal('10.1')
>>> d * 3
Decimal('0.3')
>>> 1 - d
Decimal('0.9')
>>> d + 0.1
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'decimal.Decimal' and 'float'
```

But Decimal objects don’t have unlimited precision; they simply have a predictable, well-established level of precision. For example, consider the following operations:

```
>>> import decimal
>>> d = decimal.Decimal(1) / 3
>>> d
Decimal('0.333333333333333333333333333')
>>> d * 3
Decimal('0.999999999999999999999999999')
>>> (d * 3) == 1 # d is not exactly 1/3
False
```

The expression decimal.Decimal(1) / 3 evaluates to a value that isn’t exactly one-third. But by default, it’ll be precise to 28 significant digits. You can find out how many significant digits the decimal module uses by accessing the decimal.getcontext().prec attribute. (Technically, prec is an attribute of the Context object returned by getcontext(), but it’s convenient to put it on one line.) You can change this attribute so that all Decimal objects created afterward use this new level of precision. The following interactive shell example lowers the precision from the original 28 significant digits to 2:

```
>>> import decimal
>>> decimal.getcontext().prec
28
>>> decimal.getcontext().prec = 2
>>> decimal.Decimal(1) / 3
Decimal('0.33')
```

The decimal module provides you with fine control over how numbers interact with each other. The decimal module is documented in full at https://docs.python.org/3/library/decimal.html.

## Don't Chain Inequality != Operators

Chaining comparison operators like 18 < age < 35 or chaining assignment operators like six = halfDozen = 6 are handy shortcuts for (18 < age) and (age < 35) and six = 6; halfDozen = 6, respectively.

But don’t chain the != comparison operator. You might think the following code checks whether all three variables have different values from each other, because the following expression evaluates to True:

```
>>> a = 'cat'
>>> b = 'dog'
>>> c = 'moose'
>>> a != b != c
True
```
