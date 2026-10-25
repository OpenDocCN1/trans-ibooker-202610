```
print('results>{: },{:+},{:-}'.format(25, 25, 25))
```

This example prints

```
results> 25,+25,25
```

Notice how there’s an extra space in front of the first occurrence of 25, even though it’s nonnegative; however, if the print fields had definite widths assigned—which they do not in this case—that character would produce no difference.

This next example applies the same formatting to three negative values (−25).

Click here to view code image

```
print('results>{: },{:+},{:-}'.format(-25, -25,
-25))
```

This example prints the following output, illustrating that negative numbers are always printed with a minus sign.

```
results>-25,-25,-25
```

# ## 5.8.4 The Leading-Zero Character (0)

This character specifies padding a 0 digit character for numbers, causing a “o” to be used instead of spaces. Although you can achieve similar effects by specifying align and fill characters, this technique is slightly less verbose.

**Placement:** This character, if used, immediately precedes