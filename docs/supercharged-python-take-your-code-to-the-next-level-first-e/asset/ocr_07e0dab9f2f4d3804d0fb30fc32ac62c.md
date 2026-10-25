Specifies a minimum print-field width of 10. If the data is a string, 5 characters is a print-field maximum and anything larger is truncated. If the data is floating point, the field displays at most 5 digits total to the left and right of the decimal point; rounding is performed, but if the display size still exceeds the space allowed, the number is displayed in exponential format, such as `3+010e`. The precision field (5 in this case) is not valid for integers.

Same as above, but print-field width is 8 and precision is 4.

Specifies a minimum print-field width of 10 and precision of 7, (total number of digits to the left and right), **and** it displays thousands place separators.

Fixed-point display. Uses a print-field width of 10 and displays exactly 3 digits to the right of the decimal point. Rounding up or down, or putting in trailing zeros, is performed as needed to make the number of digits come out exactly right.

Uses a print-field width of 10 and displays exactly 5 digits to the right of the decimal point.

Displays exactly 3 digits to the right of the decimal point. There is no minimum width in this case.