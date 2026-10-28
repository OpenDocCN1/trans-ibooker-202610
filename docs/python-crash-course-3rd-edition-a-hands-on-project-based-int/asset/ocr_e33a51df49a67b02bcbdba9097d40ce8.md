# Simple if Statements

The simplest kind of `if` statement has one test and one action:

```
if conditional_test:
    do something
```

You can put any conditional test in the first line and just about any action in the indented block following the test. If the conditional test evaluates to True, Python executes the code following the if statement. If the test evaluates to False, Python ignores the code following the if statement.

Let’s say we have a variable representing a person’s age, and we want to know if that person is old enough to vote. The following code tests whether the person can vote:

# voting.py

```
age = 19
if age >= 18:
    print("You are old enough to vote!")
```

Python checks to see whether the value of age is greater than or equal to 18. It is, so Python executes the indented `print()` call:

```
You are old enough to vote!
```

Indentation plays the same role in `if` statements as it did in for loops. All indented lines after an `if` statement will be executed if the test passes, and the entire block of indented lines will be ignored if the test does not pass.

You can have as many lines of code as you want in the block following the `if` statement. Let’s add another line of output if the person is old enough to vote, asking if the individual has registered to vote yet: