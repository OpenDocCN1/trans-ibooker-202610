| `__ (s elf )` | |
| --- | --- |
| `__abs s_ __(s elf )` | Absolute value. This method is automatically invoked when the abs function is applied to an object of the class. |
| `__in ver t_ __(s elf )` | Bitwise inversion, which generates a result that contains 1 in every bit position in which the operand contains 0, and vice versa. This is called in response to the ~ operator. This is also called the bitwise complement or the one’s complement. |
| `__bo ol_ __(s elf )` | Convert value to a Boolean. This conversion is invoked not only by bool(), but also by logical operators such as not and control structures that respond to a condition.
The default action is to return True in response if this method is not defined. |
| `__ro und __(s elf ,` | Precision-rounding function. This method is called by formatting functions that specify a limited precision and by the round function.
The optional argument n specifies how many significant digits to round to. If omitted, the function should round to the nearest integer. |