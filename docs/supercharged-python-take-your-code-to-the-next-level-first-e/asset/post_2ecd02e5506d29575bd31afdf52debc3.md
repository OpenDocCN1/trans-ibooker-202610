

The values returned in `quot` and `rem`, in this case, will be 2 and 3 after execution. This means that 10 divides into 23 two times and leaves a remainder of 3.

## 1.6 BASIC INPUT AND OUTPUT

Earlier, in Section 1.1, we promised to show how to prompt the user for the values used as inputs to a formula. Now we’re going to make good on that promise. (You didn’t think we were lying, did you?)

The Python `input` function is an easy-to-use input mechanism that includes an optional prompt. The text typed by the user is returned as a string.

![](img/6e45221a8147f2442c653a3d4a866570_50_0.png)

In Python 2.0, the `input` function works differently: it instead evaluates the string entered as if it were a Python statement. To achieve the same result as the Python 3.0 `input` statement, use the `raw_input` function in Python 2.0.

The `input` function prints the prompt string, if specified; then it returns the string the user entered. The input string is returned as soon as the user presses the Enter key; but no newline is appended.

![](img/6e45221a8147f2442c653a3d4a866570_51_0.png)

```
input(prompt_string)
```

To store the string returned as a number, you need to convert to integer (**int**) or floating-point (**float**) format. For example, to get an integer use this code:

> Click here to view code image

```
n = int(input('Enter integer here: '))
```

Or use this to get a floating-point number:

> Click here to view code image

```
x = float(input('Enter floating pt value here: '))
```

The prompt is printed without an added space, so you typically need to provide that space yourself.

Why is an **int** or **float** conversion necessary? Remember that they are necessary when you want to get a number. When you get any input by using the **input** function, you get back a string, such as “5.” Such a string is fine for many purposes, but you cannot perform arithmetic on it without performing the conversion first.

Python 3.0 also supports a **print** function that—in its simplest form—prints all its arguments in the order given, putting a space between each.

![](img/6e45221a8147f2442c653a3d4a866570_52_0.png)

```
print(arguments)
```

Python 2.0 has a **print** statement that does the same thing but does not use parentheses.

The **print** function has some special arguments that can be entered by using the name.

- **sep**=_string_ specifies a separator string to be used instead of the default separator, which is one space. This can be an empty string if you choose: sep=' '.
- **end**=_string_ specifies what, if anything, to print after the last argument is printed. The default is a newline. If you don't want a newline to be printed, set this argument to an empty string or some other string, as in end=' '.

Given these elementary functions—**input** and **print**—you can create a Python script that’s a complete program. For example, you can enter the following statements into a text file and run it as a script.

> Click here to view code image

```
side1 = float(input('Enter length of a side: '))
side2 = float(input('Enter another length: '))
hyp = ((side1 * side1) + (side2 * side2)) ** 0.5
print('Length of hypotenuse is:', hyp)
```

## 1.7 FUNCTION DEFINITIONS

Within the Python interactive development environment, you can more easily enter a program if you first enter it as a function definition, such as `main`. Then call that function. Python provides the `def` keyword for defining functions.

Click here to view code image

```
def main():
    side1 = float(input('Enter length of a side:
' ))
    side2 = float(input('Enter another length: '))
    hyp = (side1 * side1 + side2 * side2) ** 0.5
    print('Length of hypotenuse is: ', hyp)
```

Note that you must enter the first line as follows. The `def` keyword, parentheses, and colon (`:`) are strictly required.

```
def main():
```

If you enter this correctly from within IDLE, the environment automatically indents the next lines for you. Maintain this indentation. If you enter the function as part of a script, then you must choose an indentation scheme, and it must be consistent. Indentation of four spaces is recommended when you have a choice.

> **Note**
> Mixing tab characters with actual spaces can cause errors even though it might not look wrong. So be careful with tabs!

Because there is no “begin block” and “end block” syntax,
Python relies on indentation to know where statement blocks
begin and end. The critical rule is this:

> *
> Within any given block of code, the indentation of all statements (that is, at the
> same level of nesting) must be the same.

For example, the following block is invalid and needs to be
revised.

Click here to view code image

```
def main():
    side1 = float(input('Enter length of a side:
'))
    side2 = float(input('Enter another length:
'))
    hyp = (side1 * side1 + side2 * side2) ** 0.5
    print('Length of hypotenuse is: ', hyp)
```

If you have a nested block inside a nested block, the
indentation of each level must be consistent. Here’s an
example:

Click here to view code image

```
def main():
    age = int(input('Enter your age: '))
    name = input('Enter your name: ')
    if age < 30:
        print('Hello', name)
        print('I see you are less than 30.')
        print('You are so young.')
```

The first three statements inside this function definition are all at the same level of nesting; the last three statements are at a deeper level. But each is consistent.

Even though we haven’t gotten to the **if** statement yet (we’re just about to), you should be able to see that the flow of control in the next example is different from the previous example.

Click here to view code image

```
def main():
    age = int(input('Enter your age: '))
    name = input('Enter your name: ')
    if age < 30:
        print('Hello', name)
print('I see you are less than 30.')
print('You are so young.')
```

Hopefully you can see the difference: In _this_ version of the function, the last two lines do not depend on your age being less than 30. That’s because Python uses indentation to determine the flow of control.

Because the last two statements make sense only if the age is less than 30, it’s reasonable to conclude that this version has a bug. The correction would be to indent the last two statements so that they line up with the first **print** statement.

After a function is defined, you can call that function—which means to make it execute—by using the function name, followed by parentheses. (If you _don’t_ include the parentheses, you will not successfully execute the function!)

```
main()
```

So let’s review. To define a function, which means to create a kind of miniprogram unto itself, you enter the def statement and keep entering lines in the function until you’re done—after which, enter a blank line. Then run the function by typing its name followed by parentheses. Once a function is defined, you can execute it as often as you want.

So the following sample session, in the IDLE environment, shows the process of defining a function and calling it twice. For clarity, user input is in bold.

![](img/6e45221a8147f2442c653a3d4a866570_56_0.png)

```
>>> def main():
    side1 = float(input('Enter length of a side:
'))
    side2 = float(input('Enter another length: '))
    hyp = (side1 * side1 + side2 * side2) ** 0.5
    print('Length of hypotenuse is: ', hyp)
>>> main()
Enter length of a side: 3
Enter another length: 4
Length of hypotenuse is: 5.0
>>> main()
Enter length of a side: 30
Enter another length: 40
Length of hypotenuse is: 50.0
```

As you can see, once a function is defined, you can call it (causing it to execute) as many times as you like.

The Python philosophy is this: Because you should do this indentation anyway, why shouldn’t Python rely on the indentation and thereby save you the extra work of putting in

