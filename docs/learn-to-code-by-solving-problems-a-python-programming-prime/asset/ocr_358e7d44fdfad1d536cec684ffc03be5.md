then converting that value to an integer:

```
>>> num = input()
82
>>> num = int(num)
>>> num
82
```

Or we can combine the input and int calls:

```
>>> num = int(input())
82
>>> num
82
```

Here, the argument passed to int is the string returned by input. The int function takes this string and returns it as an integer.

If we ever need to convert the other way, from an integer to a string, we can do that with the str function:

```
>>> num = 82
>>> 'my number is ' + num
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
>>> str(num)
'82'
>>> 'my number is ' + str(num)
'my number is 82'
```

We can’t concatenate a string and an integer. The str function returns '82' from 82 so that it can be used in a string concatenation.

# Solving the Problem

We’re ready to solve Cone Volume. Create a text file called cone_volume.py and type the code in Listing 1-2.

```
① PI = 3.141592653589793

② radius = int(input())
③ height = int(input())
```