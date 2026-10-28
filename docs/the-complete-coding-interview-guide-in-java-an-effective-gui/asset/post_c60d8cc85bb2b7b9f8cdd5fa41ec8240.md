
## Coding challenge 1 – Robot grid (I)

Adobe, Microsoft

Problem: We have an m x n grid. A robot is placed at the top-left corner of this grid. The robot can only move either right or down at any point in time, but it is not allowed to move in certain cells. The robot's goal is to find a path from the top-left corner to the bottom-right corner of the grid.

Solution: First, we need to set some conventions of the m x n grid. Let's assume that the bottom-right corner has the coordinates (0, 0), while the top-left corner has the coordinates (m, n), where m is the row and n is the column of the grid. So, the robot starts from (m, n) and must find a path to (0, 0). If we try to sketch an example for a 6x6 grid, then we can obtain something like the following:

![](img/6709a71136a6b327c831c6c15aff2f6a_210_0.png)

Figure 8.4 – Determining the moving pattern

Here, we can see that the robot can go from one cell (m, n) to an adjacent cell, which can be (m-1, n) or (m, n-1). For example, if the robot is placed at (5, 5), then it can go to (4, 5) or (5, 4). Furthermore, from (4, 5), it can go to (3, 5) or (4, 4), while from (5, 4), it can go to (5, 3) or (4, 4).

So, we have a problem that can be divided into sub-problems. We must find the final path for the cells (the problem), which we can do if we are able to find the path to an adjacent cell (sub-problem). This sounds like a recursive algorithm. In recursion, we approach the problem from top to down, so we start from (m, n) and move back to the origin (0, 0), as shown in the preceding diagram. This means that from cell (m, n), we try to go into (m, n-1) or (m-1, n).

Putting this into code can be done as follows (the maze[][] matrix is a boolean matrix that has values of true for cells that we are not allowed to go in – for example, maze[3][1] = true means that we are not allowed in cell (3,1)):

```
public static boolean computePath(int m, int n,
        boolean[][] maze, Set<Point> path) {

    // we fell off the grid so we return
    if (m < 0 || n < 0) {
        return false;
    }

    // we cannot step at this cell
    if (maze[m][n]) {
        return false;
    }

    // we reached the target
    // (this is the bottom-right corner)
    if ((m == 0) && (n == 0)
        // or, try to go to the right
        || computePath(m, n - 1, maze, path)
        // or, try to go to down
        || computePath(m - 1, n, maze, path)) {

        // we add the cell to the path
        path.add(new Point(m, n));

        return true;
    }

    return false;
}
```

The complete application is called *HanoiTowers*.

## Coding challenge 3 – Josephus

Amazon, Google, Adobe, Microsoft, Flipkart

**Problem:** Consider a group of *n* men arranged in a circle (1, 2, 3, ..., *n*). Every *k*th man will be killed around the circle until only one survivor remains. Write an algorithm that finds the *k* position of this survivor. This is known as the Josephus problem.

**Solution:** Remember that we had a note previously saying that when a problem contains the words *compute the nᵗʰ* and similar expressions, then it is possibly a good candidate for being solved via recursion. Here, we have *find the k position*, which is a problem that can be divided into sub-problems and be solved via recursion.

Let's consider *n*=15 and *k*=3. So, there are 15 men and every third man will be eliminated from the circle until only one remains. Let's visualize this via the following diagram (this is very useful for figuring out the pattern of killings):

![](img/6709a71136a6b327c831c6c15aff2f6a_215_0.png)

Figure 8.6 – Josephus for n=15 and k=3

So, we have five rounds until we find the survivor, as follows:

- **Round 1:** The first elimination is position 3; next, 6, 9, 12, and 15 are eliminated.
- **Round 2:** The first elimination is position 4 (1 and 2 are skipped, since position 15 was the last eliminated in round 1); next, 8 and 13 are eliminated.
- **Round 3:** The first elimination is position 2 (14 and 1 are skipped, since position 13 was the last eliminated in round 2); next, 10 and 1 are eliminated.
- **Round 4:** The first elimination position is 11, followed by position 7.
- **Round 5:** 14 is eliminated and 5 is the survivor.

Trying to identify a pattern or a recursive call can be done based on the following observations. After the first man (kth) is eliminated, n-1 men are left. This means that we call josephus(n - 1, k) to get the position of the n-1th man. However, notice that the position returned by josephus(n - 1, k) will take into account the position starting from k%n + 1. In other words, we have to adjust the position returned by josephus(n - 1, k) to obtain (josephus(n - 1, k) + k - 1) % n + 1. The recursive method is shown here:

```
public static int josephus(int n, int k) {
    if (n == 1) {
        return 1;
    } else {
        return (josephus(n - 1, k) + k - 1) % n + 1;
    }
}
```

If you find this approach quite tricky, then you can try an iterative approach based on a queue. First, fill up the queue with n men. Next, loop the queue and, for each man, retrieve and remove the head of this queue (poll()). If the retrieved man is not the kth, then insert this man back in the queue (add()). If this is the kth man, then break the loop and repeat this process until the queue's size is 1. The code for this is as follows:

```
public static void printJosephus(int n, int k) {
    Queue<Integer> circle = new ArrayDeque<>();
    for (int i = 1; i <= n; i++) {
        circle.add(i);
    }
    while (circle.size() != 1) {
        for (int i = 1; i <= k; i++) {
            int eliminated = circle.poll();
            if (i == k) {
                System.out.println("Eliminated: " + eliminated);
                break;
            }
        }
    }
```

```
circle.add(eliminated);
}
}

System.out.println("Using queue! Survivor: "
    + circle.peek());
}
```

The complete application is called Josephus.

## Coding challenge 4 – Color spots

Amazon, Google, Adobe, Microsoft, Flipkart

Problem: Consider an r x c grid where r stands for rows and c stands for columns. Each cell has a color represented by a number k (for example, for three colors, k=3). We define the connected set of a cell (or a color spot) as the total cells in which we can go from the respective cell by successive displacements on the row or the column, thus keeping the color. The goal is to determine the color and the number of cells of the maximum connected set. In other words, we need to determine the biggest color spot.

Solution: Let's consider a 5x5 grid and three colors, where we have r=c=5 and k=3. Next, let's represent the grid as shown in the following diagrams:

![](img/6709a71136a6b327c831c6c15aff2f6a_217_0.png)

Figure 8.7 – Biggest color spot ((a) – initial grid, (b) – solved grid)

Let's focus on image (a). Here, we can see that moving from a cell to another cell can be done in a maximum of four directions (up, down, left, and right). This means that, from a cell (r,c), we can try to go to (r-1, c), (r+1, c), (r, c-1), and (r, c+1). We cannot perform a move if we risk falling from the grid or the targeted cell has another color than the current cell. So, by iterating each cell ((0, 0), (0, 1), ... (r, c)), we can determine the size of the connected set of that cell (the size of the color spot) by visiting each allowed cell and counting it. In image (a), we have four spots that are color 1 whose sizes are 1, 1, 1, and 2. We also have six spots that are color 2 whose sizes are 1, 1, 2, 1, 1, and 1. Finally, we have three spots that are color 3 whose sizes are 11, 1, and 1.

From this, we can conclude that the biggest color spot has a size of 11 and a color of 3. Mainly, we can consider that the color spot of the first cell is the maximum spot and that each time we find a color spot bigger than this one, we replace this one with the one we found.

Now, let's focus on image (b). Why do we have negative values? Because when we visit a cell, we switch its color value to -color. This is a convenient convention that's used to avoid computing the same connected set of a cell multiple times. It is like saying that we mark this cell as visited. By convention, we cannot move in a cell that has a negative value for a color, so we will not compute the size of the same color spot twice.

Now, gluing these observations together to make a recursive method leads to the following code:

```
public class BiggestColorSpot {

    private int currentColorSpot;

    void determineBiggestColorSpot(int cols,
        int rows, int a[][]) {
        ...
    }

    private void computeColorSpot(int i, int j,
        int cols, int rows, int a[][], int color) {

        a[i][j] = -a[i][j];
        currentColorSpot++;

        if (i > 1 && a[i - 1][j] == color) {
```
