The left variable stores the size of the left part of the neighborhood, and right stores the size of the right part. We then add them up to obtain the total size of the neighborhood. We get a value of 7.0.

That’s the value to beat. How do we know whether any other village has a smaller neighborhood? We can use a loop to process those other villages. If we find a neighborhood that’s smaller than our current smallest, we update our current smallest to that smaller size.

The code for our solution is in Listing 5-1.

```
n = int(input())

❶ positions = []

❷ for i in range(n):
    ❸ positions.append(int(input()))

❹ positions.sort()

❺ left = (positions[1] - positions[0]) / 2
  right = (positions[2] - positions[1]) / 2
  min_size = left + right

❻ for i in range(2, n - 1):
      left = (positions[i] - positions[i - 1]) / 2
      right = (positions[i + 1] - positions[i]) / 2
      size = left + right
      ❼ if size < min_size:
          min_size = size

print(min_size)
```

*Listing 5-1: Solving Village Neighborhood*

We begin by reading n, the number of villages, from the input. We also set positions to refer to an empty list ❶.

Each iteration of the first range for loop ❷ is responsible for reading one village position and appending it to the positions list. It does that by using input to read the next village position, int to convert it to an integer, and the list method append to append that integer to the list ❸. That one line ❸ is equivalent to these three separate lines:

```
position = input()
position = int(position)
positions.append(position)
```