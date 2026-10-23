
At the bottom of this window, you’ll see a >>> Python prompt. This is where you type Python code. Never type the >>> symbols yourself. Once you’re done programming, you can press CTRL-D to quit.

## Problem #1: Word Count

Now it’s time for our first problem! We’re going to use Python to write a little word-count program. We’ll learn how to read input from the user, process the input to solve the problem, and output the result. We’ll also learn how to manipulate text and numbers in our programs, make use of built-in Python operations, and store intermediate results on our way to the solution.

This is DMOJ problem dmopc15c7p2.

## The Challenge

Count the number of words provided. For this problem, a *word* is any sequence of lowercase letters. For example, hello is a word, but so are non-English “words” like bbaabbb.

## Input

The input is one line of text, consisting of lowercase letters and spaces. There is exactly one space between each pair of words, and there are no spaces before the first word or after the last word.

The maximum length of the line is 80 characters.

## Output

Output the number of words in the input line.

## Strings

*Values* are a fundamental building block of Python programs. Each value has a *type*, and the type determines the operations that can be performed on the value. In the Word Count problem, we’re working with a line of text. Text is stored as a string value in Python, so we’ll need to learn about strings. To solve the problem, we output the number of words in the text, so we also need to learn about numeric values. Let’s begin with strings.

## Representing Strings

A *string* is the Python type that’s used to store and manipulate text. To write a string value, we place its characters between single quotes. Follow along in the Python shell:

```python
>>> 'hello'
'hello'
>>> 'a bunch of words'
'a bunch of words'
```

The Python shell echoes each string that I’ve typed.

What happens when our string contains a single quote as one of its characters?

```python
>>> 'don't say that'
  File "<stdin>", line 1
    'don't say that'
           ^
SyntaxError: invalid syntax
```

The single quote in the word don’t terminates the string. The rest of the line, t say that’, therefore doesn’t make sense, and that’s what generates the syntax error. A *syntax error* means that we have violated the rules of Python and have not written valid Python code.

To fix this, we can take advantage of the fact that double quotes can also be used to delimit strings:

```python
>>> "don't say that"
"don't say that"
```

Unless the string in question has a single quote, I won’t use double quotes in this book.

## String Operators

We can use a string to hold the text whose words we want to count. To count the words—or to do anything else with strings—we need to learn how to work with strings.

Strings come with a rich variety of operations that we can perform. Some of them use special symbols between their operands. For example, the + operator is used for string concatenation:

produces the uppercase version of a string:

```python
>>> 'hello'.upper()
'HELLO'
```

The information we get back from a method is known as the method’s return value. For example, we could say for the previous example that upper returned the string 'HELLO'.

Performing a method on a value is known as calling the method. Calling a method involves placing the dot operator (.) between the value and the method name. It also requires parentheses after the method name. For some methods, we leave those parentheses empty, as when calling upper.

For other methods, we can optionally include information there. Still other methods require information and won’t work at all without it. Information we include when calling a method is referred to as the method’s arguments.

For example, strings have a strip method. If called with no arguments, strip removes all leading and trailing spaces from a string:

```python
>>> '  abc'.strip()
'abc'
>>> '  abc   '.strip()
'abc'
>>> 'abc'.strip()
'abc'
```

But we can also call it with a string as the argument. If we do, that argument determines which characters are stripped from the beginning and the end:

```python
>>> 'abc'.strip('a')
'bc'
>>> 'abca'.strip('a')
'bc'
>>> 'abca'.strip('ac')
'b'
```

Let’s talk about one more string method: count. We pass it a string argument, and it tells us how many occurrences of that argument are found in our string:

```python
>>> 'abc'.count('a')
1
>>> 'abc'.count('q')
0
>>> 'aaabcaa'.count('a')
5
>>> 'aaabcaa'.count('ab')
1
```

If occurrences of the argument overlap, only the first counts:

```python
>>> 'ababa'.count('aba')
1
```

Unlike the other methods I’ve described, count is directly useful to our Word Count problem.

Think of a string like 'this is a string with a few words'. Notice that a space comes after each word. In fact, if you had to count the number of words by hand, you might use the spaces to tell you where each word ends. What if we count the number of spaces in a string? To do that, we can pass a string consisting of a single space character to count. It looks like this:

```python
>>> 'this is a string with a few words'.count(' ')
7
```

We get a value of 7. That’s not quite the number of words—the string has eight words—but we’re close. Why are we getting 7 instead of 8?

The reason is that each word has a space after it except the last word. Counting the spaces therefore fails to account for the final word. To remedy that, we need to learn how to handle numbers.

## Integer and Floating-Point Numbers

An *expression* is made up of values and operators. We’ll now see how to write numeric values and combine them with operators.

There are two different Python types that represent numbers: integers (with no decimal part) and floating-point numbers (with a decimal part).

We write integer values as numbers with no decimal point. Here are some examples:

```python
>>> 30
30
>>> 7
7
>>> 1000000
1000000
>>> -9
-9
```

A value on its own is the simplest kind of expression.

The familiar mathematical operators work on integers. We have + for addition, - for subtraction, and * for multiplication. We can use these operators to write more complicated expressions.

```python
>>> 8 + 10
18
>>> 8 - 10
-2
>>> 8 * 10
80
```

Notice the spaces around the operators. While 8+10 and 8 + 10 are the same as far as Python is concerned, the latter makes the expression easier for us humans to read.

Python has two division operators, not one! The // operator performs integer division, which throws away any remainder and rounds the result down:

```python
>>> 8 // 2
4
>>> 9 // 5
1
>>> -9 // 5
-2
```

If you want the remainder of the division, use the mod operator, written as %. For example, dividing 8 by 2 leaves no remainder:

```python
>>> 8 % 2
0
```

Dividing 8 by 3 leaves a remainder of 2:

```python
>>> 8 % 3
2
```

The / operator, in contrast to //, doesn’t do any rounding:

```python
>>> 8 / 2
4.0
>>> 9 / 5
1.8
>>> -9 / 5
-1.8
```

These result values are not integers! They have a decimal point and belong to a different Python type called *float* (for “floating-point numbers”). You can write float values by including a decimal point:

```python
>>> 12.5 * 2
25.0
```

We’ll focus on integers for now and return to floating-point numbers when we solve Cone Volume later in this chapter.

When we use multiple operators in an expression, Python uses precedence rules to determine the order that operators are applied. Each operator has a precedence. Just like when we evaluate a mathematical expression on paper, Python performs multiplications and divisions (higher precedence) before additions and subtractions (lower precedence):

```python
>>> 50 + 10 * 2
70
```

Again, like on paper, operations inside parentheses have the highest precedence. We can use this to force Python to perform operations in our desired order:

```python
>>> (50 + 10) * 2
120
```

Programmers often add parentheses even when not technically required. That’s because Python has many operators, as we’ll see, and keeping track of their precedence is error-prone and not something that programmers typically do.

If you’re wondering whether integer values and float values have methods, just like strings, they do! But they aren’t all that useful. For example, there’s a method that tells us how much of the computer’s
