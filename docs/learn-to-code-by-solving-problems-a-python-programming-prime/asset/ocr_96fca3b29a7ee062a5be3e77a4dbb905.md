Lena has n unopened boxes of action figures. The boxes cannot be opened (otherwise the action figures lose their value), so the order of action figures in a box cannot be changed. Further, a box cannot be rotated (otherwise the action figures will be facing the wrong way).

Each action figure is specified by its height. For example, one of the boxes might have three action figures, from left to right, of heights 4, 5, and 7. When I talk about a box of action figures, I’ll always list the heights from left to right.

Lena wants to organize the boxes, which means to arrange the boxes so that heights of action figures increase or stay the same from left to right.

Whether she can organize the boxes or not depends on the heights of action figures in the boxes. For example, if a first box has action figures of heights 4, 5, and 7, and a second box has action figures of heights 1 and 2, then she can organize these boxes by putting the second box first. But if we keep the first box as is and change the second box to have action figures of heights 6 and 8, then there’s no way to organize these boxes.

Determine whether it’s possible for Lena to organize the boxes.

# Input

The input consists of the following lines:

+   - A line containing integer n, the number of boxes. n is between 1 and 100.
+   - n lines, one for each box. Each of these lines begins with integer k, indicating the number of action figures in this box. k is between 1 and 100. (Since k is at least 1, we don’t have to worry about empty boxes.) Following k, there are k integers giving the heights of the action figures from left to right in this box. Each height is an integer between 1 and 10,000. There is a space between each pair of integers on the line.

# Output

If Lena can organize the boxes, output YES; otherwise, output NO.

# Representing the Boxes