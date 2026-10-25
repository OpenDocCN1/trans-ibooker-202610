```
my_fibo_gen = make_fibo_gen(n)
if n in my_fibo_gen:
    print('number is a Fibonacci. ')
else:
    print('number is not a Fibonacci. ')
```

As always, remember that a generator function (which
contains the **yield** statement) is not a generator object at all,
but rather a generator factory. This is confusing, but you just
have to get used to it. In any case, Figure 4.4 shows what's
really going on, and you should refer to it often.

# ## 4.11 ACCESSING COMMAND-LINE ARGUMENTS

Running a program from the command lets you provide the
program an extra degree of flexibility. You can let the user
specify _command-line arguments_; these are optional arguments
that give information directly to the program on start-up.
Alternatively, you can let the program prompt the user for the
information needed. But use of command-line arguments is
typically more efficient.

Command-line arguments are always stored in the form of
strings. So—just as with data returned by the input function—
you may need to convert this string data to numeric format.
To access command-line arguments from within a Python
program, first import the **sys** package.

```
import sys
```