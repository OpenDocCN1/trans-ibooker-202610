```
leftmost_index[color] = i
rightmost_index[color] = i
⑤ else:
    rightmost_index[color] = i

max_length = 0

for i in range(m):
    relative = input().split()
    first = int(relative[0])
    last = int(relative[1])
    if first in leftmost_index and last in leftmost_index:
        ⑥ length = rightmost_index[last] - leftmost_index[first] + 1
        if length > max_length:
            max_length = length

print(max_length)
```

Listing 10-1: Solving Longest Scarf, algorithm 2

This solution uses two dictionaries: one to keep track of the leftmost index for each color ① and one to keep track of the rightmost index for each color ②.

As promised, we look at each foot of the scarf just once ③. Here’s how we keep the leftmost_index and rightmost_index dictionaries up-to-date:

+   - If the color of the current foot has never been seen before ④, then the current index serves as both the leftmost and rightmost index for this color.

+   - Otherwise, the color of the current foot has been seen before ⑤. We don’t want to update the leftmost index for this color, because the current index is to the right of the old one. We do want to update the rightmost index, though, because we have found an index to the right of the old one.

Now for the payoff: for each relative, we can simply look up the leftmost and rightmost indices from these dictionaries ⑥. The maximum length of the desired scarf is the rightmost index of the color of the last foot, minus the leftmost index of the color of the first foot, plus one.

As I’ll argue now, this algorithm is far better than algorithm 1. Reading the scarf takes O(n) time, as does processing the scarf’s feet. That’s O(n) time so far. We then take a constant number of steps to process each relative (not n steps like before!), so that’s O(m) time. In