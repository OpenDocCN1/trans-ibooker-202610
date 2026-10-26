# Input

+   - The first line contains a single integer, N, which is the height and width of the course.
+   - The next N² - 1 lines each contain the directions string and an integer called power. Each set of N lines corresponds to a single row, beginning from the top of the grid, where each cell's data is defined from left to right (for example, in a 3 x 3 grid, 0 -> [0, 0], 1 -> [0, 1], 2 -> [0, 2], 3 -> [1, 0], 4 -> [1, 1], and so on).
+   - directions contains 0-3 characters from the set { 'N', 'S', 'E', 'W' }, which represent the cells that your robot can visit from each point. Thus, if the directions string is SW, then the robot can move south or west from that point. power represents the energy expenditure required to cross the cell. Positive values for power indicate that a charging station is located within the cell.

# Output

+   - If traversing the course causes the robot to explode, print a single line – TRAVERSAL ABORTED.
+   - Otherwise, print the maximum amount of energy your robot can have upon reaching the bottom-right cell of the course, relative to the amount of energy it started with. For example, if the robot can finish the maze with 10 more units of energy than it started with, print 10; if it finishes the maze with 10 fewer units of energy than it started with, print -10.

# Example

Let's say we had the following input:

```
3
SE -10
SE -8
S -6
S 7
E -10
S 20
E -1
NE 5
```