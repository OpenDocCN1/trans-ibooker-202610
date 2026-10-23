In general, repeated code is a sign that we might be able to improve the code’s design. We’d like to avoid repeated code because it adds to the amount of code that we must maintain, and it makes it harder to fix problems in the code if it turns out that the repeated code is flawed. Here, the repeated code seems acceptable to me (it’s only three lines), but let’s talk about two ways to avoid it. These are general approaches that you’ll be able to apply to other similar problems.

# Using a Huge Size

The only reason we’re calculating the size of a village’s neighborhood before the loop is so that the loop has something to compare the other neighborhood sizes against. If we entered the loop without a value for `min_size`, we’d get an error when the code tries to compare it to the size of the current village.

If we set `min_size` to 0.0 before the loop, then the loop will never find a smaller size, and we’ll incorrectly output 0.0 no matter the test case. Using 0.0 would be a bug!

But a huge value, one at least as big as every possible neighborhood size, *will* work. We just need to make it so huge that the first iteration of the loop is guaranteed to find a size that’s no bigger, ensuring that our fake huge size never gets output.

From the “Input” section of this problem description, we know that each position is between −1,000,000,000 and 1,000,000,000. The biggest neighborhood we could ever have, then, occurs when we have a village at position −1,000,000,000, another at position 1,000,000,000, and a village somewhere in between. That in-between village will have a neighborhood size of 1,000,000,000. We can therefore start `min_size` with a size of 1000000000.0 or greater. This alternate approach is in Listing 5-2.

```
n = int(input())

positions = []

for i in range(n):
    positions.append(int(input()))

positions.sort()

min_size = 1000000000.0

for i in range(1, n - 1):
    left = (positions[i] - positions[i - 1]) / 2
```