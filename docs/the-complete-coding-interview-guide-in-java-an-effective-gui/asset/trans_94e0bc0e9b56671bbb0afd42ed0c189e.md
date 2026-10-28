

## 编程挑战 1 – 机器人网格 (I)

Adobe, Microsoft

问题：我们有一个 m x n 网格。机器人被放置在这个网格的左上角。机器人只能在任何时候向右或向下移动，但不允许进入某些单元格。机器人的目标是从左上角找到一条路径到网格的右下角。

解决方案：首先，我们需要设定 m x n 网格的一些约定。假设右下角的坐标为 (0, 0)，而左上角的坐标为 (m, n)，其中 m 是行，n 是列。于是，机器人从 (m, n) 开始，必须找到一条到 (0, 0) 的路径。如果我们为 6x6 网格绘制一个示例，那么我们可以得到如下所示的内容：

![](img/6709a71136a6b327c831c6c15aff2f6a_210_0.png)

图 8.4 – 确定移动模式

这里，我们可以看到机器人可以从一个单元格 (m, n) 移动到相邻单元格，可以是 (m-1, n) 或 (m, n-1)。例如，如果机器人被放置在 (5, 5)，那么它可以移动到 (4, 5) 或 (5, 4)。此外，从 (4, 5)，它可以移动到 (3, 5) 或 (4, 4)，而从 (5, 4)，它可以移动到 (5, 3) 或 (4, 4)。

因此，我们有一个可以分解为子问题的问题。我们必须找到单元格的最终路径（问题），如果我们能够找到到相邻单元格的路径（子问题），就可以做到。这听起来像是一个递归算法。在递归中，我们从上到下处理问题，因此我们从 (m, n) 开始并向原点 (0, 0) 回溯，如前面的图表所示。这意味着从单元格 (m, n)，我们尝试进入 (m, n-1) 或 (m-1, n)。

将其写入代码可以如下完成（maze[][] 矩阵是一个布尔矩阵，对于我们不允许进入的单元格，其值为 true – 例如，maze[3][1] = true 意味着我们不允许进入单元格 (3,1)）：

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

完整的应用程序称为 *HanoiTowers*。

## 编程挑战 3 – 约瑟夫环

Amazon, Google, Adobe, Microsoft, Flipkart

**问题：** 考虑一组 *n* 个男人排成一个圆圈 (1, 2, 3, ..., *n*)。每隔 *k* 个男人就会被杀死，直到只剩下一个幸存者。编写一个算法来找到这个幸存者的 *k* 位置。这就是约瑟夫问题。

**解决方案：** 记住我们之前说过，当一个问题包含 *compute the nᵗʰ* 之类的词时，那么它可能是通过递归求解的良好候选。这里，我们有 *find the k position*，这是一个可以分解为子问题并通过递归求解的问题。

让我们考虑 *n*=15 和 *k*=3。于是，有 15 个男人，每第三个男人将被从圆圈中淘汰，直到只剩下一个。让我们通过以下图表来可视化这一点（这对于弄清楚杀戮模式非常有用）：

![](img/6709a71136a6b327c831c6c15aff2f6a_215_0.png)

图 8.6 – n=15 且 k=3 的约瑟夫环

于是，我们有五轮直到找到幸存者，如下所示：

- **第 1 轮：** 第一次淘汰是位置 3；接下来，6、9、12 和 15 被淘汰。
- **第 2 轮：** 第一次淘汰是位置 4（1 和 2 被跳过，因为第 1 轮最后淘汰的是位置 15）；接下来，8 和 13 被淘汰。
- **第 3 轮：** 第一次淘汰是位置 2（14 和 1 被跳过，因为第 2 轮最后淘汰的是位置 13）；接下来，10 和 1 被淘汰。
- **第 4 轮：** 第一次淘汰位置是 11，随后是位置 7。
- **第 5 轮：** 14 被淘汰，5 是幸存者。

尝试识别模式或递归调用可以根据以下观察来完成。在第一个男人（第 k 个）被淘汰后，剩下 n-1 个男人。这意味着我们调用 josephus(n - 1, k) 来获取第 n-1 个人的位置。然而，请注意，josephus(n - 1, k) 返回的位置将考虑到从 k%n + 1 开始的位置。换句话说，我们必须调整 josephus(n - 1, k) 返回的位置以获得 (josephus(n - 1, k) + k - 1) % n + 1。递归方法如下所示：

```
public static int josephus(int n, int k) {
    if (n == 1) {
        return 1;
    } else {
        return (josephus(n - 1, k) + k - 1) % n + 1;
    }
}
```

如果你觉得这种方法相当棘手，那么你可以通过基于队列的迭代方法来尝试。首先，用 n 个男人填充队列。接下来，循环队列，对于每个男人，检索并移除该队列的头部（poll()）。如果检索到的男人不是第 k 个，则将该男人重新插入队列（add()）。如果这是第 k 个男人，则中断循环并重复此过程，直到队列大小为 1。该代码如下：

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

完整的应用程序称为 Josephus。

## 编程挑战 4 – 彩色斑点

Amazon, Google, Adobe, Microsoft, Flipkart

问题：考虑一个 r x c 网格，其中 r 代表行，c 代表列。每个单元格都有一个由数字 k 表示的颜色（例如，对于三种颜色，k=3）。我们定义单元格的连通集（或彩色斑点）为我们可以通过在行或列上进行连续位移从相应单元格到达的总单元格数，同时保持颜色。目标是确定最大连通集的颜色和单元格数。换句话说，我们需要确定最大的彩色斑点。

解决方案：让我们考虑一个 5x5 网格和三种颜色，其中 r=c=5 且 k=3。接下来，让我们按以下图表表示网格：

![](img/6709a71136a6b327c831c6c15aff2f6a_217_0.png)

图 8.7 – 最大的彩色斑点（(a) – 初始网格，(b) – 已解决网格）

让我们关注图像 (a)。这里，我们可以看到从一个单元格移动到另一个单元格最多可以在四个方向进行（上、下、左、右）。这意味着，从一个单元格 (r,c)，我们可以尝试移动到 (r-1, c)、(r+1, c)、(r, c-1) 和 (r, c+1)。如果我们有掉出网格的风险或目标单元格的颜色与当前单元格不同，则我们不能执行移动。因此，通过迭代每个单元格（(0, 0)、(0, 1)、...(r, c)），我们可以确定该单元格的连通集的大小（彩色斑点的大小），通过访问每个允许的单元格并计数。在图像 (a) 中，我们有四个颜色为 1 的斑点，其大小分别为 1、1、1 和 2。我们还有六个颜色为 2 的斑点，其大小分别为 1、1、2、1、1 和 1。最后，我们有三个颜色为 3 的斑点，其大小分别为 11、1 和 1。

从中，我们可以得出结论，最大的彩色斑点大小为 11，颜色为 3。主要是，我们可以认为第一个单元格的彩色斑点是最大斑点，每当我们找到比它更大的彩色斑点时，我们就用找到的斑点替换它。

现在，让我们关注图像 (b)。为什么我们有负值？因为当我们访问一个单元格时，我们将其颜色值切换为 -color。这是一种方便的约定，用于避免多次计算同一单元格的连通集。这就像说我们将此单元格标记为已访问。根据约定，我们不能移动到颜色值为负的单元格，因此我们不会两次计算同一彩色斑点的大小。

现在，将这些观察结果组合起来形成一个递归方法会得到以下代码：

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

