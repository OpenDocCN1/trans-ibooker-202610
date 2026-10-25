| 'b | Uses binary radix. |
| ' | |
| '6 | Uses binary radix; right justifies numbers within a field of 6 |
| b' | characters. |
| 'x | Uses hexadecimal radix. |
| ' | |
| '5 | Uses hexadecimal radix; right justifies numbers within a field of 5 |
| x' | characters. |
| 'o | Uses octal radix. |
| ' | |
| '5 | Uses octal radix; right justifies numbers within a field of 5 |
| o' | characters. |

The remainder of this section discusses the features in more detail, particularly width and precision fields.
The thousands place separator is fairly self-explanatory but works only with numbers. Python raises an exception if this specifier is used with data that isn’t numeric.
You might use it to format a large number such as 150 million.