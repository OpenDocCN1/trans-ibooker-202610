
**解决方案：**这个问题听起来有点模糊。重要的是与面试官讨论并就两个重要方面达成一致：

- 两个矩形彼此平行，并与水平面成 0 度角（它们平行于坐标轴），还是可以旋转一个角度？

大多数情况下，给定的矩形彼此平行且平行于坐标轴。如果涉及旋转，那么解决方案需要一些几何知识，这些知识在面试中并不那么明显。很可能，面试官想测试你的逻辑能力，而不是你的几何知识，但挑战自己，同时实现非平行矩形的解决方案。

- 矩形的坐标是在笛卡尔坐标系中给出的吗？

答案应该是肯定的，因为这是数学中常用的坐标系。这意味着矩形从左到右、从下到上增大其大小。

所以，让我们将这两个矩形记为 r1 和 r2。每个矩形通过左上角和右下角的坐标给出。r1 的左上角坐标为 r1lt.x 和 r1lt.y，而右下角坐标为 r1rb.x 和 r1rb.y，如下图所示：

![](img/6709a71136a6b327c831c6c15aff2f6a_669_0.png)

**图 15.8 – 矩形坐标**

我们可以说，如果两个矩形相互接触（它们至少有一个公共点），则它们是重叠的。换句话说，下图中显示的五对矩形是重叠的：

![](img/6709a71136a6b327c831c6c15aff2f6a_670_0.png)

从前面的图中，我们可以得出结论：两个不重叠的矩形可以处于以下四种情况之一：

- r1 完全在 r2 的右侧。
- r1 完全在 r2 的左侧。
- r1 完全在 r2 的上方。
- r1 完全在 r2 的下方。

下图展示了这四种情况：

**图 15.10 – 不重叠的矩形**

![](img/6709a71136a6b327c831c6c15aff2f6a_671_0.png)

我们可以用坐标来表示上述四点，如下所示：

- r1 完全在 r2 的右侧 → r1lt.x > r2rb.x
- r1 完全在 r2 的左侧 → r2lt.x > r1rb.x
- r1 完全在 r2 的上方 → r1rb.y > r2lt.y
- r1 完全在 r2 的下方 → r2rb.y > r1lt.y

```java
public static boolean overlap(Point r1lt, Point r1rb,
        Point r2lt, Point r2rb) {
    // r1 is totally to the right of r2 or vice versa
    if (r1lt.x > r2rb.x || r2lt.x > r1rb.x) {
        return false;
    }
    // r1 is totally above r2 or vice versa
    if (r1rb.y > r2lt.y || r2rb.y > r1lt.y) {
        return false;
    }
    return true;
}
```

这段代码在 O(1) 时间内运行。或者，我们可以将这两个条件合并为一个条件，如下所示：

```java
public static boolean overlap(Point r1lt, Point r1rb,
        Point r2lt, Point r2rb) {

    return (r1lt.x <= r2rb.x && r1rb.x >= r2lt.x
            && r1lt.y >= r2rb.y && r1rb.y <= r2lt.y);
}
```

完整的应用程序名为 *RectangleOverlap*。请注意，面试官可能以不同的方式定义*重叠*。基于这个问题，你应该能够相应地调整代码。

## 编程挑战 9 – 大数乘法

Amazon, Microsoft

**问题：**假设你得到了两个以字符串形式表示的正大数 *a* 和 *b*。这些数字不适合 `int` 或 `long` 域。编写一段代码来计算 `a*b`。

**解决方案：**让我们假设 `a=4145775` 和 `b=771467`。那么 `a*b=3198328601925`。解决这个问题依赖于数学。下图描绘了可以在纸上应用并同样可以编码的 `a*b` 解决方案：

![](img/6709a71136a6b327c831c6c15aff2f6a_672_0.png)

**图 15.11 – 两个大数相乘**

```java
// loop 'b' from right to left
for (int j = lenB - 1; j >= 0; j--) {
    // current digit of second number
    int n2 = b.charAt(j) - '0';
    // multiply with current digit of first number
    int sum = n1 * n2 + c[idx1 + idx2] + carry;
    // carry of the next iteration
    carry = sum / 10;
    c[idx1 + idx2] = sum % 10;
    idx2++;
}

// store carry
if (carry > 0) {
    c[idx1 + idx2] += carry;
}

// shift position to left after every
// multiplication of a digit in 'a'
idx1++;
}

// ignore '0's from the right
int i = c.length - 1;
while (i >= 0 && c[i] == 0) {
    i--;
}

// If all were '0's - means either both or
// one of 'a' or 'b' were '0'
if (i == -1) {
    return "0";
}

String result = "";
while (i >= 0) {
    result += (c[i--]);
}

return result;
}
```

完整的应用程序名为 *MultiplyLargeNumbers*。

## 编程挑战 10 – 具有相同数字的下一个最大数字

Amazon, Google, Microsoft

**问题：**假设你得到一个正整数。编写一段代码，返回具有相同数字的下一个最大数字。

**解决方案：**这个问题的解决方案可以通过几个例子来观察。让我们考虑以下示例：

- 示例 1：6 → 不可能
- 示例 2：1234 → 1243
- 示例 3：1232 → 1322
- 示例 4：321 → 不可能
- 示例 5：621873 → 623178

从前面的示例中，我们可以直观地看出，解决方案可以通过重新排列给定数字的各位数字来获得。因此，如果我们能找到导致目标数字的交换数字规则集，那么我们就可以尝试实现。

让我们尝试几个观察：

- 从示例 1 和 4 中，我们可以看到，如果给定数字的各位数字按降序排列，那么就不可能找到更大的数字。任何交换都会导致更小的数字。
- 从示例 2 中，我们可以看到，如果给定数字的各位数字按升序排列，那么可以通过交换最后两位数字来获得具有相同数字的下一个更大数字。
- 从示例 3 和 5 中，我们可以看到，我们需要找到所有更大数字中最小的那个。为此，我们必须从最右侧开始处理这个数字。下面的算法阐明了这一说法。

基于这三个观察，我们可以详细阐述以下算法，该算法以数字 621873 为例进行了说明：

1. 我们从最右侧开始逐位遍历数字。我们一直遍历，直到找到一个比前一个遍历的数字更小的数字。例如，如果给定数字是 621873，那么我们将遍历到 621873 中的数字 1。数字 1 是第一个比先前遍历的数字 8 更小的数字。
2. 接下来，我们关注在第 1 步找到的数字右侧的数字。我们想在这些数字中找到最小的数字（我们将其记为 t）。由于这些数字按降序排列，最小的数字位于最后一位。例如，在 621873 中，1 的右侧数字中最小的数字是 3。
3. 我们交换这两个数字（1 和 3），得到 623871。
4. 最后，我们将 t 右侧的所有数字按升序排序。但由于我们知道，t 右侧的所有数字（除了最后一个数字）都是按降序排序的，我们可以应用线性反转。这意味着结果是 623178。这就是要查找的数字。

该算法可以很容易地实现，如下所示：

```java
public static void findNextGreater(int arr[]) {

    int min = -1;
    int len = arr.length;

    int prevDigit = arr[arr.length - 1];
```

```java
while (start < end) {
    swap(arr, start, end);
    start++;
    end--;
}
}

private static void swap(int[] arr, int i, int j) {
    int aux = arr[i];
    arr[i] = arr[j];
    arr[j] = aux;
}
```

这段代码在 O(n) 时间内运行。完整的应用程序名为 NextElementSameDigits。

## 编程挑战 11 – 一个可被其各位数字整除的数字

Amazon, Google, Adobe, Microsoft

**问题：**假设你得到一个整数 n。编写一个程序，如果给定数字能被其各位数字整除，则返回 true。

**解决方案：**让我们考虑 n=412。输出应为 true，因为 412 能被 2、1 和 4 整除。另一方面，如果 n=143，则输出应为 false，因为 143 不能被 3 和 4 整除。

如果你认为这个问题很简单，那你完全正确。这类问题被用作热身问题，有助于快速筛选大量候选人。大多数情况下，你应该在给定的时间内（例如 2-3 分钟）解决它。

> **重要说明**
> 建议像对待任何其他问题一样认真地对待这些简单问题。一个小错误可能会让你过早地被淘汰出局。

因此，对于这个问题，算法包含以下步骤：

1. 获取给定数字的所有各位数字。
2. 对于每个数字，检查给定数字 % digit 是否为 0（这意味着可整除）。

