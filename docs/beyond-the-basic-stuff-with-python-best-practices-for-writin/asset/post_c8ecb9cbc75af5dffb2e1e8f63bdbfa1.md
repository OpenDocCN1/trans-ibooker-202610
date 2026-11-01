
rather than the actual objects. If your list contains other lists, copy.copy() only copies the references to these inner lists. Enter the following into the interactive shell to see this problem:

```
>>> import copy
>>> bacon = [[1, 2], [3, 4]]
>>> ham = copy.copy(bacon)
>>> id(bacon), id(ham)
(2356896466248, 2356896375368)
>>> bacon.append('APPENDED')
>>> bacon
[[1, 2], [3, 4], 'APPENDED']
>>> ham
[[1, 2], [3, 4]]
>>> bacon[0][0] = 'CHANGED'
>>> bacon
[['CHANGED', 2], [3, 4], 'APPENDED']
>>> ham
[['CHANGED', 2], [3, 4]]
>>> id(bacon[0]), id(ham[0])
(2356896337480, 2356896337480)
```

A visualization of the execution of this code is at https://author.com/copycopy2. Although bacon and ham are two different list objects, they refer to the same [1, 2] and [3, 4] inner lists, so changes to these inner lists get reflected in both variables, even though we used copy.copy(). The solution is to use copy.deepcopy(), which will make copies of any list objects inside the list object being copied (and any list objects in those list objects, and so on). Enter the following into the interactive shell:

```
>>> import copy
>>> bacon = [[1, 2], [3, 4]]
>>> ham = copy.deepcopy(bacon)
>>> id(bacon[0]), id(ham[0])
(2356896337352, 2356896466184)
>>> bacon[0][0] = 'CHANGED'
>>> bacon
[['CHANGED', 2], [3, 4]]
>>> ham
[[1, 2], [3, 4]]
```

A visualization of the execution of this code is at https://author.com/copydeepcopy. Although copy.deepcopy() is slightly slower than copy.copy(), it's safer to use if you don't know whether the list being copied contains other lists (or other mutable objects like dictionaries or sets). My general advice is to always use copy.deepcopy(): it might prevent subtle bugs, and the slowdown in your code probably won't be noticeable.

## Don’t Use Mutable Values for Default Arguments

Python allows you to set default arguments for parameters in the functions you define. If a user doesn’t explicitly set a parameter, the function will execute using the default argument. This is useful when most calls to the function use the same argument, because default arguments make the parameter optional. For example, passing None for the split() method makes it split on whitespace characters, but None is also the default argument: calling 'cat dog'.split() does the same thing as calling 'cat dog'.split(None). The function uses the default argument for the parameter’s argument unless the caller passes one in.

But you should never set a mutable object, such as a list or dictionary, as a default argument. To see how this causes bugs, look at the following example, which defines an addIngredient() function that adds an ingredient string to a list that represents a sandwich. Because the first and last items of this list are often 'bread', the mutable list ['bread', 'bread'] is used as a default argument:

```
>>> def addIngredient(ingredient, sandwich=['bread', 'bread']):
...     sandwich.insert(1, ingredient)
...     return sandwich
...
>>> mySandwich = addIngredient('avocado')
>>> mySandwich
['bread', 'avocado', 'bread']
```

But using a mutable object, such as a list like ['bread', 'bread'], for the default argument has a subtle problem: the list is created when the function’s def statement executes, not each time the function is called. This means that only one ['bread', 'bread'] list object gets created, because we only define the addIngredient() function once. But each function call to addIngredient() will be reusing this list. This leads to unexpected behavior, like the following:

```
>>> mySandwich = addIngredient('avocado')
>>> mySandwich
['bread', 'avocado', 'bread']
>>> anotherSandwich = addIngredient('lettuce')
>>> anotherSandwich
['bread', 'lettuce', 'avocado', 'bread']
```

Because addIngredient('lettuce') ends up using the same default argument list as the previous calls, which already had 'avocado' added to it, instead of ['bread', 'lettuce', 'bread'] the function returns ['bread', 'lettuce', 'avocado', 'bread']. The 'avocado' string appears again because the list for the sandwich parameter is the same as the last function call. Only one ['bread', 'bread'] list was created, because the function’s def statement only executes once, not each time the function is called. A visualization of the execution of this code is at https://author.com/sandwich.

If you need to use a list or dictionary as a default argument, the pythonic solution is to set the default argument to None. Then have code that checks for this and supplies a new list or dictionary whenever the function is called. This ensures that the function creates a new mutable object each time the function is called instead of just once when the function is defined, such as in the following example:

```
>>> def addIngredient(ingredient, sandwich=None):
...     if sandwich is None:
...         sandwich = ['bread', 'bread']
...     sandwich.insert(1, ingredient)
...     return sandwich
...
>>> firstSandwich = addIngredient('cranberries')
>>> firstSandwich
['bread', 'cranberries', 'bread']
>>> secondSandwich = addIngredient('lettuce')
>>> secondSandwich
['bread', 'lettuce', 'bread']
>>> id(firstSandwich) == id(secondSandwich)
False
```

Notice that firstSandwich and secondSandwich don’t share the same list reference ① because sandwich = ['bread', 'bread'] creates a new list object each time addIngredient() is called, not just once when addIngredient() is defined.

Mutable data types include lists, dictionaries, sets, and objects made from the class statement. Don’t put objects of these types as default arguments in a def statement.

## Don’t Build Strings with String Concatenation

In Python, strings are immutable objects. This means that string values can’t change, and any code that seems to modify the string is actually creating a new string object. For example, each of the following operations changes the content of the spam variable, not by changing the string value, but by replacing it with a new string value that has a new identity:

```
>>> spam = 'Hello'
>>> id(spam), spam
(38330864, 'Hello')
>>> spam = spam + ' world!'
>>> id(spam), spam
(38329712, 'Hello world!')
>>> spam = spam.upper()
>>> id(spam), spam
(38329648, 'HELLO WORLD!')
>>> spam = 'Hi'
>>> id(spam), spam
(38395568, 'Hi')
>>> spam = f'{spam} world!'
>>> id(spam), spam
(38330864, 'Hi world!')
```

Notice that each call to id(spam) returns a different identity, because the string object in spam isn't being changed: it's being replaced by a whole new string object with a different identity. Creating new strings by using f-strings, the format() string method, or the %s format specifiers also creates new string objects, just like string concatenation. Normally, this technical detail doesn't matter. Python is a high-level language that handles many of these details for you so you can focus on creating your program.

But building a string through a large number of string concatenations can slow down your programs. Each iteration of the loop creates a new string object and discards the old string object: in code, this looks like concatenations inside a for or while loop, as in the following:

```python
>>> finalString = ''
>>> for i in range(100000):
...     finalString += 'spam '
...
>>> finalString
spam spam spam spam spam spam spam spam spam spam spam spam --snip--
```

Because the finalString += 'spam ' happens 100,000 times inside the loop, Python is performing 100,000 string concatenations. The CPU has to create these intermediate string values by concatenating the current finalString with 'spam ', put them into memory, and then almost immediately discard them on the next iteration. This is a lot of wasted effort, because we only care about the final string.

The pythonic way to build strings is to append the smaller strings to a list and then join the list together into one string. This method still creates 100,000 string objects, but it only performs one string concatenation, when it calls join(). For example, the following code produces the equivalent finalString but without the intermediate string concatenations:

```python
>>> finalString = []
>>> for i in range(100000):
...     finalString.append('spam ')
...
>>> finalString = ''.join(finalString)
>>> finalString
spam spam spam spam spam spam spam spam spam spam spam spam --snip--
```

When I measure the runtime of these two pieces of code on my machine, the list appending approach is 10 times faster than the string concatenation approach. (Chapter 13 describes how to measure how fast your programs run.) This difference becomes greater the more iterations the for loop makes. But when you change range(100000) to range(100), although concatenation remains slower than list appending, the speed difference is negligible. You don't need to obsessively avoid string concatenation, f-strings,
