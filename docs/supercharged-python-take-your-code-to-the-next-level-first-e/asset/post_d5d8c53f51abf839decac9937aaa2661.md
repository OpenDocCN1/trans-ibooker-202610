
variables. But so does the following code, which—upon being executed—creates a variable `foo` if it does not already exist.

Click here to view code image

```python
def my_func():
    global foo
    foo = 5     # Create foo if it does not already
                # exist (as a global).
    print(foo)
```

Assuming `foo` does not already exist, the effect of this function is to create `foo` and set it to 5. It cannot be created as a local—because of the statement **global `foo`**—and therefore `foo` is created as a global variable. This works even though the assignment to `foo` is not part of module-level code.

In general, there is a golden rule about global and local variables in Python. It’s simply this:

> If there’s any chance that a function might attempt to assign a value to a global variable, use the `global` statement so that it’s not treated as local.

## CHAPTER 1 SUMMARY

Chapter 1 covers the fundamentals of Python except for class definitions, advanced operations on collections, and specialized parts of the library such as file operations. The information presented here is enough to write many Python programs.

So congratulations! If you understand everything in this chapter, you are already well on the way to becoming a fluent Python programmer. The next couple of chapters plunge into the fine points of lists and strings, the two most important kinds of collections.

Chapter 3 covers called something called “comprehension” in Python (not to be confused with artificial intelligence) and explains how comprehension applies not only to lists but also to sets, dictionaries, and other collections. It also shows you how to use lambda functions.

## CHAPTER 1 REVIEW QUESTIONS

- 1 Considering that there are no data declarations in Python, is it even theoretically possible to have uninitialized data?

- 2 In what sense are Python integers “infinite,” and in what sense are they not infinite at all?

- 3 Is a class having infinite range even theoretically possible?

- 4 How exactly is indentation in Python more critical than in most other programming languages?

- 5 The best policy is to use a completely consistent indentation scheme throughout a Python program, but does Python give you some leeway? Exactly where must indentation be consistent in a program? Where can it differ? Show examples if you can.

- 6 Explain precisely why tab characters can cause a problem with the indentations used in a Python program (and thereby introduce syntax errors)?

- 7 What is the advantage of having to rely so much on indentation in Python?

- 8 How many different values can a Python function return to the caller?

- 9 Recount this chapter’s solution to the forward reference problem for functions. How can such an issue arise in the first place?

- 10 When you’re writing a Python text string, what, if anything, should guide your choice of what kind of quotation marks to use (single, double, or triple)?

- 11 Name at least one way in which Python lists are different from arrays in other languages, such as C, which are contiguously stored collections of a single base type.

## CHAPTER 1 SUGGESTED PROBLEMS

1. Write a little program that asks for your name, age, and address, and then prints all the information you just entered. However, instead of placing it in a function called main, place it in a function called test_func. Then call test_func to run it.

2. Write a program that gets the radius of a sphere, calculates the volume, and then prints the answer. If necessary, look up the volume formula online.

# 2. Advanced String Capabilities

How does a computer communicate messages to humans? Through hand-waving, smoke signals, or (as in sci-fi movies of the 1950s) a blinking red light?

No. Even programs that utilize voice or voice recognition (somewhat outside the scope of this book) depend on groups of printable characters called text strings, or just strings. Every programmer needs to manage the art of prompting for, searching, and printing these strings. Fortunately, Python excels at this task.

Even if you’ve used Python text strings before, you’ll likely want to peruse this chapter to make sure that you’re using all the built-in capabilities of Python strings.

## 2.1 STRINGS ARE IMMUTABLE

Data types in Python are either mutable (changeable) or immutable.

The advantage of mutable types is clear. The data can be changed “in place,” meaning you don’t have to reconstruct an object from scratch every time you make a change. Mutable types include lists, dictionaries, and sets.

The advantage of immutable types is less obvious but important. An immutable type can be used as a key for a dictionary; such keys are frequently strings. For example, you might have a ratings dictionary to list average ratings from a group of critics.

```python
movie_dict = { 'Star Bores': 5.0,
	'The Oddfather': 4.5,
	'Piranha: The Revenge': 2.0 }
```

Another advantage of immutable types is that because they cannot be changed, their usage is optimized internally. Using tuples, for example, is somewhat more efficient than using lists. The limitation of immutable types is that such data cannot be changed in place. The following statements, for example, are not valid.

```python
my_str = 'hello, Dave, this is Hal.'
my_str[0] = 'H' # ERROR!
```

The second statement in this example is invalid because it attempts to take the string created in the first statement and modify the data itself. As a result, Python raises a **TypeError** exception. But the following statements are valid.

```python
my_str = 'hello'
my_str = 'Hello'
```

These statements are valid because each time, a completely new string is created, and the name `my_str` is reassigned.

In Python, a variable is nothing more than a name, and it may be reused, over and over. That’s why these last statements might seem to violate immutability of strings but in fact do not. No existing string is altered in this last example; rather, two different strings are created and the name `my_str` is reused.

This behavior follows from the nature of assignment in Python and its lack of data declarations. You can reuse a name as often as you want.

## 2.2 NUMERIC CONVERSIONS, INCLUDING BINARY

Type names in Python implicitly invoke type conversions wherever such conversions are supported.

![](img/6e45221a8147f2442c653a3d4a866570_96_0.png)

```python
type(data_object)
```

The action is to take the specified `data_object` and produce the result after converting it to the specified `type`—if the appropriate conversion exists. If not, Python raises a **ValueError** exception.

Here are some examples:

```python
s = '45'
n = int(s)
x = float(s)
```

If you then print `n` and `x`, you get the following:

```
45
45.0
```

The `int` conversion, unlike most conversions, takes an optional second argument. This argument enables you to convert a string to a number while interpreting it in a different radix, such as binary. Here’s an example:

Click here to view code image

```python
n = int('10001', 2)  # Interpret in binary radix.
```

Printing `n` reveals it was assigned the decimal value 17. Likewise, you can use other bases with the `int` conversion. The following code uses octal (8) and hexadecimal (16) bases.

Click here to view code image

```python
n1 = int('775', 8)
n2 = int('1E', 16)
print('775 octal and 1E hex:', n1, n2)
```

These statements print the following results:

```
775 octal and 1E hex: 509 30
```

We can therefore summarize the **int** conversion as taking an optional second argument, which has a default value of 10, indicating decimal radix.

![](img/6e45221a8147f2442c653a3d4a866570_98_0.png)

```python
int(data_object, radix=10)
```

The **int** and **float** conversions are necessary when you get input from the keyboard—usually by using the **input** statement —or get input from a text file, and you need to convert the digit characters into an actual numeric value.

A **str** conversion works in the opposite direction. It converts a number into its string representation. In fact, it works on any type of data for which the type defines a string representation.

Converting a number to a string enables you to do operations such as counting the number of printable digits or counting the number of times a specific digit occurs. For example, the following statements print the length of the number 1007.

Click here to view code image

```python
n = 1007
s = str(n)    # Convert to '1007'
print('The length of', n, 'is', len(s), 'digits.')
```

This example prints the following output:

Click here to view code image
