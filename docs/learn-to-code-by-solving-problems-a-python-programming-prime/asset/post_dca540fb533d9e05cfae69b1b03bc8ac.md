
```python
>>> title = 'The Escape'
>>> for char in title:
...    if char.isupper():
...        print(char)
T
E
```

Be careful with the indentation here. We need one level of indentation for the for loop, and an extra level of indentation for the nested if statement.

On the first iteration, char refers to `'T'`. Since `'T'` is uppercase, the `isupper` test returns `True`, and the if statement block runs. That results in the output of `T`. On the second iteration, char refers to `'h'`. This time, the `isupper` test returns `False`, so the if statement block doesn't run. Overall, the for loop loops through each character of the string, but the nested if statement fires only twice: on the `'T'` at the beginning of the string and on the `'E'` at the beginning of `'Escape'`.

What about a for loop nested in a for loop? We can do that! Here's an example:

```python
>>> letters = 'ABC'
>>> digits = '123'
>>> for letter in letters:
...    for digit in digits:
...        print(letter + digit)
A1
A2
A3
B1
B2
B3
C1
C2
C3
```

The code produces all two-character strings whose first character is from `letters` and whose second character is from `digits`.

On the first iteration of the outer (`letters`) loop, letter refers to `'A'`. This iteration involves completely running the inner (`digits`) loop. The whole time the inner loop runs, letter refers to `'A'`. On the first iteration of the inner loop, digit refers to `1`, which explains the A1 output. On the second iteration of the inner loop, digit refers to `2`, and A2 is output. On the third and final iteration of the inner loop, digit refers to `3`, and A3 is output.

We’re not done! We’ve gone through only one iteration of the outer loop. On the second iteration of the outer loop, letter refers to `'B'`. Now the three iterations of the inner loop run again, this time with letter referring to `'B'`. This accounts for the B1, B2, and B3 outputs. Finally, on the third iteration of the outer loop, letter refers to `'C'`, and the inner loop produces C1, C2, and C3.

## CONCEPT CHECK

What is the output of the following code?

```python
title = 'The Escape'
total = 0

for char1 in title:
    for char2 in title:
        total = total + 1

print(total)
```

- A. 10
- B. 20
- C. 100
- D. This code produces a syntax error because two nested loops cannot both use `title`

Answer: C. `total` starts off as 0 and is increased by 1 on each iteration of the inner loop. The length of `'The Escape'` is 10. The outer loop therefore has 10 iterations. For each of those iterations, the inner loop has 10 iterations. The inner loop therefore has 10*10 = 100 iterations in all.

## Solving the Problem

Back to Three Cups. The structure we need is a for loop to go through each swap, and a nested if statement to keep track of where the ball is:

```python
for swap_type in swaps:
    # Big if statement to keep track of the ball
```

There are three types of swaps (A, B, and C) and three possible locations for the ball, so it’s tempting to conclude that we have to write an if statement with 3 * 3 = 9 Boolean expressions (one after the if and one after each of eight `elif`s). In fact, we need only six Boolean expressions. Three of the nine don’t move the ball at all: swap type A when the ball is at the right, swap type B when the ball is at the left, and swap type C when the ball is at the middle.

Listing 3-1 has a solution to Three Cups.

```python
swaps = input()

ball_location = 1

❶ for swap_type in swaps:
    ❷ if swap_type == 'A' and ball_location == 1:
        ❸ ball_location = 2
    elif swap_type == 'A' and ball_location == 2:
        ball_location = 1
    elif swap_type == 'B' and ball_location == 2:
        ball_location = 3
    elif swap_type == 'B' and ball_location == 3:
        ball_location = 2
    elif swap_type == 'C' and ball_location == 1:
        ball_location = 3
    elif swap_type == 'C' and ball_location == 3:
        ball_location = 1

print(ball_location)
```

*Listing 3-1: Solving Three Cups*

I’ve used `input` to assign the string of swaps to the `swaps` variable. The for loop ❶ loops through these swaps. Each swap is processed by the nested if statement ❷. The if and elif branches each encode what happens with a given type of swap and a given ball location and then move the ball accordingly. For example, if the swap type is A and the ball is at location 1 ❷, then the ball ends up at location 2 ❸.

This is a code example where it matters whether we use multiple `elif`s (one big if statement) or multiple `if`s (multiple if statements). If we change the `elif`s to `if`s, then our code is no longer correct. Listing 3-2 shows the incorrect code.

```python
# This code is incorrect

swaps = input()

ball_location = 1

for swap_type in swaps:
    ① if swap_type == 'A' and ball_location == 1:
        ball_location = 2
    ② if swap_type == 'A' and ball_location == 2:
        ball_location = 1
    if swap_type == 'B' and ball_location == 2:
        ball_location = 3
    if swap_type == 'B' and ball_location == 3:
        ball_location = 2
    if swap_type == 'C' and ball_location == 1:
        ball_location = 3
    if swap_type == 'C' and ball_location == 3:
        ball_location = 1

print(ball_location)
```

*Listing 3-2: Solving Three Cups incorrectly*

If we say that the code is incorrect, we’re claiming that it fails at least one test case. Can you find a test case where this code produces the wrong answer?

Here’s one such test case:

```text
A
```

It may make sense to us that the ball can move at most once per swap. But Python robotically runs the code you have written, whether it matches what we expect or not. In this case, we have only one swap, so the ball should move at most once. On the first and only iteration of the for loop, Python checks the expression ①. It’s True, so Python sets ball_location to 2. Then, Python checks the expression ②. Because we just changed ball_location to 2, this expression is True! Python therefore sets ball_location to 1. The output of the program is 1 when it should be 2.

This is an example of a *logic error*: an error that causes a program to follow the wrong logic and produce the wrong answer. A common term for logic error is a *bug*. When programmers work through their code to fix bugs, it’s called *debugging*.

It often takes only a simple test case to demonstrate when a program is incorrect. When you’re trying to narrow down what’s going wrong with your code, don’t start with long test cases. Such test case results are hard to verify by hand and often set in motion complex execution paths from which we may learn very little. A small test case, by contrast, doesn’t cause our program to do much; if what it does is wrong, then we don’t have far to look for the culprit. Devising small, targeted test cases is not always easy. It’s a skill that you can hone through practice.

Submit our correct code to the judge, and then let’s move on.

Before continuing, you might like to try solving exercises 1 and 2 from “Chapter Exercises” on page 67.

## Problem #6: Occupied Spaces

We know how to loop through the characters of a string. But sometimes we need to know where we are in the string, not just the character that’s stored there. This problem is one such example.

This is DMOJ problem ccc18j2.

### The Challenge

You supervise a parking lot with *n* parking spaces. Yesterday, you recorded whether each parking space was occupied by a car or was empty. Today, you again recorded whether each parking space was occupied by a car or was empty. Indicate the number of parking spaces that were occupied on both days.

### Input

The input consists of three lines.

- The first line contains integer *n*, the number of parking spaces. *n* is between 1 and 100.
- The second line contains a string of *n* characters for yesterday’s information, one character for each parking space. A C indicates an occupied parking space (C for car), and a . indicates an empty parking space. For example, CC. means that the first two parking spaces were occupied and the third was empty.
- The third line contains a string of *n* characters for today’s information, in the same format as the second line.

### Output

Output the number of parking spaces that were occupied on both days.

### A New Kind of Loop

We could have up to 100 parking spaces, so you may not be surprised that a loop will show up here somewhere. The kind of for loop we learned when solving Three Cups can certainly loop through a string of parking-space information:

```python
>>> yesterday = 'CC.'
>>> for parking_space in yesterday:
...     print('The space is ' + parking_space)
...
The space is C
The space is C
The space is .
```

That tells us whether each space was occupied yesterday. But we also need to know whether each space was also occupied today.

Consider this test case:

```text
3
CC.
.C.
```

The first parking space was occupied yesterday. Was that parking space occupied on both days? To answer that, we need to look at the corresponding character in today’s string. It’s a . (empty), so this parking space was not occupied on both days.

What about the second parking space? That one was also occupied yesterday. And, looking at the second character of today’s string, it was also occupied today. So this is a parking space that was occupied on both days. (This is the only such parking space; the correct output for this test case is 1.)
