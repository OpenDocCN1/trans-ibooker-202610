In earlier VB family members, the same can be accomplished without using the **Math** namespace:

```
Dim pi As Double = 3.14159
Dim roundedPi As Double = Round(pi)
```

In both VB.NET and earlier VB family members, you can also pass a second parameter – the number of fractional digits to round to, for example:

```
Dim pi As Double = 3.14159
Dim roundedPi As Double = Math.Round(pi,4)
```

This code will round to four decimal places, storing the value **3.1416** in the **roundedPi** variable.

+   - **Truncation:** You can truncate a numeric value using the **truncate** function. Truncation removes the remaining digits from the real number. In VB.NET, you want to call the **truncate** method in the **Math** namespace, for example:

```
Dim pi As Double = 3.14159
Dim trunPi As Double = Math.Truncate(pi)
```

The preceding code will store the value **3** in the **trunPi** variable.

In VBA, the same can be accomplished without using the **Fix** function without the **Math** namespace:

```
Dim pi As Double
Pi = 3.14159
Dim trunPi As Double
trunPi = Fix(pi)
```

VBScript and VB6 did not have a **truncate** function. There are other functions, such as **Fix** and **Int**, if you want to remove the decimal portion. A trick to using these functions while preserving digits to the right of the decimal is multiplying the number by a multiple of 10 and then dividing the result by the same number. Here is an example in **VBScript**:

```
Dim pi
pi = 3.14159
Dim trunPi
trunPi = Int(pi * 10000) / 10000
```

The preceding code will truncate to four decimal places, storing the value **3.1415** in the **trunPi** variable.

# Type conversion

Using casting operators, you can convert numeric values from one data type to another. Here is an example:

```
Dim n1 As Integer = 25
Dim n2 As Double = CDbl(n1)
```