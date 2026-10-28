one_hundred is a function and a first-class Python object. I want to print the current time whenever one_hundred runs. I could just change the function:

```
import datetime

def one_hundred():
    now = datetime.datetime.now()
    print(f"The time is {now.strftime('%H:%M:%S')}")
    return 100
```

```
one_hundred()
```

The time is 12:48:28
100

A decorator simplifies this process considerably, and I do not have to alter the original function body. I reset one_hundred to its original definition and create say_the_time.

```
def one_hundred():
    return 100
```

```
def say_the_time(func):
    def wrapper():
        now = datetime.datetime.now().strftime("%H:%M:%S")
        print(f"The time is {now}")
        return func()
    return wrapper
```

say_the_time takes a single argument func, which is a function with no arguments. It defines an inner function wrapper, which prints the time, evaluates func(), and returns the result. say_the_time then returns the function wrapper. Let’s see it in action for one_hundred:

```
h = say_the_time(one_hundred)
```

```
h
```

<function __main__.say_the_time.<locals>.wrapper()>

```
h()
```

The time is 12:48:28
100