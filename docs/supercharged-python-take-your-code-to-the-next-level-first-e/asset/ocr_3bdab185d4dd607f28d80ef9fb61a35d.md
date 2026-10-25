![](bbox=[0.143, 0.088, 0.255, 0.168])

```
```
print(arguments)
```
```

Python 2.0 has a **print** statement that does the same thing but does not use parentheses.

The **print** function has some special arguments that can be entered by using the name.

+   - **sep**=_string_ specifies a separator string to be used instead of the default separator, which is one space. This can be an empty string if you choose: sep=' '.
+   - **end**=_string_ specifies what, if anything, to print after the last argument is printed. The default is a newline. If you don't want a newline to be printed, set this argument to an empty string or some other string, as in end=' '.

Given these elementary functions—**input** and **print**—you can create a Python script that’s a complete program. For example, you can enter the following statements into a text file and run it as a script.

> > Click here to view code image

```
```
side1 = float(input('Enter length of a side: '))
side2 = float(input('Enter another length: '))
hyp = ((side1 * side1) + (side2 * side2)) ** 0.5
print('Length of hypotenuse is:', hyp)
```
```