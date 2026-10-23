```
...    where = bisect_left(lst, 1000000)
...
```

On my computer, that takes about a second. You might be wondering what would happen if you replaced the binary search with a call to the list index method. If you try it, you’ll literally wait hours for the code to run. That’s because index, like the in operator, does a linear search through the list. (See “Efficiency of Searching a List” in Chapter 8 for more on this.) It has no guarantee that the list is sorted, so it can’t perform a blazing-fast binary search. It has to go through the values one by one, comparing each of them to the value we’re searching for. If you have a sorted list and you want to find values in it, binary search is unstoppable.

# Solving the Problem

We’re ready to solve Cow Baseball using binary search. See Listing 9-7 for the code.

```
① from bisect import bisect_left, bisect_right

input_file = open('baseball.in', 'r')
output_file = open('baseball.out', 'w')

n = int(input_file.readline())

positions = []

for i in range(n):
    positions.append(int(input_file.readline()))

positions.sort()

total = 0

for i in range(n):
    for j in range(i + 1, n):
        first_two_diff = positions[j] - positions[i]
        low = positions[j] + first_two_diff
        high = positions[j] + first_two_diff * 2
        ② left = bisect_left(positions, low)
        ③ right = bisect_right(positions, high)
        total = total + right - left

output_file.write(str(total) + '\n')
```