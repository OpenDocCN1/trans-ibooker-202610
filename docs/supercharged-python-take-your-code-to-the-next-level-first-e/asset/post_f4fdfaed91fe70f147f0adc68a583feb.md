

The **yield** statement changes this function from an ordinary function to a generator function, so it returns a generator object (iterator). We can now determine whether a number is a Fibonacci by using the following test:

> Click here to view code image

```
n = int(input('Enter number: '))
if n in make_fibo_gen(n):
    print('number is a Fibonacci. ')
else:
    print('number is not a Fibonacci. ')
```

This example works because the iterator produced does not yield an infinite sequence, something that would cause a problem. Instead, the iterator terminates if n is reached without being confirmed as a Fibonacci.

Remember—and we state this one last time—by putting **yield** into the function make_fibo_gen, it becomes a generator function and it returns the generator object we need. The previous example could have been written as follows, so that the function call is made in a separate statement. The effect is the same.

> Click here to view code image

```
n = int(input('Enter number: '))
my_fibo_gen = make_fibo_gen(n)
if n in my_fibo_gen:
    print('number is a Fibonacci. ')
else:
    print('number is not a Fibonacci. ')
```

As always, remember that a generator function (which contains the **yield** statement) is not a generator object at all, but rather a generator factory. This is confusing, but you just have to get used to it. In any case, Figure 4.4 shows what's really going on, and you should refer to it often.

## 4.11 ACCESSING COMMAND-LINE ARGUMENTS

Running a program from the command lets you provide the program an extra degree of flexibility. You can let the user specify _command-line arguments_; these are optional arguments that give information directly to the program on start-up. Alternatively, you can let the program prompt the user for the information needed. But use of command-line arguments is typically more efficient.

Command-line arguments are always stored in the form of strings. So—just as with data returned by the input function—you may need to convert this string data to numeric format. To access command-line arguments from within a Python program, first import the **sys** package.

```
import sys
```

You can then refer to the full set of command-line arguments, including the function name itself, by referring to a list named **argv**.

![](img/6e45221a8147f2442c653a3d4a866570_280_0.png)

```
argv   # If 'import sys.argv' used
sys.argv   # If sys imported as 'import sys'
```

In either case, **argv** refers to a list of command-line arguments, all stored as strings. The first element in the list is always the name of the program itself. That element is indexed as **argv[0]**, because Python uses zero-based indexing. For example, suppose that you are running `quad` (a quadratic-equation evaluator) and input the following command line:

```
python quad.py -1 -1 1
```

In this case, **argv** will be realized as a list of four strings. Figure 4.5 illustrates how these strings are stored, emphasizing that the first element, **argv[0]**, refers to a string containing the program name.

![](img/6e45221a8147f2442c653a3d4a866570_281_0.png)

In most cases, you’ll probably ignore the program name and focus on the other arguments. For example, here is a program named `silly.py` that does nothing but print all the arguments given to it, including the program name.

```
import sys
for thing in sys.argv:
    print(thing, end=' ')
```

Now suppose we enter this command line:

```
python silly.py arg1 arg2 arg3
```

The Terminal program (in Mac) or the DOS Box prints the following:

```
silly.py arg1 arg2 arg3
```

The following example gives a more sophisticated way to use these strings, by converting them to floating-point format and passing the numbers to the `quad` function.

> Click here to view code image

```
import sys

def quad(a, b, c):
    '''Quadratic Formula function.'''
    determin = (b * b - 4 * a * c) ** .5
    x1 = (-b + determin) / (2 * a)
    x2 = (-b - determin) / (2 * a)
    return x1, x2

def main():
    '''Get argument values, convert, call quad.'''
    s1, s2, s3 = sys.argv[1], sys.argv[2],
    sys.argv[3]
    a, b, c = float(s1), float(s2), float(s3)
    x1, x2 = quad(a, b, c)
    print('x values: {}, {}'.format(x1, x2))

main()
```

The interesting line here is this one:

> Click here to view code image

```
s1, s2, s3 = sys.argv[1], sys.argv[2],
sys.argv[3]
```

Again, the `sys.argv` list is zero-based, like any other Python list, but the program name, referred to as `sys.argv[0]`, typically isn’t used in the program code. Presumably you already know what the name of your program is, so you don’t need to look it up.

Of course, from within the program you can’t always be sure that argument values were specified on the command line. If they were not specified, you may want to provide an alternative, such as prompting the user for these same values.

Remember that the length of the argument list is always N+1, where N is the number of command-line arguments—beyond the program name, of course.

Therefore, we could revise the previous example as follows:

> Click here to view code image

```
import sys

def quad(a, b, c):
    '''Quadratic Formula function.'''
    determin = (b * b - 4 * a * c) ** .5
    x1 = (-b + determin) / (2 * a)
    x2 = (-b - determin) / (2 * a)
    return x1, x2

def main():
    '''Get argument values, convert, call quad.'''
    if len(sys.argv) > 3:
        s1, s2, s3 = sys.argv[1], sys.argv[2],
        sys.argv[3]
    else:
        s1 = input('Enter a: ')
        s2 = input('Enter b: ')
        s3 = input('Enter c: ')
    a, b, c = float(s1), float(s2), float(s3)
    x1, x2 = quad(a, b, c)
    print('x values: {}, {}'.format(x1, x2))

main()
```

The key lines in this version are in the following **if** statement:

> Click here to view code image

```
if len(sys.argv) > 3:
    s1, s2, s3 = sys.argv[1], sys.argv[2],
        sys.argv[3]
else:
    s1 = input('Enter a: ')
    s2 = input('Enter b: ')
    s3 = input('Enter c: ')
a, b, c = float(s1), float(s2), float(s3)
```

If there are at least four elements in **sys.argv** (and therefore three command-line arguments beyond the program name itself), the program uses those strings. Otherwise, the program prompts for the values.

So, from the command line, you’ll be able to run the following:

```
python quad.py 1 -9 20
```

The program then prints these results:

```
x values: 4.0 5.0
```

# CHAPTER 4 SUMMARY

A large part of this chapter presented ways to improve your efficiency through writing better and more efficient Python code. Beyond that, you can make your Python programs run faster if you call the `print` function as rarely as possible from within IDLE—or else run programs from the command line only.

A technique helpful in making your code more efficient is to profile it by using the `time` and `datetime` packages to compute the relative speed of the code, given different algorithms. Writing decorators is helpful in this respect, because you can use them to profile function performance.

One of the best ways of supercharging your applications, in many cases, is to use one of the many free packages available for use with Python. Some of these are built in; others, like the `numpy` package, you’ll need to download.

# CHAPTER 4 QUESTIONS FOR REVIEW

- 1. Is an assignment operator such as `+=` only a convenience? Can it actually result in faster performance at run time?

- 2. In most computer languages, what is the minimum number of statements you’d need to write instead of the Python statement `a, b = a + b, a`?

- 3. What’s the most efficient way to initialize a list of 100 integers to 0 in Python?

- 4. What’s the most efficient way of initializing a list of 99 integers with the pattern 1, 2, 3 repeated? Show precisely how to do that, if possible.

- 5. If you’re running a Python program from within IDLE, describe how to most efficiently print a multidimensional list.

- 6. Can you use list comprehension on a string? If so, how?

- 7. How can you get help on a user-written Python program from the command line? From within IDLE?

- 8. Functions are said to be “first-class objects” in Python but not in most other languages, such as C++ or Java. What is something you can do with a Python function (callable object) that you cannot do in C or C++?

- 9. What’s the difference between a wrapper, a wrapped function, and a decorator?

- 10. When a function is a generator function, what does it return, if anything?

- 11. From the standpoint of the Python language, what is the one change that needs to be made to a function to turn it into a generator function?

- 12. Name at least one advantage of generators.

# CHAPTER 4 SUGGESTED PROBLEMS

