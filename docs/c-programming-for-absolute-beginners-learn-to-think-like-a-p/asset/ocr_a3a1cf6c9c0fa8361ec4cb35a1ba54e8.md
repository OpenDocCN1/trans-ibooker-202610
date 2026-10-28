# do-while Construction

To write the loop, you use the `do-while` construct.

The computer enters the loop after the word `do`, executes its statements, and asks “Once more?” If the condition after the `while` word holds, the computer returns to the beginning of the loop, in other words, after the `do` word. And so on.

The loop terminates at the moment when its condition after the `while` word is evaluated as unfulfilled (`false`).

# This Case

In this case, the program evaluates the entered password after each input. The evaluation result is then stored in a bool-typed variable called `ok`.

You want the loop to go on if the entered password is incorrect. That is why you use a negation operator (an exclamation mark) in the `while` condition.

# Variable Outside of the Loop

C# requires that all variables used in the loop condition be declared outside of the loop. When you declare them inside, they are not visible when specifying the condition.

# Tip

Visual Studio can help you with your do-while loops. Just enter do and press the Tab key twice.

# Waiting for Descend

Imagine the computer watches some quantity that grows most of the time, and the task is to detect the (possibly rare) moment when it lessens (descends).

You would usually encounter such a problem when digging through a large amount of data stored in a file or in a database. However, you will resolve this on data entered by the user.