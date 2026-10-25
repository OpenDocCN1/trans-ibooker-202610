| __floor__(self) | Round-down function. The effect should be to round those values down to the greatest integer that is not higher in value than the object. |
|---|---|
| | This method is called by the math.floor function, part of the math package. |
| __ceil__(self) | Round-upward function. The effect should be to round those values up to the lowest integer that is not lower in value than the object. |
| | This method is called by the math.ceil function, part of the math package. |
| __trunc__(self) | Round-downward method. This method is similar to __floor__, but instead of rounding up or down, it merely truncates the fractional portion of a floating-point value or values. For example, -3.5 is rounded up to -3.0, but 3.5 is rounded down to 3.0. |
| | This method is called by the math.trunc function, part of the math package. |

Like the binary methods, these unary methods expect an implementation that creates a new object (that is, an instance) and returns it. This object should generally be of the same type as the object being operated on.

The Point class, introduced earlier, is a mathematical object