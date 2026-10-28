
![](img/6709a71136a6b327c831c6c15aff2f6a_629_0.png)

Figure 14.21 – Sorting the stack in place (1)

Next, we push back into the stack as long as the current element to push is smaller than the current top of the stack or the stack is empty. So, we will push 1, 2, and 8. We don’t push 3 (the next element to be pushed) since 3 is less than 8 (you can see this statement in the following diagram as action 1). At this point, we need to make room to 3, so we must pop the top of the stack, 8 (you can see this statement in the following diagram as action 2). Finally, we push 3 and, afterward, we push 8 into the stack (you can see this statement in the following diagram as action 3):

![](img/6709a71136a6b327c831c6c15aff2f6a_629_1.png)

Figure 14.22 – Sorting the stack in place (2)

So far, so good! Next, we must repeat the flow presented in the preceding diagram. So, the next element to be pushed from the recursion call stack into the given stack is 5. But 5 is less than 8, so we cannot push it (you can see this statement in the following diagram as action 1). At this point, we need to make room for 5, so we have to pop the top of the stack, which is 8 (you can see this statement in the following diagram as action 2). Finally, we push 5 and, afterward, we push 8 into the stack (you can see this statement in the following diagram as action 3):

![](img/6709a71136a6b327c831c6c15aff2f6a_630_0.png)

Finally, the last element that should be pushed from the recursion call stack into the given stack is 4. However, 4 is less than 8, so we cannot push it (you can see this statement in the following diagram as action 1). At this point, we need to make room to 4, so we must pop the top of the stack, which is 8 (you can see this statement in the following diagram as action 2). However, we still cannot push 4 into the stack because 4 is less than 5 (the new top element after popping 8). We must pop 5 as well (you can see this statement in the following diagram as action 3). Now, we can push 4. Next, we push 5 and 8. You can see this in the following diagram as action 4:

![](img/6709a71136a6b327c831c6c15aff2f6a_630_1.png)

Done! The given stack has been sorted. Let's see the code:

```
public static void sort(Stack<Integer> stack) {
	// stack is empty (base case)
	if (stack.isEmpty()) {
		return;
	}
	// remove the top element
	int top = stack.pop();
	// apply recursion for the remaining elements in the stack
	sort(stack);
	// insert the popped element back in the sorted stack
	sortedInsert(stack, top);
}

private static void sortedInsert(
	Stack<Integer> stack, int element) {
	// the stack is empty or the element
	// is greater than all elements in the stack (base case)
	if (stack.isEmpty() || element > stack.peek()) {
		stack.push(element);
		return;
	}
	// the element is smaller than the top element,
	// so remove the top element
	int top = stack.pop();
	// apply recursion for the remaining elements in the stack
	sortedInsert(stack, element);
	// insert the popped element back in the stack
	stack.push(top);
}
```

So, if we see the given matrix as an array, then we can reduce the problem of applying the Binary Search to a sorted array. There is no need to physically transform the matrix into an array. All we need to do is express the Binary Search accordingly using the following statements:

- The left-most integer of the array is at index 0 (let's denote it as *left*).
- The right-most integer of the array is at index (*rows* * *cols*) - 1 (let's denote it as *right*).
- The middle point of the array is at (*left* + *right*) / 2.
- The integer at the middle point of the index is at *matrix[mid / cols][mid % cols]*, where *cols* is the number of columns in the matrix.

With these statements in place, we can write the following implementation:

```
public static boolean search(int[][] matrix, int element) {
    int rows = matrix.length;      // number of rows
    int cols = matrix[0].length; // number of columns

    // search space is an array as [0, (rows * cols) - 1]
    int left = 0;
    int right = (rows * cols) - 1;

    // start binary search
    while (left <= right) {
        int mid = (left + right) / 2;
        int midElement = matrix[mid / cols][mid % cols];

        if (element == midElement) {
            return true;
        } else if (element < midElement) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
}
```
```
return false;
}
```

The preceding code performs in O(log mn) time, where m is the number of rows and n is the number of columns in the given matrix. The application is called SearchInFullSortedMatrix.

## Coding challenge 11 – Searching in a sorted matrix

Amazon, Microsoft, Flipkart

Problem: Consider you’ve been given a sorted matrix of integers of size rows x cols. Each row and each column are sorted in ascending order. Write a snippet of code that returns true if a given integer is in this matrix.

Solution: Notice that this problem is not like the previous coding challenge since the first integer of each row doesn’t have to be greater than the last integer of the previous row. If we apply the Binary Search algorithm (as we did for the previous coding challenge), then we must apply it to every row. Since Binary Search has a complexity time of O(log n) and we have to apply it to every row, this means that this approach will perform in O(m log n) time, where m is the number of rows and n is the number of columns in the given matrix.

![](img/6709a71136a6b327c831c6c15aff2f6a_634_0.png)

Let's assume that we search for the element 80, which can be found at (2, 3). Let's try to deduce this position. The climax of this deduction orbits the fact that the matrix has sorted rows and columns. Let's analyze the start of the columns: if the start of a column is greater than 80 (for example, column 4), then we know that 80 cannot be in that column, since the column is the minimum element in that column. Moreover, 80 cannot be found in any of the columns to the right of that column since the start element of each column must increase in size from left to right. Furthermore, we can apply the same logic to rows. If the start of a row is greater than 80, then we know that 80 cannot be in that row or subsequent (downward) rows.

Now, if we look at the end of the columns and rows, we can deduce some similar conclusions (mirrored conclusions). If the end of a column is less than 80 (for example, column 2), then we know that 80 cannot be in that column since the end of the column is the maximum element in that column. Moreover, 80 cannot be found in any of the columns to the left of that column since the start element of each column must decrease in size from right to left. Furthermore, we can apply the same logic to rows. If the end of a row is less than 80, then we know that 80 cannot be in that row or subsequent (upward) rows.

If we join, synthesize, and generalize these conclusions for an element, p, then we can deduce the following:

- If the start of a column is greater than p, then p must be to the left of that column.
- If the start of a row is greater than p, then p must be above that row.
- If the end of a column is less than p, then p must be to the right of that column.
- If the end of a row is less than p, then p must be below that row.

This is already starting to look like an algorithm. There is one more thing that we must decide, though. Where do we start from? From which row and column? Fortunately, we have several options. For example, we can start with the greatest column (0, last column) and work to the left of the same row, or with the greatest row (last row, 0) and work up on the same column.

Let’s assume that we choose to start with the greatest column (0, last column) and work to the left to find the element, p. This means that our flow will be as follows (let’s denote i=0 and j=cols-1):

- 1. If matrix[i][j] > p, then move left in the same row. The elements in this column are definitely greater than matrix[i][j] and hence, by extension, greater than p. So, we discard the current column, decrease j by 1, and repeat.
- 2. If matrix[i][j] < p, then move down in the same column. The elements in this row are definitely less than matrix[i][j] and hence, by extension, less than p. So, we discard the current row, increase i by 1, and repeat.
- 3. If p is equal to matrix[i][j], return true.
