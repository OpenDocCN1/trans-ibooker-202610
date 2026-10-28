We first import the `datetime` class from the `datetime` module. Then we call the method `strptime()` with the string containing the date we want to process as its first argument. The second argument tells Python how the date is formatted. In this example, `'%Y-'` tells Python to look for a four-digit year before the first dash; `'%m-'` indicates a two-digit month before the second dash; and `'%d'` means the last part of the string is the day of the month, from 1 to 31.

The `strptime()` method can take a variety of arguments to determine how to interpret the date. Table 16-1 shows some of these arguments.

**Table 16-1**: *Date and Time Formatting Arguments from the `datetime` Module*

| Argument | Meaning |
|---|---|
| %A | Weekday name, such as Monday |
| %B | Month name, such as January |
| %m | Month, as a number (01 to 12) |
| %d | Day of the month, as a number (01 to 31) |
| %Y | Four-digit year, such as 2019 |
| %y | Two-digit year, such as 19 |
| %H | Hour, in 24-hour format (00 to 23) |
| %I | Hour, in 12-hour format (01 to 12) |
| %p | AM or PM |
| %M | Minutes (00 to 59) |
| %S | Seconds (00 to 61) |

# Plotting Dates

We can improve our plot by extracting dates for the daily high temperature readings, and using these dates on the x-axis:

**sitka_highs.py**

```
from pathlib import Path
import csv
from datetime import datetime
```