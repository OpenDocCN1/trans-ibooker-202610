At the bottom of this window, you’ll see a >>> Python prompt. This is where you type Python code. Never type the >>> symbols yourself. Once you’re done programming, you can press CTRL-D to quit.

# Problem #1: Word Count

Now it’s time for our first problem! We’re going to use Python to write a little word-count program. We’ll learn how to read input from the user, process the input to solve the problem, and output the result. We’ll also learn how to manipulate text and numbers in our programs, make use of built-in Python operations, and store intermediate results on our way to the solution.

This is DMOJ problem dmopc15c7p2.

# The Challenge

Count the number of words provided. For this problem, a *word* is any sequence of lowercase letters. For example, hello is a word, but so are non-English “words” like bbaabbb.

# Input

The input is one line of text, consisting of lowercase letters and spaces. There is exactly one space between each pair of words, and there are no spaces before the first word or after the last word.

The maximum length of the line is 80 characters.

# Output

Output the number of words in the input line.

# Strings

*Values* are a fundamental building block of Python programs. Each value has a *type*, and the type determines the operations that can be performed on the value. In the Word Count problem, we’re working with a line of text. Text is stored as a string value in Python, so we’ll need to learn about strings. To solve the problem, we output the number of words in the text, so we also need to learn about numeric values. Let’s begin with strings.