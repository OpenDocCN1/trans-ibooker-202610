If you use *pop* with no arguments, it removes and returns the last item in the list. In that sense, it is the opposite of *append*.

```
years.pop()
```

2012

```
years
```

[2021, 1983, 1976, 1997, 1990, 1961, 1980]

I cover lists and their operations in detail in section 3.2.

# 2.9 Printing

So far, we have only seen Python displaying the value of objects or evaluated expressions.

```
"This is a sample string"
```

'This is a sample string'

If you are displaying information to a consumer of your code, you need more sophisticated tools to generate text from data. The *print* function is a versatile way of combining strings and objects into readable forms.

In its simplest form, *print* takes a **str**, or an object that can be converted to a **str**, and displays it on the console. Python does not show the delimiting quotes.

```
print("This is a sample string")
```

This is a sample string

```
print(17 + 4)
```

21

You can print several objects at once, and Python separates them with spaces.

```
print(57, 99, -4.3)
```

57 99 -4.3