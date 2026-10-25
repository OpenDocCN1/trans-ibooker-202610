| L | unsigned long | int | 4 |
|---|---------------|-----|---|
| q | long long     | int | 8 |
| Q | unsigned long | int | 8 |
| f | float         | float | 4 |
| d | double        | float | 8 |
| ints | char[]      | str | int length |
| P | Pascal string type; see online help for more information. | | |

Table 8.2 lists C-language data types in the second column.
Many other languages usually have a concept of short and long
integers and short and long floating-point numbers that
correspond to these types. (Python integers, however, have to
be “packed,” as shown in this section.)

> > **Note**
> >
> > The integer prefix can be applied to fields other than strings. For example, ‘3f’
> > means the same as ‘fff’.

To write to a binary file using the **struct** package, follow