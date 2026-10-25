Because the argument to be printed ('Hey Bill G, pick me! ') is 20 characters in length but the print-field width is 24 characters, four copies of the fill character, a minus sign in this case, are used for padding.

The fill character can be any character other than a curly brace. Note that if you want to pad a number with zeros, you can alternatively use the '0' specifier described in Section 5.8.4, “The Leading Zero Character (0).”

The _align_ character must be one of the four values listed in Table 5.3.

# Table 5.3. “Align” Characters Used in Formatting

| Align character | Meaning |
|---|---|
| < | Left justify. This is the default for string data. |
| > | Right justify. This is the default for numbers. |
| ^ | Center the text in the middle of the print field. (This slightly favors left justification when the text can’t be centered perfectly.) |
| = | Place all padding characters between the sign character (+ or –) and the number to be printed. This specification is valid only for numeric data. |