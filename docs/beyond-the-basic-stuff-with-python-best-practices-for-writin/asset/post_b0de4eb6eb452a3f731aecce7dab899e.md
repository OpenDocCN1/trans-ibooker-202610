
String interning is an optimization technique that interpreters and compilers use for many different languages. You'll find further details at https://en.wikipedia.org/wiki/String_interning.

## Python’s Fake Increment and Decrement Operators

In Python, you can increase the value of a variable by 1 or reduce it by 1 using the augmented assignment operators. The code spam += 1 and spam -= 1 increments and decrements the numeric values in spam by 1, respectively.

Other languages, such as C++ and JavaScript, have the ++ and -- operators for incrementing and decrementing. (The name “C++” itself reflects this; it’s a tongue-in-cheek joke that indicates it’s an enhanced form of the C language.) Code in C++ and JavaScript could have operations like ++spam or spam++. Python wisely doesn’t include these operators because they’re notoriously susceptible to subtle bugs (as discussed at https://softwareengineering.stackexchange.com/q/59880).

But it’s perfectly legal to have the following Python code:

```
>>> spam = --spam
>>> spam
42
```

The first detail you should notice is that the ++ and -- “operators” in Python don’t actually increment or decrement the value in spam. Rather, the leading - is Python’s unary negation operator. It allows you to write code like this:

```
>>> spam = 42
>>> -spam
-42
```

It’s legal to have multiple unary negative operators in front of a value. Using two of them gives you the negative of the negative of the value, which for integer values just evaluates to the original value:

```
>>> spam = 42
>>> -(-spam)
42
```

This is a very silly operation to perform, and you likely won’t ever see a unary negation operator used twice in real-world code. (But if you did, it’s probably because the programmer learned to program in another language and has just written buggy Python code!)

There is also a + unary operator. It evaluates an integer value to the same sign as the original value, which is to say, it does absolutely nothing:

```
>>> spam = 42
>>> +spam
42
>>> spam = -42
```

```
>>> +spam
-42
```

Writing +42 (or ++42) seems just as silly as --42, so why does Python even have this unary operator? It exists only to complement the - operator if you need to overload these operators for your own classes. (That's a lot of terms you might not be familiar with! You'll learn more about operator overloading in Chapter 17.)

The + and - unary operators are only valid when in front of a Python value, not after it. Although spam++ and spam-- might be legal code in C++ or JavaScript, they produce syntax errors in Python:

```
>>> spam++
  File "<stdin>", line 1
    spam++
         ^
SyntaxError: invalid syntax
```

Python doesn't have increment and decrement operators. A quirk of the language syntax merely makes it seem like it does.

## All of Nothing

The all() built-in function accepts a sequence value, such as a list, and returns True if all the values in that sequence are “truthy.” It returns False if one or more values are “falsey.” You can think of the function call all([False, True, True]) as equivalent to the expression False and True and True.

You can use all() in conjunction with list comprehensions to first create a list of Boolean values based on another list and then evaluate their collective value. For example, enter the following into the interactive shell:

```
>>> spam = [67, 39, 20, 55, 13, 45, 44]
>>> [i > 42 for i in spam]
[True, False, False, True, False, True, True]
>>> all([i > 42 for i in spam])
False
>>> eggs = [43, 44, 45, 46]
>>> all([i > 42 for i in eggs])
True
```

The all() utility returns True if all numbers in spam or eggs are greater than 42.

But if you pass an empty sequence to all(), it always returns True. Enter the following into the interactive shell:

```
>>> all([])
True
```

It's best to think of all([]) as evaluating the claim “none of the items in this list are falsey” instead of “all the items in this list are truthy.” Otherwise,

you might get some odd results. For instance, enter the following into the interactive shell:

```
>>> spam = []
>>> all([i > 42 for i in spam])
True
>>> all([i < 42 for i in spam])
True
>>> all([i == 42 for i in spam])
True
```

This code seems to be showing that not only are all the values in spam (an empty list) greater than 42, but they’re also less than 42 and exactly equal to 42! This seems logically impossible. But remember that each of these three list comprehensions evaluates to the empty list, which is why none of the items in them are falsey and the all() function returns True.

## Boolean Values Are Integer Values

Just as Python considers the float value 42.0 to be equal to the integer value 42, it considers the Boolean values True and False to be equivalent to 1 and 0, respectively. In Python, the bool data type is a subclass of the int data type. (We’ll cover classes and subclasses in Chapter 16.) You can use int() to convert Boolean values to integers:

```
>>> int(False)
0
>>> int(True)
1
>>> True == 1
True
>>> False == 0
True
```

You can also use isinstance() to confirm that a Boolean value is considered a type of integer:

```
>>> isinstance(True, bool)
True
>>> isinstance(True, int)
True
```

The value True is of the bool data type. But because bool is a subclass of int, True is also an int. This means you can use True and False in almost any place you can use integers. This can lead to some bizarre code:

```
>>> True + False + True + True  # Same as 1 + 0 + 1 + 1
3
>>> -True          # Same as -1.
-1
```

```
>>> 42 * True      # Same as 42 * 1 mathematical multiplication.
42
>>> 'hello' * False # Same as 'hello' * 0 string replication.
''
>>> 'hello'[False]  # Same as 'hello'[0]
'h'
>>> 'hello'[True]   # Same as 'hello'[1]
'e'
>>> 'hello'[-True]  # Same as 'hello'[-1]
'o'
```

Of course, just because you can use bool values as numbers doesn’t mean you should. The previous examples are all unreadable and should never be used in real-world code. Originally, Python didn’t have a bool data type. It didn’t add Booleans until Python 2.3, at which point it made bool a subclass of int to ease the implementation. You can read the history of the bool data type in PEP 285 at https://www.python.org/dev/peps/pep-0285/.

Incidentally, True and False were only made keywords in Python 3. This means that in Python 2, it was possible to use True and False as variable names, leading to seemingly paradoxical code like this:

```
Python 2.7.14 (v2.7.14:84471935ed, Sep 16 2017, 20:25:58) [MSC v.1500 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> True is False
False
>>> True = False
>>> True is False
True
```

Fortunately, this sort of confusing code isn’t possible in Python 3, which will raise a syntax error if you try to use the keywords True or False as variable names.

## Chaining Multiple Kinds of Operators

Chaining different kinds of operators in the same expression can produce unexpected bugs. For example, this (admittedly unrealistic) example uses the == and in operators in a single expression:

```
>>> False == False in [False]
True
```

This True result is surprising, because you would expect it to evaluate as either:

- (False == False) in [False], which is False.
- False == (False in [False]), which is also False.

But False == False in [False] isn’t equivalent to either of these expressions. Rather, it’s equivalent to (False == False) and (False in [False]), just

as 42 < spam < 99 is equivalent to (42 < spam) and (spam < 99). This expression evaluates according to the following diagram:

```
(False == False) and (False in [False])
↓
(True) and (False in [False])
↓
(True) and (True)
↓
True
```

The False == False in [False] expression is a fun Python riddle, but it's unlikely to come up in any real-world code.

## Python's Antigravity Feature

To enable Python's antigravity feature, enter the following into the interactive shell:

```
>>> import antigravity
```

This line is a fun Easter egg that opens the web browser to a classic XKCD comic strip about Python at https://xkcd.com/353/. It might surprise you that Python can open your web browser, but this is a built-in feature the webbrowser module provides. Python's webbrowser module has an open() function that finds your operating system's default web browser and opens a browser window to a specific URL. Enter the following into the interactive shell:

```
>>> import webbrowser
>>> webbrowser.open('https://xkcd.com/353/')
```

The webbrowser module is limited, but it can be useful for directing the user to further information on the internet.

## Summary

It's easy to forget that computers and programming languages are designed by humans and have their own limitations. So much software is built on top of and relies upon the creations of language designers and hardware engineers. They work incredibly hard to make sure that if you have a bug in your program, it's because your program is faulty, not the interpreter software or CPU hardware running it. We can end up taking these tools for granted.

But this is why there's value in learning the odd nooks and crannies of computers and software. When your code raises errors or crashes (or even just acts weirdly and makes you think, “that's odd”), you'll need to understand the common gotchas to debug these problems.

You almost certainly won't run into any of the issues brought up in this chapter, but being aware of these small details is what will make you an experienced Python programmer.
