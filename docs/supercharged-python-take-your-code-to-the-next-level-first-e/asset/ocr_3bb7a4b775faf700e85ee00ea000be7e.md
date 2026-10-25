typically isn’t used in the program code. Presumably you already know what the name of your program is, so you don’t need to look it up.

Of course, from within the program you can’t always be sure that argument values were specified on the command line. If they were not specified, you may want to provide an alternative, such as prompting the user for these same values.

Remember that the length of the argument list is always N+1, where N is the number of command-line arguments—beyond the program name, of course.

Therefore, we could revise the previous example as follows:

Click here to view code image

```
import sys

def quad(a, b, c):
    ''"Quadratic Formula function.""

    determin = (b * b - 4 * a * c) ** .5
    x1 = (-b + determin) / (2 * a)
    x2 = (-b - determin) / (2 * a)
    return x1, x2

def main():
    ''"Get argument values, convert, call quad.""
    if len(sys.argv) > 3:
        s1, s2, s3 = sys.argv[1], sys.argv[2],
        sys.argv[3]
    else:
        s1 = input('Enter a: ')
        s2 = input('Enter b: ')
        s3 = input('Enter c: ')
    a, b, c = float(s1), float(s2), float(s3)
    x1, x2 = quad(a, b, c)
```