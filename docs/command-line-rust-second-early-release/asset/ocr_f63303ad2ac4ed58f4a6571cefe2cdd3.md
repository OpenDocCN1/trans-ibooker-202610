number, a dash
(' - '), and a second number and select the fields or
columns from the
first number to the second, inclusive. Numbers or
number ranges may be
preceded by a dash, which selects all fields or
columns from 1 to the
last number. Numbers or number ranges may be followed
by a dash, which
selects all fields or columns from the last number to
the end of the
line. Numbers and number ranges may be repeated,
overlapping, and in any
order. If a field or column is specified multiple
times, it will appear
only once in the output. It is not an error to select
fields or columns
not present in the input line.

The options are as follows:

-b list
    The list specifies byte positions.

-c list
    The list specifies character positions.

-d delim
    Use delim as the field delimiter character
instead of the tab
    character.

-f list
    The list specifies fields, separated in the
input by the field
    delimiter character (see the -d option.)
Output fields are sepa-
    rated by a single occurrence of the field
delimiter character.

-n      Do not split multi-byte characters.
Characters will only be out-
    put if at least one byte is selected, and,
after a prefix of zero
    or more unselected bytes, the rest of the