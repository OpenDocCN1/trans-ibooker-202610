Most of this book focuses on two kinds of numeric data: integer and floating point. These data types are sufficient for many applications. As you’ll see, however, they are not perfect. Integers have the obvious drawback of not holding fractions, and floating-point data has rounding errors, as we’ll show in this chapter.

This chapter introduces other data formats, including one that we’ll develop ourselves, using the principles of Chapter 9, “Classes and Magic Methods.”

+   - The `Decimal` class, which is a “fixed-point” data type that can hold decimal fractions, such as 0.02, precisely and without error.
+   - The `Money` class, which you can download or develop yourself. For the sake of illustration, this chapter takes the latter approach: developing this class ourselves.
+   - The `Fraction` class, which can store fractions such as one-third or one-seventh precisely and without any rounding errors, something that is not possible with the other classes.
+   - The `complex` class, which represents complex numbers from the world of higher math. Such numbers have both a “real” and an “imaginary” part.

If you’re not familiar with the use of complex numbers from higher mathematics, don’t worry. You can safely ignore these numbers unless you’re doing the sort of work that requires it. If you’re one of these people, you already know it.

None of these classes requires you to download anything from the Internet, and the `complex` class doesn’t even require anything to be imported. It’s a built-in class, just as `int`,