
From a safe cell of coordinates (r, c), we can move in four safe directions: (r-1, c), (r, c-1), (r+1, c), and (r, c+1). If we think of the possible movements as directions (edges) and the cells as vertices, then we can visualize this problem in the context of a graph. The edges are the possible moves, while the vertices are the possible cells where we can go. Each move holds the distance from the current cell to the start cell (a start cell is a cell from the first column). For each move, the distance is increased by 1. So, in the context of a graph, the problem reduces to finding the shortest path in a graph. Hence, we can use the **Breadth-first Search (BFS)** approach to solve this problem. In *Chapter 13, Trees and Graphs*, you were provided with a description of the BFS algorithm, and another problem was solved in the same manner as the one being solved here – the *Chess Knight* problem.

Now, based on the experience provided by the previous problem, we can elaborate on this algorithm:

- 1. Start with an empty queue.
- 2. Enqueue all the safe cells of the first column and set their distances to 0 (here, 0 represents the distance from each cell to itself). Moreover, these cells are marked as visited or flagged.
- 3. As long as the queue is not empty, do the following:
- a. Pop the cell representing the top of the queue.
- b. If the popped cell is the destination cell (that is, it is on the last column), then simply return its distance (the distance from the destination cell to the source cell on the first column).
- c. If the popped cell is not the destination then, for each of the four adjacent cells of this cell, enqueue each valid cell (safe and unvisited) into the queue with distance (+1) and mark it as visited.
- d. If we processed all the cells in the queue without reaching the destination, then there is no solution. Return -1.

Since we rely on the BFS algorithm, we know that all the cells whose shortest paths are 1 are visited first. Next, the visited cells are the adjacent cells who have the shortest paths as 1+1=2 and so on. So, the cell that has the shortest path is equal to the shortest path of its parent + 1. This means that, when we traverse the target cell for the first time, gives us the final result. This is the shortest path. Let's see the most relevant part of the code for this:

```
private static int findShortestPath(int [] [] board) {
	// stores if cell is visited or not
	boolean[][] visited = new boolean[M][N];
	Queue<Cell> queue = new ArrayDeque<>();
	// process every cell of first column
	for (int rl = 0; rl < M; rl++) {
		// if the cell is safe, mark it as visited and
		// enqueue it by assigning it distance as 0 from itself
		if (board[rl][0] == 1) {
			queue.add(new Cell(rl, 0, 0));
			visited[rl][0] = true;
		}
	}
	while (!queue.isEmpty()) {
		// pop the front node from queue and process it
		int rIdx = queue.peek().r;
		int cIdx = queue.peek().c;
		int dist = queue.peek().distance;
		queue.poll();
		// if destination is found then return minimum distance
		if (cIdx == N - 1) {
			return (dist + 1);
		}
		// process all adjacent cells
		for (int i = 0; i < 4; i++) {
			int nRow = rIdx + dr[i];
			int nCol = cIdx + dc[i];
			if (nRow >= 0 && nRow < M && nCol >= 0 && nCol < N && board[nRow][nCol] == 1 && !visited[nRow][nCol]) {
				queue.add(new Cell(nRow, nCol, dist + 1));
				visited[nRow][nCol] = true;
			}
		}
	}
	return -1;
}
```

If we have three operators, a, b, and c, we can write the expressions shown in the following image:

| Infix | Postfix | Prefix |
|-------|---------|--------|
| (a + b) * c | a b + c * | * a b c |
| a + (b * c) | a b c * + | + a * b c |

Figure 12.10 – Infix, postfix, and prefix

The most common problems refer to evaluating prefix and postfix expressions and converting between prefix, infix, and postfix expressions. All these problems have solutions that rely on stacks (or binary trees) and are covered in any serious book dedicated to fundamental algorithms. Take your time and harvest some resources about this topic to get familiar with it. Since this topic is widely covered in dedicated books and is not a common topic in interviews, we will not cover it here.

## Summary

This chapter covered the must-know stack and queue problems for any candidate that is preparing for a Java developer technical interview. Stacks and queues occur in many real applications, so mastering them is one of the top skills that an interviewer will test you on.

In the next chapter, Trees, Tries, and Graphs, you'll see that stacks and queues are frequently used for solving problems that involve trees and graphs as well, which means they also deserve your attention.

## General tree

Roughly speaking, we can categorize trees into binary trees and the rest of the allowed trees. A binary tree is a tree in which each node has up to two children. In the following diagram, the left-hand side image is of a non-binary tree, while the right-hand side image is of a binary tree:

![](img/6709a71136a6b327c831c6c15aff2f6a_493_0.png)

> Figure 13.2 – Non-binary tree versus binary tree

In terms of code, a binary tree can be shaped as follows (this implementation is used later in the Coding challenges section, so keep this in mind):

```
private class Node {
  private Node left;
  private Node right;
  private final T element;

  public Node(T element) {
    this.element = element;
    this.left = null;
    this.right = null;
  }

  public Node(Node left, Node right, T element) {
    this.element = element;
    this.left = left;
    this.right = right;
  }
}
```

```
// operations
}
```

As you can see, each Node keeps references to two other Node elements, as well as a generic data (element). The left and right nodes represent the children of the current node. Most tree coding challenges that are encountered in interviews use binary trees, so they deserve special attention. Binary trees can be categorized as follows.

## Knowing binary tree traversal

Before attending a technical interview, you must know how to traverse a binary tree. Often, traversing a binary tree will not be a problem in itself, but you have to be comfortable with the Breadth-first Search (BFS) and the Depth-first Search (DFS) algorithms, along with their three variations: Pre-Order, In-Order, and Post-Order. The following diagram represents the result of each traversal type:

![](img/6709a71136a6b327c831c6c15aff2f6a_494_0.png)

Let’s have a brief overview of the BFS and DFS algorithms.

## Breadth-first Search (BFS) for trees

BFS for trees is also referred as Level-Order traversal. The main idea is to maintain a queue of nodes that will ensure the order of traversal. Initially, the queue contains only the root node. The steps of the algorithm are as follows:

- 1. Pop the first node from the queue as the current node.
- 2. Visit the current node.
- 3. If the current node has a left node, then enqueue that left node.
- 4. If the current node has a right node, then enqueue that right node.
- 5. Repeat from step 1 until the queue is empty.

In terms of code, we have the following:

```
private void printLevelOrder(Node node) {
    Queue<Node> queue = new ArrayDeque<>();
    queue.add(node);

    while (!queue.isEmpty()) {
        // Step 1
        Node current = queue.poll();

        // Step 2
        System.out.print(" " + current.element);

        // Step 3
        if (current.left != null) {
            queue.add(current.left);
        }

        // Step 4
        if (current.right != null) {
            queue.add(current.right);
        }
    }
}
```

Next, let's focus on DFS.

## Depth-first Search (DFS) for trees

DFS for trees has three variations: Pre-Order, In-Order, and Post-Order.

Pre-Order traversal visits the current node before its child nodes, as follows (root | left sub-tree | right sub-tree):

```
private void printPreOrder(Node node) {
    if (node != null) {
        System.out.print(" " + node.element);
        printPreOrder(node.left);
```
