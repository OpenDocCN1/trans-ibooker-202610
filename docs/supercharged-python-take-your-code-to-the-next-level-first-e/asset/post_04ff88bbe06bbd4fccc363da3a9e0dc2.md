

# 5 What happens when one of the indexes used in a slicing expression is out of range?

# 6 If you pass a list to a function, and if you want the function to be able to change the values of the list—so that the list is different after the function returns—what action should you avoid?

# 7 What is an unbalanced matrix?

# 8 Why does the creation of arbitrarily large matrixes require the use of either list comprehension or a loop?

# CHAPTER 3 SUGGESTED PROBLEMS

1 Use the `reduce` list-processing function to help get the average of a randomly chosen list of numbers. The correct answer should be no more than one or two lines of code. Then calculate the deviation of each element by subtracting each element from the average (also called the “mean”) and squaring each result. Finally, return the resulting list.

2 Write a program that enables users to enter a list of numbers, in which the list is any length they want. Then find the _median_ value, not the average or mean. The median value is the value that has just as many greater values as lesser values, in comparison to the rest of the list. If you order the entire list from lowest to highest, and if there are an even number of elements, then the median would be the average of the two values in the middle.

# # 4. Shortcuts, Command Line, and Packages

Master crafters need many things, but, above all, they need to master the tools of the profession. This chapter introduces tools that, even if you’re a fairly experienced Python programmer, you may not have yet learned. These tools will make you more productive as well as increase the efficiency of your programs.

So get ready to learn some new tips and tricks.

# ## 4.1 OVERVIEW

Python is unusually gifted with shortcuts and time-saving programming techniques. This chapter begins with a discussion of twenty-two of these techniques.

Another thing you can do to speed up certain programs is to take advantage of the many packages that are available with Python. Some of these—such as `re` (regular expressions), `system`, `random`, and `math`—come with the standard Python download, and all you have to do is to include an `import` statement. Other packages can be downloaded quite easily with the right tools.

# # 4.2 TWENTY-TWO PROGRAMMING SHORTCUTS

This section lists the most common techniques for shortening and tightening your Python code. Most of these are new in the book, although a few of them have been introduced before and are presented in greater depth here.

+   - Use Python line continuation as needed.
+   - Use `for` loops intelligently.
+   - Understand combined operator assignment (+= etc.).
+   - Use multiple assignment.
+   - Use tuple assignment.
+   - Use advanced tuple assignment.
+   - Use list and string “multiplication.”
+   - Return multiple values.
+   - Use loops and the `else` keyword.
+   - Take advantage of Booleans and `not`.
+   - Treat strings as lists of characters.
+   - Eliminate characters by using `replace`.
+   - Don’t write unnecessary loops.
+   - Use chained comparisons.
+   - Simulate “switch” with a table of functions.
+   - Use the `is` operator correctly.
+   - Use one-line `for` loops.

+   - Squeeze multiple statements onto a line.
+   - Write one-line if/then/else statements.
+   - Create Enum values with `range`.
+   - Reduce the inefficiency of the `print` function within IDLE.
+   - Place underscores inside large numbers.

Let’s look at these ideas in detail.

# ## 4.2.1 Use Python Line Continuation as Needed

In Python, the normal statement terminator is just the end of a physical line (although note the exceptions in Section 3.18). This makes programming easier, because you can naturally assume that statements are one per line.

But what if you need to write a statement longer than one physical line? This dilemma can crop up in a number of ways. For example, you might have a string to print that you can’t fit on one line. You could use literal quotations, but line wraps, in that case, are translated as newlines—something you might not want. The solution, first of all, is to recognize that literal strings positioned next to other literal strings are automatically concatenated.

Click here to view code image

```
>>> my_str = 'I am Hen-er-y the Eighth,' ' I am!'
>>> print(my_str)
I am Hen-er-y the Eighth, I am!
```

If these substrings are too long to put on a single physical line, you have a couple of choices. One is to use the line-continuation character, which is a backslash (\(\\\)).

Click here to view code image

```
my_str = 'I am Hen-er-y the Eighth,' \
' I am!'
```

Another technique is to observe that any open—and so far unmatched—parenthesis, square bracket, or brace automatically causes continuation onto the next physical line. Consequently, you can enter as long a statement as you want—and you can enter a string of any length you want—without necessarily inserting newlines.

Click here to view code image

```
my_str = ('I am Hen-er-y the Eighth,' \
'I am! I am not just any Henry VIII,' \
'I really am!')
```

This statement places all this text in one string. You can likewise use open parentheses with other kinds of statements.

Click here to view code image

```
length_of_hypotenuse = ( (side1 * side1 + side2 * \
side2) \
** 0.5 )
```

A statement is not considered complete until all open parentheses [(] have been matched by closing parentheses [)].

The same is true for braces and square brackets. As a result, this statement will automatically continue to the next physical line.

# ## 4.2.2 Use “for” Loops Intelligently

If you come from the C/C++ world, you may tend to overuse the **range** function to print members of a list. Here’s an example of the C way of writing a **for** loop, using **range** and an indexing operation.

Click here to view code image

```
beat_list = ['John', 'Paul', 'George', 'Ringo']
for i in range(len(beat_list)):
    print(beat_list[i])
```

If you ever write code like this, you should try to break the habit as soon as you can. It’s better to print the contents of a list or iterator directly.

Click here to view code image

```
beat_list = ['John', 'Paul', 'George', 'Ringo']
for guy in beat_list:
    print(guy)
```

Even if you need access to a loop variable, it’s better to use the **enumerate** function to generate such numbers. Here’s an example:

Click here to view code image

```
beat_list = ['John', 'Paul', 'George', 'Ringo']
for i, name in enumerate(beat_list, 1):
    print(i, '. ', name, sep='')
```

This prints

```
1. John
2. Paul
3. George
4. Ringo
```

There are, of course, some cases in which it’s necessary to use indexing. That happens most often when you are trying to change the contents of a list in place.

# ## 4.2.3 Understand Combined Operator Assignment (+= etc.)

The combined operator-assignment operators are introduced in Chapter 1 and so are reviewed only briefly here. Remember that assignment (=) can be combined with any of the following operators: +, -, /, //, %, **, &, ^, |, <<, >>.

The operators &, |, and ^ are bitwise “and,” “or,” and “exclusive or,” respectively. The operators << and >> perform bit shifts to the left and to the right.

This section covers some finer points of operator-assignment usage. First, any assignment operator has low precedence and is carried out last.

Second, an assignment operator may or may not be in place, depending on whether the type operated on is mutable. _In place_ refers to operations that work on existing data in memory rather than creating a completely new object. Such operations are faster and more efficient.

Integers, floating-point numbers, and strings are immutable. Assignment operators, used with these types, do not cause in-place assignment; they instead must produce a completely new object, which is reassigned to the variable. Here’s an example:

```
s1 = s2 = 'A string.'
s1 += '...with more stuff!'
print('s1:', s1)
print('s2:', s2)
```

The print function, in this case, produces the following output:

```
s1: A string...with more stuff!
s2: A string.
```

When s1 was assigned a new value, it did not change the string data in place; it assigned a whole new string to s1. But s2 is a name that still refers to the original string data. This is why s1 and s2 now contain different strings.

But lists are mutable, and therefore changes to lists can occur in place.

```
a_list = b_list = [10, 20]
a_list += [30, 40]
print('a_list:', a_list)
print('b_list:', b_list)
```
