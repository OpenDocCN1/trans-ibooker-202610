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

[1, 3, 2, 4]

# Exercise 2.23

What are the 1 and the 4 in the range function call?

That worked as expected. Figure 2.9 shows how the code transformed the list. You can see how the smallest number, 1, is moved to the start of the list. Only two swaps were necessary, and I showed these with line segments with circles at their ends,

![](bbox=[0.28, 0.62, 0.78, 0.86])

Figure 2.9: Movement of the list items while sorting