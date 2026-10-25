# # 1.7 FUNCTION DEFINITIONS

Within the Python interactive development environment, you can more easily enter a program if you first enter it as a function definition, such as `main`. Then call that function. Python provides the `def` keyword for defining functions.

Click here to view code image

```
```
def main():
    side1 = float(input('Enter length of a side:
' ))
    side2 = float(input('Enter another length: '))
    hyp = (side1 * side1 + side2 * side2) ** 0.5
    print('Length of hypotenuse is: ', hyp)
```
```

Note that you must enter the first line as follows. The `def` keyword, parentheses, and colon (`:`) are strictly required.

```
```
def main():
```
```

If you enter this correctly from within IDLE, the environment automatically indents the next lines for you. Maintain this indentation. If you enter the function as part of a script, then you must choose an indentation scheme, and it must be consistent. Indentation of four spaces is recommended when you have a choice.

> > **Note**
> > Mixing tab characters with actual spaces can cause errors even though it might not look wrong. So be careful with tabs!