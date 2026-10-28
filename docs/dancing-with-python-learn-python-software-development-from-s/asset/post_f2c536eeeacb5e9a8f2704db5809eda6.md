
While we need the else clause in this example, you don’t need to include it if you don’t need it. You might simply want to do something if a condition is true and then move on.

```
if number_of_shortbread_cookies == 0:
    print("Shortbread cookies are out of stock.")

if number_of_sugar_cookies == 0:
    print("Sugar cookies are out of stock.")
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_89_0.png)

Figure 2.8: Flowchart for shortbread and sugar cookies

### 2.10.2 “elif”

One or more elif clauses are also optional. The elif is short for “else if.”

```
if number_of_shortbread_cookies > 0:
    print("Here is a shortbread cookie.")
elif number_of_sugar_cookies > 0:
    print("Here is a sugar cookie.")
else:
    print("Sorry, I have no cookies to give you.")
```

### Exercise 2.15

Draw a flowchart of this `elif` example.

You may use as many `elif` clauses as you wish, though beyond three or four, there are probably more efficient ways to write your code.

Python is very flexible in what it considers to be true or false in a condition, beyond the literal or computed Boolean values `True` and `False`.

- The number 0 (or 0.0) is false, and all other numbers are true.
- An empty list is false, but a list with any items in it is true.
- The empty string `""` is false, and all other strings are true.

Suppose `cookies_in_stock` is a list of the kinds of cookies we have available for purchase.

```
cookies_in_stock = ["shortbread", "sugar", "oatmeal"]

if not cookies_in_stock:
    print("We are out of cookies.")
```

The condition is not true and Python does not display the message. You should always use a form like this to test whether a list is empty rather than computing its length and comparing it to 0.

## 2.11 Loops

We use a loop to repeat some action or set of actions while a condition is true.

### 2.11.1 while

Suppose we want to know the sum of the positive integers less than or equal to 5. This is not much work for you to type and compute directly.

```
1 + 2 + 3 + 4 + 5

15
```

I used an f-string with a new kind of expression inside the braces. If you place an equal sign “=” after a variable, Python prints out both the variable’s name and value. The spaces around “=” are optional, but I included them for readability.

### Exercise 2.17

Rewrite the call to print to get an equivalent display, but only use {count} and {sum}.

### 2.11.2 break

The break keyword allows you to leave a loop immediately.

```
sum, count = 0, 1

while True:
    sum += count
    count += 1
    if count > 500:
        break

sum
```

```
125250
```

The code “while True” says, “the following code should be repeated indefinitely until something happens to stop the repetition.” The break is that “something.”

### Exercise 2.18

How many addition operations did we perform in this while loop?

I find while loops with the terminating condition and break buried in the code harder to read and debug than the first version above. Your aim should be that anyone who glances at your code can tell what it is doing as quickly as possible.

### 2.11.3 continue

The continue keyword tells Python, “stop what you are doing and return to the top of the loop at the point immediately before testing the condition.” This code adds the odd numbers less than or equal to 500:

```
sum, count = 0, 1

while count <= 500:
    if count % 2 == 0:
        count += 1
        continue

    sum += count
    count += 1

sum
```

```
62500
```

Note how I tested whether count was even with the remainder % operator.

### Exercise 2.19

How would you test for count being odd? How would you test for count being evenly divisible by 5?

### 2.11.4 for loops

A for loop is often much more concise than a while loop if you are iterating over a sequence of values. We often use the range function to deliver a sequence of integers starting with its first parameter and ending before and not including its second.

```
sum = 0

for count in range(1, 501):
    sum += count

sum
```

```
125250
```

We initialize sum to zero, and start count with the value one. This is the first parameter to range. We repeat the body of the for loop while count is less than 501, the second parameter to range.

We can use a third parameter to range, and that is the step. If you only provide one parameter, the starting int defaults to 0, the step defaults to 1, and the given number is the end. With two parameters, the step defaults to 1.

```
sum = 0

for count in range(1, 501, 2):
    sum += count

sum
```

```
62500
```

Here we iterated over the odd integers greater than or equal to 1 and less than 501.

### Exercise 2.20

The first parameter to range need not be less than the second, and the arguments may be any integers, including negative ones. Write a for loop that prints 5, 4, 3, 2, 1 on a single line. Start by printing the numbers in the stated order and then experiment with print.

Use for to iterate over items in a list or characters in a string.

```
for year in [2020, 2022, 2025, 2026]:
    print(year)
```

```
2020
2022
2025
2026
```

```
for character in "abc":
    print(character)
```

```
a
b
c
```

The expression range(0, x) is equivalent to range(x). Here we show the substrings of a given string beginning with the first character:

```
string = "Zdrasti"

for index in range(len(string)):
    print(string[0:index + 1])
```

```
Z
Zd
Zdr
Zdra
Zdras
Zdrast
Zdrasti
```

Study this example carefully because it pulls together several very useful features and functions of Python.

### Exercise 2.21

Change the example to show the substrings in the string ending with the final character.

One of my favorite parts of Python is how you can use a for loop to build a list. This example is a list comprehension and creates a list of the first five squares:

```
[x*x for x in range(1, 6)]
```

```
[1, 4, 9, 16, 25]
```

We return to these in section 3.5.

## 2.12 Functions

In this section, we combine many of the techniques in this chapter into examples of functions.

### 2.12.1 A simple function

This Python code is the definition of an *identity* function:

```
def identity(x):
    return x
```

*identity* takes one parameter x, does nothing to it, and returns it.

A function definition begins with def followed by the function name. After that, we list zero or more named parameters in parentheses. We end the line with a colon. The body of the function starts on the next line and is indented.

Because of the way I wrote *identity*, it doesn’t care what the type of x is. It is overloaded to work on anything.

```
identity(7)
```

```
7
```

```
identity("just a string")
```

```
'just a string'
```

```
identity(["one", 2, 3.0])
```

```
['one', 2, 3.0]
```

The definition of *identity* defined the *parameter* x. A parameter is a variable assigned the value of an *argument* when a function is called. In identity(7), the parameter x is assigned the argument 7.

Slightly more interesting is a function that “doubles” its input.

```
def double(x):
    return 2 * x
```

```
double(7)
```

```
14
```

To fix ideas, let’s start by thinking about a list of numbers. We want to manipulate the list to move the smallest number in the list to index 0. Before we write a function to do this, let’s write some test code. Here is the list:

```
numbers = [3, 2, 1, 4]
```

and here is the code to percolate the smallest number to the first position. Note the loop, the conditional, and the simultaneous assignment.

```
for index in range(1, 4):
    if numbers[0] > numbers[index]:
        numbers[0], numbers[index] = numbers[index], numbers[0]

numbers
```

```
[1, 3, 2, 4]
```

### Exercise 2.23

What are the 1 and the 4 in the range function call?

That worked as expected. Figure 2.9 shows how the code transformed the list. You can see how the smallest number, 1, is moved to the start of the list. Only two swaps were necessary, and I showed these with line segments with circles at their ends,

![](img/4ffeafa3e3f1373f02971e43b0755ea6_98_0.png)

Figure 2.9: Movement of the list items while sorting
