
如果对于输入规模 N，算法能在 N 的多项式级步数内解决问题，则称该算法是高效的。

以**多项式时间算法**为解的问题也被认为属于计算复杂性的 P（多项式）类。问题还可以划分为其他几种计算复杂性，这里给出几个例子：

- **NP（非确定性多项式时间）**问题的解可以在多项式时间内验证，但尚无已知的多项式时间解法。
- **EXPTIME（指数时间）**问题的解法运行时间与输入规模呈指数关系。
- **PSPACE（多项式空间）**问题需要多项式级别的空间。

弄清 P 类问题集合是否与 NP 类问题集合完全相同，就是著名的 P = NP 问题。经过数十年的努力，这一问题仍未解决，甚至为能够解决它的人设立了 100 万美元奖金。我们将在第 9 章《动态规划 II》中再次讨论 P 类和 NP 类问题。

几十年来，计算机科学家一直将算法作为数学对象进行研究，并确定了一套设计高效算法的通用方法（或**范式**），可用于解决各种各样的问题。其中应用最广泛的算法设计范式之一称为*分治法*，这也是本章的研究主题。

**分治**类算法将给定问题分解为较小的部分，尝试解决每个部分的问题，最后将每个部分的解合并为整个问题的解。几种广泛使用的算法都属于这一类，例如二分查找、快速排序、归并排序、矩阵乘法、快速傅里叶变换和天际线算法。这些算法几乎出现在当今使用的所有主要应用中，包括数据库、Web 浏览器，甚至 Java 虚拟机和 V8 JavaScript 引擎等语言运行时。

在本章中，我们将向你展示使用分治法解决问题意味着什么，以及如何判断你的问题是否适合采用这种解法。接下来，我们将练习递归思维，并向你展示现代 C++ 标准库提供的工具，以便你可以使用分治法解决问题。在本章最后，我们将探讨 MapReduce，包括讨论它为何以及如何扩展，以及如何利用相同范式通过 CPU 级和机器级并行化来扩展你的程序。

让我们深入了解一种使用分治法的基础算法——二分查找。

## 二分查找

让我们从标准的搜索问题开始：假设给定一个已排序的正整数序列，需要判断某个数 N 是否存在于该序列中。搜索问题在许多场景中自然出现；例如，前台在按客户 ID 排序的文件中查找客户档案，或者教师在学生名册中查找某位学生的成绩。实际上，他们都在解决搜索问题。

现在，我们可以用两种不同的方式来处理这个问题。第一种方法是遍历整个序列，检查每个元素是否等于 N。这称为线性搜索，代码如下所示：

```
bool linear_search(int N, std::vector<int>& sequence)
{
    for (auto i : sequence)
    {
        if (i == N)
            return true;    // Element found!
    }
    return false;
}
```

这种方法的一个优点是它适用于所有数组，无论排序与否。然而，它的效率不高，并且没有利用给定数组已排序的事实。就算法复杂度而言，它是一个 O(N) 算法。

另一种利用序列已排序这一事实的替代解法如下：

1. 从整个序列作为当前范围开始。
2. 将当前范围的中间元素与 N 比较。设该中间元素为 M。
3. 如果 M = N，则说明已在序列中找到 N，搜索停止。
4. 否则，根据两条规则修改范围：
   - 如果 N < M，说明若 N 存在于范围内，则它一定在 M 的左侧，因此我们可以安全地将 M 右侧的所有元素移出范围。
   - 如果 N > M，算法将 M 左侧的所有元素移出范围。
5. 如果范围内剩余元素超过 1 个，则回到步骤 2。
6. 否则，说明 N 不存在于序列中，搜索停止。

为了说明该算法，我们将展示二分查找的工作过程，其中 S 是从 1 到 9 的整数排序序列，N = 2：

1. 算法首先将 S 的所有元素放入范围。此步骤中的中间元素为 5。我们将 N 与 5 比较：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_166_0.png)

图 4.2：二分查找算法——步骤 1

2. 由于 N < 5，如果 N 存在于序列中，它一定在 5 的左侧。因此，我们可以安全地从搜索范围中丢弃序列中位于 5 右侧的所有元素。现在范围只包含 1 到 5 之间的元素，中间元素现在是 3。我们现在可以将 N 与 3 比较：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_166_1.png)

图 4.3：二分查找算法——步骤 2

3. 我们发现当前中间元素 3 仍然大于 N，范围可以进一步缩减为只包含 1 到 3 之间的元素。新的中间元素现在是 2，正好等于 N，搜索终止：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_166_2.png)

图 4.4：二分查找算法——步骤 3

在下面的练习中，我们将学习二分查找算法的实现。

## 练习 18：二分查找基准测试

在本练习中，我们将编写并对一个二分查找实现进行基准测试。请按照以下步骤完成本练习：

1. 首先添加以下头文件：

```
#include <iostream>
#include <vector>
#include <chrono>
#include <random>
#include <algorithm>
#include <numeric>
```

2. 添加如下线性搜索代码：

```
bool linear_search(int N, std::vector<int>& S)
{
    for (auto i : S)
    {
        if (i == N)
            return true;    // Element found!
    }
    return false;
}
```

3. 添加如下所示的二分查找代码：

```
bool binary_search(int N, std::vector<int>& S)
{
    auto first = S.begin();
    auto last = S.end();
    
    while (true)
    {
        // Get the middle element of current range
        auto range_length = std::distance(first, last);
        auto mid_element_index = first + std::floor(range_length / 2);
        auto mid_element = *(first + mid_element_index);

        // Compare the middle element of current range with N
```

```
```cpp
if (mid_element == N)
    return true;
else if (mid_element > N)
    std::advance(last, -mid_element_index);
if (mid_element < N)
    std::advance(first, mid_element_index);

// If only one element left in the current range
if (range_length == 1)
    return false;
}
}
```

4. 为了评估二分查找的性能，我们将实现两个函数。首先编写小型测试：

```cpp
void run_small_search_test()
{
    auto N = 2;
    std::vector<int> S{ 1, 3, 2, 4, 5, 7, 9, 8, 6 };

    std::sort(S.begin(), S.end());

    if (linear_search(N, S))
        std::cout << "Element found in set by linear search!" << std::endl;
    else
        std::cout << "Element not found." << std::endl;

    if (binary_search(N, S))
        std::cout << "Element found in set by binary search!" << std::endl;
    else
        std::cout << "Element not found." << std::endl;
}
```

5. 现在，添加大型测试函数，如下所示：

```cpp
void run_large_search_test(int size, int N)
{
    std::vector<int> S;
    std::random_device rd;
    std::mt19937 rand(rd());
```
