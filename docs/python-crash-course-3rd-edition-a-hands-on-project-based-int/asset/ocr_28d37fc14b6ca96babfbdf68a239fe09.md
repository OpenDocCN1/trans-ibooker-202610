In this chapter you’ll also learn a variety of ways to pass information to functions. You’ll learn how to write certain functions whose primary job is to display information and other functions designed to process data and return a value or set of values. Finally, you’ll learn to store functions in separate files called *modules* to help organize your main program files.

# Defining a Function

Here’s a simple function named `greet_user()` that prints a greeting:

```
greeter.py

```python
def greet_user():
    """Display a simple greeting."""
    print("Hello!")


greet_user()
```
```

This example shows the simplest structure of a function. The first line uses the keyword `def` to inform Python that you’re defining a function. This is the *function definition*, which tells Python the name of the function and, if applicable, what kind of information the function needs to do its job. The parentheses hold that information. In this case, the name of the function is `greet_user()`, and it needs no information to do its job, so its parentheses are empty. (Even so, the parentheses are required.) Finally, the definition ends in a colon.

Any indented lines that follow `def greet_user():` make up the *body* of the function. The text on the second line is a comment called a *docstring*, which describes what the function does. When Python generates documentation for the functions in your programs, it looks for a string immediately after the function's definition. These strings are