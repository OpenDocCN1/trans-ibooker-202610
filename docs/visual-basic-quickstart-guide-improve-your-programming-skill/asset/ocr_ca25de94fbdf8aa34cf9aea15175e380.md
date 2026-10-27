The preceding code will declare two integer variables (n1 and n2) and add them together, storing their result in the third variable named n3.

+   - **Subtraction:** You can subtract values using the - operator. Here is an example:

```
Dim n1 As Integer = 4
Dim n2 As Integer = 2
Dim n3 As Integer = n1 - n2
```

The preceding code will declare two integer variables (n1 and n2) and subtract n2 from n1, storing the result in the third variable named n3.

+   - **Multiplication:** You can multiply values using the * operator. Here is an example:

```
Dim n1 As Integer = 4
Dim n2 As Integer = 2
Dim n3 As Integer = n1 * n2
```

This code will declare two integer variables (n1 and n2) and then multiply them together, storing their result in the third variable named n3.

+   - **Division:** You can divide values using the / operator. The result is a real number. Here is an example:

```
Dim n1 As Integer = 5
Dim n2 As Integer = 2
Dim n3 As Double = n1 + n2
```

The preceding code will declare two integer variables (n1 and n2) and divide them, storing their result in the third variable named n3 of type Double.

+   - **Modular division:** We often want the remainder of the division's operation in computer science. This operation is called modular division. You can perform modular divisions using the Mod operator. Here is an example:

```
Dim n1 As Integer = 5
Dim n2 As Integer = 2
Dim n3 As Integer = n1 Mod n2
```

The preceding code will declare two integer variables (n1 and n2) and then divide them, storing the remainder of one in the third variable named n3.

# Rounding and truncation

There are several functions that allow you to get a decimal value with less precision:

+   - **Rounding:** Using the Round function, you can round a numeric value to the nearest integer. In VB.NET, you want to call the Round method in the Math namespace, for example:

```
Dim pi As Double = 3.14159
Dim roundedPi As Double = Math.Round(pi)
```

The preceding code will store the value 3 in the roundedPi variable.