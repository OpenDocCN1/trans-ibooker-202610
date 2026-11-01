```
File "abcTraceback.py", line 11, in c
    42 / 0 # This will cause a zero divide error.
ZeroDivisionError: division by zero
```

Let's examine this traceback line by line, starting with this line:

```
Traceback (most recent call last):
```

This message lets you know that what follows is a traceback. The most recent call last text indicates that each of the function calls is listed in order, starting with the first function call and ending with the most recent.

The next line shows the traceback's first function call:

```
File "abcTraceback.py", line 13, in <module>
    a()  # Call a().
```

These two lines are the frame summary, and they show the information inside a frame object. When a function is called, the local variable data as well as where in the code to return to after the function call ends are stored in a frame object. Frame objects hold local variables and other data associated with function calls. Frame objects are created when the function is called and destroyed when the function returns. The traceback shows a frame summary for each frame leading up to the crash. We can see that this function call is on line 13 of abcTraceback.py, and the <module> text tells us this line is in the global scope. Line 13 is displayed with two spaces of indentation next.

The four lines that follow are the next two frame summaries:

```
File "abcTraceback.py", line 3, in a
    b()  # Call b().
File "abcTraceback.py", line 7, in b
    c()  # Call c().
```

We can tell from the line 3, in a text that b() was called on line 3 inside the a() function, which led to c() being called on line 7 inside the b() function. Notice that the print() calls on lines 2, 6, and 10 aren't displayed in the traceback, even though they ran before the function calls occurred. Only the lines containing function calls that lead up to the exception are displayed in the traceback.

The last frame summary shows the line that caused the unhandled exception, followed by the name of the exception and the exception's message:

```
File "abcTraceback.py", line 11, in c
    42 / 0 # This will cause a zero divide error.
ZeroDivisionError: division by zero
```

Note that the line number given by the traceback is where Python finally detected an error. The true source of the bug could be somewhere before this line.