
## Namespaces are one honking great idea—let’s do more of those!

Namespaces are separate containers for identifiers to prevent naming conflicts. For example, the open() built-in function and the webbrowser.open() function have the same name but refer to different functions. Importing webbrowser doesn’t overwrite the built-in open() function because both open() functions exist in different namespaces: the built-in namespace and the webbrowser module’s namespace, respectively. But keep in mind that flat is better than nested: as great as namespaces are, you should make them only to prevent naming conflicts, not to add needless categorization.

As with all opinions about programming, you can argue against those I’ve listed here, or they might simply be irrelevant to your situation. Arguing over how you should write code or what counts as “pythonic” is rarely as productive as you think it is. (Unless you’re writing an entire book full of programming opinions.)

## Learning to Love Significant Indentation

The most common concern I hear about Python from programmers coming from other languages is that Python’s significant indentation (often mistakenly called significant whitespace) is weird and unfamiliar. The amount of indentation at the start of a line of code has meaning in Python, because it determines which lines of code are in the same code block.

Grouping blocks of Python code using indentation can seem odd, because other languages begin and end their blocks with braces, { and }. But programmers in non-Python languages usually indent their blocks too, just like Python programmers, to make their code more readable. For example, the Java programming language doesn’t have significant indentation. Java programmers don’t need to indent blocks of code, but they often do anyway for readability. The following example has a Java function named main() that contains a single call to a println() function:

```
// Java Example
public static void main(String[] args) {
    System.out.println("Hello, world!");
}
```

This Java code would run just fine if the println() line weren’t indented, because the braces, rather than the indentation, are what mark the start and end of blocks in Java. Instead of allowing indentation to be optional, Python forces your code to be consistently readable. But note that Python doesn’t have significant whitespace, because Python doesn’t restrict how you can use nonindentation whitespace (both 2 + 2 and 2+2 are valid Python expressions).

Some programmers argue that the opening brace should be on the same line as the opening statement, while others argue it should be on the following line. Programmers will argue the merits of their preferred style until the end of time. Python neatly sidesteps this issue by not using braces at all, letting Pythonistas get back to more productive work. I've come to wish that all programming languages would adopt Python's approach to grouping blocks of code.

But some people still long for braces and want to add them to a future version of Python—despite how unpythonic they are. Python's __future__ module backports features to earlier Python versions, and you'll find a hidden Easter egg if you try to import a braces feature into Python:

```
>>> from __future__ import braces
SyntaxError: not a chance
```

I wouldn't count on braces being added to Python any time soon.

## Commonly Misused Syntax

If Python isn't your first programming language, you might write your Python code with the same strategies you use to write code in other programming languages. Or perhaps you learned an unusual way of writing your Python code because you were unaware that there are more established best practices. This awkward code works, but you could save some time and effort by learning more standard approaches to writing pythonic code. This section explains common missteps programmers make and how you should write the code instead.

### Use enumerate() Instead of range()

When looping over a list or other sequence, some programmers use the range() and len() functions to generate the index integers from 0 up to, but not including, the length of the sequence. It's common to use the variable name i (for index) in these for loops. For example, enter the following unpythonic example into the interactive shell:

```
>>> animals = ['cat', 'dog', 'moose']
>>> for i in range(len(animals)):
...     print(i, animals[i])
...
0 cat
1 dog
2 moose
```

The range(len()) convention is straightforward but less than ideal because it can be difficult to read. Instead, pass the list or sequence to the built-in enumerate() function, which will return an integer for the index and the item at that index. For example, you can write the following pythonic code.

```
>>> # Pythonic Example
>>> animals = ['cat', 'dog', 'moose']
>>> for i, animal in enumerate(animals):
...     print(i, animal)
...
0 cat
1 dog
2 moose
```

The code you write will be slightly cleaner using enumerate() instead of range(len()). If you need only the items but not the indexes, you can still directly iterate over the list in a pythonic way:

```
>>> # Pythonic Example
>>> animals = ['cat', 'dog', 'moose']
>>> for animal in animals:
...     print(animal)
...
cat
dog
moose
```

Calling enumerate() and iterating over a sequence directly are preferable to using the old-fashioned range(len()) convention.

### Use the with Statement Instead of open() and close()

The open() function will return a file object that contains methods for reading or writing a file. When you're done, the file object's close() method makes the file available to other programs for reading and writing. You can use these functions individually. But doing so is unpythonic. For example, enter the following into the interactive shell to write the text “Hello, world!” to a file named spam.txt:

```
>>> # Unpythonic Example
>>> fileObj = open('spam.txt', 'w')
>>> fileObj.write('Hello, world!')
13
>>> fileObj.close()
```

Writing code this way can lead to unclosed files if, say, an error occurs in a try block and the program skips the call to close(). For example:

```
>>> # Unpythonic Example
>>> try:
...     fileObj = open('spam.txt', 'w')
...     eggs = 42 / 0  # A zero divide error happens here.
...     fileObj.close()  # This line never runs.
... except:
...     print('Some error occurred.')
...
Some error occurred.
```

Upon reaching the zero divide error, the execution moves to the except block, skipping the close() call and leaving the file open. This can lead to file corruption bugs later that are hard to trace back to the try block.

Instead, you can use the with statement to automatically call close() when the execution leaves the with statement's block. The following pythonic example does the same task as the first example in this section:

```
>>> # Pythonic Example
>>> with open('spam.txt', 'w') as fileObj:
...     fileObj.write('Hello, world!')
...
```

Even though there's no explicit call to close(), the with statement will know to call it when the execution leaves the block.

### Use is to Compare with None Instead of ==

The == equality operator compares two object's values, whereas the is identity operator compares two object's identities. Chapter 7 covers value and identity. Two objects can store equivalent values, but being two separate objects means they have separate identities. However, whenever you compare a value to None, you should almost always use the is operator rather than the == operator.

In some cases, the expression spam == None could evaluate to True even when spam merely contains None. This can happen due to overloading the == operator, which Chapter 17 covers in more detail. But spam is None will check whether the value in the spam variable is literally None. Because None is the only value of the NoneType data type, there is only one None object in any Python program. If a variable is set to None, the is None comparison will always evaluate to True. Chapter 17 describes the specifics of overloading the == operator, but the following is an example of this behavior:

```
>>> class SomeClass:
...     def __eq__(self, other):
...         if other is None:
...             return True
...
>>> spam = SomeClass()
>>> spam == None
True
>>> spam is None
False
```
