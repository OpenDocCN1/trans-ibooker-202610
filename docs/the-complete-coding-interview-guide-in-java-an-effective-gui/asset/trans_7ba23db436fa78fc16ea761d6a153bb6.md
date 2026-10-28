

```java
}

// create the result array
int[] result = new int[len];

// create the min heap
MinHeap[] heap = new MinHeap[k];

// add in the heap first element from each array
for (int i = 0; i < k; i++) {
    heap[i] = new MinHeap(arrs[i][0], i, 0);
}

// perform merging
for (int i = 0; i < result.length; i++) {
    heapify(heap, 0, k);

    // add an element in the final result
    result[i] = heap[0].data;

    heap[0].currentIndex++;
    int[] subarray = arrs[heap[0].heapIndex];
    if (heap[0].currentIndex >= subarray.length) {
        heap[0].data = Integer.MAX_VALUE;
    } else {
        heap[0].data = subarray[heap[0].currentIndex];
    }
}

return result;
}
```

完整的应用程序称为 *MergeKSortedArr*。

![](img/6709a71136a6b327c831c6c15aff2f6a_349_0.png)

## 图 10.13 – 将数组拆分为两个相等部分

我们可以从该图中得出，中位数值应遵守的第一个条件是 qLeft + pLeft = qRight + pRight。换句话说，qPointer + pPointer = (q.length - qPointer) + (p.length - pPointer)。

然而，由于我们的数组长度不相同（“它们可以相等，但这只是应该被我们的解决方案覆盖的一个特例”），我们不能简单地将两者都减半。我们可以做的是假设 p >= q（如果它们不是这样给出，那么我们只需交换它们以强制此假设）。此外，在此假设的 umbrella 下，我们可以写出以下内容：

```
qPointer + pPointer = (q.length - qPointer) + (p.length - pPointer) →
2 * pPointer = q.length + p.length - 2 * qPointer →
 pPointer = (q.length + p.length)/2 - qPointer
```

到目前为止，pPointer 可能落在中间，我们可以通过加 1 来避免这种情况，这意味着我们有以下起始指针：

+   • qPointer = ((q.length - 1) + 0)/2
+   • pPointer = (q.length + p.length + 1)/2 - qPointer

如果 p>=q，则最小的 (q.length + p.length + 1)/2 - qPointer 将始终导致 pPointer 为正整数。这将消除数组越界异常并遵守第一个条件。

## 图 10.14 – 计算中位数值（步骤 1）

我们算法的步骤 1 规定 q[qPointer-1] <= p[pPointer] 且 p[pPointer-1] <= q[qPointer]。显然，9 < 28，但 25 > 10，所以我们应用步骤 2，然后返回步骤 1。我们增加 qPointer 并减少 pPointer，因此 qPointerMin 变为 qPointer + 1。新的 qPointer 将为 (4 + 6) / 2 = 5，新的 pPointer 将为 (7 + 9 + 1)/2 - 5 = 3。以下图像将帮助您可视化此场景：

## 图 10.15 – 计算中位数值（步骤 2）

在这里，您可以看到新的 qPointer 和新的 pPointer 遵守我们算法的步骤 1，因为 q[qPointer-1]，即 11，小于 p[pPointer]，即 18；并且 p[pPointer-1]，即 17，小于 q[qPointer]，即 65。由此，我们找到了完美的 qPointer 为 5。

最后，我们必须找到左侧的最大值和右侧的最小值，并根据两个数组的奇偶长度，返回左侧的最大值或左侧的最大值与右侧的最小值的平均值。我们知道左侧的最大值是 max(q[qPointer-1], p[pPointer-1])，因此 max(11, 17) = 17。我们还知道右侧的最小值是 min(q[qPointer], p[pPointer])，因此 min(65, 18) = 18。由于长度之和为 7 + 9 = 16，我们计算出中位数值为这两个数的平均值，因此 avg(17, 18) = 17.5。我们可以将其可视化如下：

![](img/6709a71136a6b327c831c6c15aff2f6a_352_0.png)

图 10.16 – 中位数值（最终结果）

将此算法放入代码中会得到以下输出：

```
public static float median(int[] q, int[] p) {

    int lenQ = q.length;
    int lenP = p.length;

    if (lenQ > lenP) {
        swap(q, p);
    }

    int qPointerMin = 0;
    int qPointerMax = q.length;
    int midLength = (q.length + p.length + 1) / 2;

    int qPointer;
    int pPointer;

    while (qPointerMin <= qPointerMax) {

        qPointer = (qPointerMin + qPointerMax) / 2;
        pPointer = midLength - qPointer;
```
```
```java
// perform binary search
if (qPointer < q.length
    && p[pPointer-1] > q[qPointer] ) {
    // qPointer must be increased
    qPointerMin = qPointer + 1;
} else if (qPointer > 0
    && q[qPointer-1] > p[pPointer] ) {
    // qPointer must be decreased
    qPointerMax = qPointer - 1;
} else { // we found the proper qPointer

    int maxLeft = 0;

    if (qPointer == 0) { // first element on array 'q'?
        maxLeft = p[pPointer - 1];
    } else if (pPointer == 0) { // first element
                               // of array 'p'?
        maxLeft = q[qPointer - 1];
    } else { // we are somewhere in the middle -> find max
        maxLeft = Integer.max(q[qPointer-1], p[pPointer-1]);
    }

    // if the length of 'q' + 'p' arrays is odd,
    // return max of left
    if ((q.length + p.length) % 2 == 1) {
        return maxLeft;
    }

    int minRight = 0;

    if (qPointer == q.length) { // last element on 'q'?
        minRight = p[pPointer];

    } else if (pPointer == p.length) { // last element
                                       // on 'p'?
        minRight = q[qPointer];
    } else { // we are somewhere in the middle -> find min
```
```
```
minRight = Integer.min(q[qPointer], p[pPointer]);
}

return (maxLeft + minRight) / 2.0f;
}
}

return -1;
}
```

我们的解决方案在 O(log(max(q.length, p.length))) 时间内执行。完整的应用程序称为 MedianOfSortedArrays。

## 编程挑战 15 – 全为一的子矩阵

Amazon, Microsoft, Flipkart

问题：假设您被给定一个 m x n 的矩阵，只包含 0 和 1（二进制矩阵）。编写一段代码返回仅包含元素 1 的最大正方形子矩阵的大小。

解决方案：让我们假设给定的矩阵是以下图像中的矩阵（5x7 矩阵）：

![](img/6709a71136a6b327c831c6c15aff2f6a_354_0.png)

图 10.17 – 给定的 5 x 7 二进制矩阵

正如您所见，仅包含元素 1 的正方形子矩阵的大小为 3。暴力方法，或朴素方法，将是找到所有包含全为 1 的正方形子矩阵并确定哪个具有最大尺寸。然而，对于具有 *z=min(m, n)* 的 *m x n* 矩阵，时间复杂度将为 O(z³mn)。您可以在本书捆绑的代码中找到暴力实现。当然，在查看解决方案之前先挑战自己。

现在，让我们尝试找到更好的方法。让我们假设给定的矩阵为 *n x n* 大小并研究 4x4 样本矩阵的几种场景。在 4x4 矩阵中，我们可以看到全为 1 的最大正方形子矩阵可以为 3x3 大小，因此在 *n x n* 矩阵中，全为 1 的最大正方形子矩阵可以为 *n-1 x n-1* 大小。此外，以下图像揭示了对 *m x n* 矩阵也为真的两个基本情况：

![](img/6709a71136a6b327c831c6c15aff2f6a_355_0.png)

图 10.18 – 4 x 4 矩阵中全为 1 的最大子矩阵

这些情况解释如下：

+   - 如果给定的矩阵只包含一行，则其中为 1 的单元格将是正方形子矩阵的最大尺寸。因此，最大尺寸为 1。
+   - 如果给定的矩阵只包含一列，则其中为 1 的单元格将是正方形子矩阵的最大尺寸。因此，最大尺寸为 1。

接下来，让我们假设 *subMatrix[i][j]* 表示以单元格 *(i,j)* 结尾的全为 1 的最大正方形子矩阵的大小：

![](img/6709a71136a6b327c831c6c15aff2f6a_355_1.png)

图 10.19 – 整体递推关系

前面的图使我们能够在给定的矩阵和辅助 *subMatrix*（与给定矩阵大小相同的矩阵，应根据递推关系填充）之间建立递推关系：

+   - 这并不容易直观，但我们可以看出如果 *matrix[i][j]* = 0，则 *subMatrix[i][j]* = 0
+   - 如果 *matrix[i][j]* = 1，则 *subMatrix[i][j]* = 1 + min(*subMatrix[i - 1][j]*, *subMatrix[i][j - 1]*, *subMatrix[i - 1][j - 1]*)

如果我们将此算法应用于我们的 5 x 7 矩阵，则得到以下结果：

**给定矩阵**

| 0 | 0 | 1 | 1 | 0 | 0 | 0 |
|---|---|---|---|---|---|---|
| 0 | 0 | 1 | 1 | 1 | 1 | 1 |
| 1 | 1 | 0 | 0 | 1 | 1 | 1 |
| 1 | 1 | 0 | 0 | 1 | 1 | 1 |
| 0 | 1 | 1 | 0 | 0 | 0 | 0 |

**subMatrix**

| 0 | 0 | 1 | 1 | 0 | 0 | 0 |
|---|---|---|---|---|---|---|
| 0 | 0 | 1 | 2 | 1 | 1 | 1 |
| 1 | 1 | 0 | 0 | 1 | 2 | 2 |
| 1 | 2 | 0 | 0 | 1 | 2 | 3 |
| 0 | 1 | 1 | 0 | 0 | 0 | 0 |

图 10.20 – 解析我们的 5 x 7 矩阵

将前面的基本情况和递推关系组合起来得到以下算法：

+   1. 创建一个与给定矩阵大小相同的辅助矩阵（*subMatrix*）。
+   2. 将给定矩阵的第一行和第一列复制到此辅助 *subMatrix*（这些是基本情况）。
+   3. 对于给定矩阵中的每个单元格（从 (1, 1) 开始），执行以下操作：
+      a. 填充 *subMatrix* 符合前面的递推关系。
+      b. 跟踪 *subMatrix* 的最大元素，因为该元素给出了包含全为 1 的子矩阵的最大尺寸。

以下实现阐明了任何剩余的细节：

```
```java
public static int ofOneOptimized(int[][] matrix) {

    int maxSubMatrixSize = 1;
    int rows = matrix.length;
```

