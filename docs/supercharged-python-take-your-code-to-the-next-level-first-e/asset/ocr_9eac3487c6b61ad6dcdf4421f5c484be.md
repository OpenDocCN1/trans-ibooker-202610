The items here are mostly independent of each other. Python interprets each item according to placement and context. For example, _prec_ (precision) appears right after a decimal point (.) if it appears at all.

When looking at the examples, remember that curly braces and colons are used only when you use _spec_ with the global **format** function and not the **format** method. With the **format** function, you might include _align_, _sign_, `0`, _width_, _precision_, and _type_ specifiers, but no curly braces or colon. Here’s an example:

```
s = format(32.3, '<+08.3f')
```

# ## 5.8.1 Print-Field Width

One of the commonly used items is print-field _width_, specified as an integer. The text to be printed is displayed in a field of this size. If the text is shorter than this width, it’s justified and extra spaces are padded with blank spaces by default.

**Placement**: As you can see from the syntax display, the _width_ item is in the middle of the _spec_ syntax. When used with the **format** method, _width_ always follows a colon (:), as does the rest of the _spec_ syntax.

The following example shows how width specification works on two numbers: 777 and 999. The example uses asterisks (*) to help illustrate where the print fields begin and end, but otherwise these asterisks are just literal characters thrown in for the sake of illustration.