has no effect on the ball. The correct output is therefore 3, because the ball ends up at the right.

Notice that for each swap, we have to make a decision to determine whether the ball moves and, if it does, to move the ball appropriately. Making decisions is something we know how to do from Chapter 2. For example, if the swap type is A and the ball is at the left, then the ball moves to the middle. That looks like this:

```
if swap_type == 'A' and ball_location == 1:
    ball_location = 2
```

We could add an elif for each other case where the ball moves: swap type A and ball is at the middle, swap type B and ball is at the middle, swap type B and ball is at the right, and so on. This big if statement would be enough to handle one swap. But that’s not enough to solve the Three Cups problem, because we could have a test case of up to 50 swaps. We’d need to repeat the if statement logic for each swap. And we certainly wouldn’t want to copy and paste the same code 50 times. Imagine if you had made a typo and had to fix it 50 times. Or if you suddenly became interested in test cases with up to a million swaps. No, what we have learned so far is not going to cut it. We need a way to walk through the swaps, performing the same logic for each one. We need a loop.

# for Loops

Python’s for statement produces for loops. for loops allow us to process each element of a sequence. The only sequence type we’ve seen so far is the string. We’ll learn others as we go; for loops work on all of them.

Here’s our first example of a for loop:

```
>>> secret_word = 'olive'
>>> for char in secret_word:
...     print('Letter: ' + char)
...
Letter: o
Letter: l
Letter: i
Letter: v
Letter: e
```