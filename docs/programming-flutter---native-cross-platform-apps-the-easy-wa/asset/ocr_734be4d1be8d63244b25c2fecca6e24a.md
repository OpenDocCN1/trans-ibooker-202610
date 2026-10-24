```
Text(number)
```

because Text expects a String. This means you need to change that to:

```
Text(number.toString())
```

What if you want to display a string and the number?
Then you would need to write something like this, using string concatenation:

```
"You tapped the button" + number.toString() + "times"
```

What string interpolation allows you to do is write this instead:

```
"You tapped the button $number times"
```

By prefixing a variable’s name with $ you can insert its value into the string. If you need to access an element of a List, the member of a class, or write any kind of expression, you need to enclose the statement in curly braces. If you had a list like the following:

```
List numbers = [1, 2, 3];
```

you would use string interpolation like this:

```
"There are ${numbers.length} and second element is number ${numbers[1]}"
```

# Multi-Line Strings and Raw Strings

Like in many other languages, you can make multi-line strings by using triple quotes and you can make raw strings by prefixing the opening quotes with the letter “r”.

A multi-line string makes it so that you can turn this:

```
"First line\nSecond line";
```

or this:

```
"First line\n" +
```