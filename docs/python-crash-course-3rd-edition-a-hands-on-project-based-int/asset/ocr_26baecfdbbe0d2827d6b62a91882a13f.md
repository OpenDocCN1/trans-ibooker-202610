confusing for users to read, users will see friendly error messages that you’ve written.

# Handling the ZeroDivisionError Exception

Let’s look at a simple error that causes Python to raise an exception. You probably know that it’s impossible to divide a number by zero, but let’s ask Python to do it anyway:

division_calculator.py

```
print(5/0)
```

Python can’t do this, so we get a traceback:

```
Traceback (most recent call last):
    File "division_calculator.py", line 1, in <module>
        print(5/0)
          ^^
ZeroDivisionError: division by zero
```

The error reported in the traceback, ZeroDivisionError, is an exception object ①. Python creates this kind of object in response to a situation where it can’t do what we ask it to. When this happens, Python stops the program and tells us the kind of exception that was raised. We can use this information to modify our program. We’ll tell Python what to do when this kind of exception occurs; that way, if it happens again, we’ll be prepared.

# Using try-except Blocks

When you think an error may occur, you can write a try-except block to handle the exception that might be raised. You tell Python to try running some code, and you tell it what to do if the code results in a particular kind of exception.