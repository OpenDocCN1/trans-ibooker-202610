```
import math

math.e
```

2.718281828459045

```
math.factorial(10)
```

3628800

Once you do the import, you can use any module constant or function in this way. If I define my version of factorial:

```
def factorial(n):
    print("Using my factorial")

    if n < 2:
        return 1

    return n * factorial(n - 1)
```

I now have both functions available to me:

```
factorial(5)
```

Using my factorial
Using my factorial
Using my factorial
Using my factorial
Using my factorial
120

```
math.factorial(5)
```

120

In this way, I have avoided having one name cover the other. We also say that we do not have a namespace collision. This is good practice: you use what you need, and you know its source. I put in the call to print in my function so we can tell when Python is running this specific code.