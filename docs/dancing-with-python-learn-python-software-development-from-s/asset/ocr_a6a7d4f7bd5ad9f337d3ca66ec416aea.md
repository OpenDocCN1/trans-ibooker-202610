It is as simple as adding or deleting the @decorator line to get or remove the extra functionality.

# Exercise 10.7

Write a decorator function assert_first_arg_is_int that inserts an assert statement that checks whether the first argument passed to a function is an int.

Although I coded return func(*args, **kwargs), I could have saved the result in a variable, done more work in the wrapper function, and then returned the value.

```
def enter_and_exit(func):
    def wrapper(*args, **kwargs):
        print(f">>> Entering {func.__name__}")
        result = func(*args, **kwargs)
        print(f">>> Exiting {func.__name__} with {result}")
        return result
    return wrapper

@enter_and_exit
def adder_with_carry(a, b, carry=0):
    return a + b + carry

adder_with_carry(10, 20, carry=1)
```

>>> Entering adder_with_carry
>>> Exiting adder_with_carry with 31
31

# Exercise 10.8

Extend the last definition of enter_and_exit to print the argument values in the "Entering ..." message.

# Exercise 10.9

Use one or more functions from the time module to create a decorator that displays how long it takes to execute func within wrapper.