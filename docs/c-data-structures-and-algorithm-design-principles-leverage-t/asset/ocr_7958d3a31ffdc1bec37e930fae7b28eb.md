The primary difference between those examples and the subset sum problem is that the minimal way to represent each state uniquely in the latter requires two dimensions – the index in the set and the current sum.

Let's consider some of the insights we have gained about this problem in greater depth:

+   - Each possible subset of size k can be formed by taking new elements and appending them onto every subset of size k - 1.
+   - If a solution has been found at index i with a sum value of x, then any sequence of state transitions that eventually lead to that same set of conditions will produce an identical result:

set = { 2 5 6 3 7 20 }
target = 28

|i:| 0 | 1 | 2 | 3 | 4 | 5 |
|Sum:| 2 {2} -> {2} -> {2 6} -> {2 6} -> {2 6} -> {2 6 20} TRUE |
|Sum:| - 5 {5} -> {5} -> {5 3} -> {5 3} -> {5 3 20} TRUE |

Figure 8.7: Multiple paths with the same sum value on the same index value

Both of these recursive paths have a sum value equal to 8 and an index value equal to 3 at the states indicated in red which, due the optimal substructure of the subset sum problem, means that the solution for that state only needs to be found once – its result will be the same any time those conditions are arrived at, regardless of what occurred before.

With these facts in mind, we can essentially invert our top-down approach to develop the bottom-up approach.

# Top-down logic:

+   1. Start at the target sum and the first index of the set.
+   2. Iterate through the set:
+      - If the sum is reduced to zero, the result is TRUE.
+      - If the end of the set is reached or the target is exceeded, the result is FALSE.
+      - Otherwise, you can either subtract the current value from the sum or ignore it.
+   3. If the target can be found from state S, where the sum is equal to x and the index is equal to i, then the target can also be found from any earlier state that eventually leads to the state S.