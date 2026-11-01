
Although this looks like a function call ①, it's actually a print statement with a string value wrapped in parentheses, the same way assigning spam = (2 + 2) is equivalent to spam = 2 + 2. In Python 2 and 3, you can pass multiple values to the print statement or print() function, respectively. In Python 3, this would look like the following:

```
>>> print('Hello', 'world') # run in Python 3
Hello world
```

But using this same code in Python 2 would be interpreted as passing a tuple of two string values in a print statement, producing this output:

```
>>> print('Hello', 'world') # run in Python 2
('Hello', 'world')
```

A statement and an expression composed of a function call have subtle but real differences.

## Block vs. Clause vs. Body

The terms block, clause, and body are often used interchangeably to refer to a group of Python instructions. A block begins with indentation and ends when that indentation returns to the previous indent level. For example, the code that follows an if or for statement is called the statement's block. A new block is required following statements that end with a colon, such as if, else, for, while, def, class, and so on.

But Python does allow one-line blocks. This is valid, although not recommended, Python syntax:

```
if name == 'Zophie': print('Hello, kitty!')
```

By using the semicolon, you can also have multiple instructions in the if statement's block:

```
if name == 'Zophie': print('Hello, kitty!'); print('Do you want a treat?')
```

But you can't have one-liners with other statements that require new blocks. The following isn't valid Python code:

```
if name == 'Zophie': if age < 2: print('Hello, kitten!')
```

The if statement is the clause header, and the two print() calls nested in the if are the clause suite or body. The official Python documentation uses block to refer to a piece of Python code that executes as a unit, such as a module, a function, or a class definition (https://docs.python.org/3/reference/executionmodel.html).

## Variable vs. Attribute

Variables are simply names that refer to objects. Attributes are, to quote the official documentation, “any name following a dot” (https://docs.python.org/3/tutorial/classes.html#python-scopes-and-namespaces). Attributes are associated with objects (the name before the dot/period). For example, enter the following into the interactive shell:

```
>>> import datetime
>>> spam = datetime.datetime.now()
>>> spam.year
2018
>>> spam.month
1
```

In this code example, spam is a variable that contains a datetime object (returned from datetime.datetime.now()), and year and month are attributes of that object. Even in the case of, say, sys.exit(), the exit() function is considered an attribute of the sys module object.

Other languages call attributes fields, properties, or member variables.

## Function vs. Method

A function is a collection of code that runs when called. A method is a function (or a callable, described in the next section) that is associated with a class, just as an attribute is a variable associated with an object. Functions include built-in functions or functions associated with a module. For example, enter the following into the interactive shell:

```
>>> len('Hello')
5
>>> 'Hello'.upper()
'HELLO'
>>> import math
>>> math.sqrt(25)
5.0
```

In this example, len() is a function and upper() is a string method. Methods are also considered attributes of the objects they’re associated with. Note that a period doesn’t necessarily mean you’re dealing with a method instead of a function. The sqrt() function is associated with math, which is a module, not a class.

## Iterable vs. Iterator

Python’s for loops are versatile. The statement for i in range(3): will run a block of code three times. The range(3) call isn’t just Python’s way of telling a for loop, “repeat some code three times.” Calling range(3) returns a range object, just like calling list('cat') returns a list object. Both of these objects are examples of iterable objects (or simply, iterables).

You use iterables in for loops. Enter the following into the interactive shell to see a for loop iterate over a range object and a list object:

```
>>> for i in range(3):
...   print(i) # body of the for loop
...
0
1
2
>>> for i in ['c', 'a', 't']:
...   print(i) # body of the for loop
...
c
a
t
```

Iterables also include all sequence types, such as range, list, tuple, and string objects, but also some container objects, such as dictionary, set, and file objects.

However, more is going on under the hood in these for loop examples. Behind the scenes, Python is calling the built-in iter() and next() functions for the for loop. When used in a for loop, iterable objects are passed to the built-in iter() function, which returns iterator objects. Although the iterable object contains the items, the iterator object keeps track of which item is next to be used in a loop. On each iteration of the loop, the iterator object is passed to the built-in next() function to return the next item in the iterable. We can call the iter() and next() functions manually to directly see how for loops work. Enter the following into the interactive shell to perform the same instructions as the previous loop example:

```
>>> iterableObj = range(3)
>>> iterableObj
range(0, 3)
>>> iteratorObj = iter(iterableObj)
>>> i = next(iteratorObj)
>>> print(i) # body of the for loop
0
>>> i = next(iteratorObj)
>>> print(i) # body of the for loop
1
>>> i = next(iteratorObj)
>>> print(i) # body of the for loop
2
>>> i = next(iteratorObj)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

Notice that if you call next() after the last item in the iterable has been returned, Python raises a StopIteration exception 1. Instead of crashing your programs with this error message, Python’s for loops catch this exception to know when they should stop looping.

An iterator can only iterate over the items in an iterable once. This is similar to how you can only use open() and readlines() to read the contents of a file once before having to reopen the file to read its contents again. If you want to iterate over the iterable again, you must call iter() again to create another iterator object. You can create as many iterator objects as you want; each will independently track the next item it should return. Enter the following into the interactive shell to see how this works:

```
>>> iterableObj = list('cat')
>>> iterableObj
['c', 'a', 't']
>>> iteratorObj1 = iter(iterableObj)
>>> iteratorObj2 = iter(iterableObj)
>>> next(iteratorObj1)
'c'
>>> next(iteratorObj1)
'a'
>>> next(iteratorObj2)
'c'
```

Remember that iterable objects are passed as an argument to the iter() function, whereas the object returned from iter() calls is an iterator object. Iterator objects are passed to the next() function. When you create your own data types with class statements, you can implement the __iter__() and __next__() special methods to use your objects in for loops.

## Syntax vs. Runtime vs. Semantic Errors

There are many ways to categorize bugs. But at a high level you could divide programming errors into three types: syntax errors, runtime errors, and semantic errors.

Syntax is the set of rules for the valid instructions in a given programming language. A syntax error, such as a missing parenthesis, a period instead of a comma, or some other typo will immediately generate a SyntaxError. Syntax errors are also known as parsing errors, which occur when the Python interpreter can’t parse the text of the source code into valid instructions. In English, this error would be the equivalent of having incorrect grammar or a string of nonsense words like, “by uncontaminated cheese certainly it’s.” Computers require specific instructions and can’t read the programmer’s mind to determine what the program should do, so a program with a syntax error won’t even run.

A runtime error is when a running program fails to perform some task, such as trying to open a file that doesn't exist or dividing a number by zero. In English, a runtime error is the equivalent of giving an impossible instruction like, "Draw a square with three sides." If a runtime error isn't addressed, the program will crash and display a traceback. But you can catch runtime errors using try-except statements that run error handling code. For example, enter the following into the interactive shell:

```
>>> slices = 8
>>> eaters = 0
>>> print('Each person eats', slices / eaters, 'slices.')
```

This code will display this traceback when you run it:

```
Traceback (most recent call last):
  File "<pyshell#4>", line 1, in <module>
    print('Each person eats', slices / eaters, 'slices.')
ZeroDivisionError: division by zero
```

It's helpful to remember that the line number the traceback mentions is only the point at which the Python interpreter detected an error. The true cause of the error might be on the previous line of code or even much earlier in the program.

Syntax errors in the source code are caught by the interpreter before the program runs, but syntax errors can also happen at runtime. The eval() function can take a string of Python code and run it, which might produce a SyntaxError at runtime. For example, eval('print("Hello, world)') is missing a closing double quote, which the program won't encounter until the code calls eval().

A semantic error (also called a logical error) is a more subtle bug. Semantic errors won't cause error messages or crashes, but the computer carries out instructions in a way the programmer didn't intend. In English, the equivalent of a semantic error would be telling the computer, "Buy a carton of milk from the store and if they have eggs, buy a dozen." The computer would then buy 13 cartons of milk because the store had eggs. For better or worse, computers do exactly what you tell them to. For example, enter the following into the interactive shell:

```
>>> print('The sum of 4 and 2 is', '4' + '2')
```

You would get the following output:

```
The sum of 4 and 2 is 42
```

Obviously, 42 isn't the answer. But notice that the program didn't crash. Because Python's + operator adds integer values and concatenates string values, mistakenly using the string values '4' and '2' instead of integers caused unintended behavior.
