

## Exercise 41: 0-1 背包问题

我们现在将使用表格化自底向上的方法实现前述逻辑。让我们开始：

- 1. 我们将首先包含以下头文件：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;
```

- 2. 我们的第一步将是处理输入。我们需要声明两个整数，items 和 capacity，它们分别表示可供选择的物品总数和背包的重量限制。我们还需要两个数组，value 和 weight，在其中存储每个物品对应的数据：

```cpp
int main()
{
    int items, capacity;

    cin >> items >> capacity;

    vector<int> values(items), weight(items);

    for(auto &v : values) cin >> v;
    for(auto &w : weight) cin >> w;

    ....
}
```

- 3. 现在，我们将定义函数 Knapsack_01()，它具有对应于输入的参数并返回一个整数：

```cpp
int Knapsack_01(int items, int capacity, vector<int> value, vector<int> weight)
{
    ....
}
```

- 4. 我们的 DP 表将是二维的，并且与我们在子集和问题中使用的表非常接近。在子集和表中，第一维的大小初始化为集合长度加一，而第二维的大小初始化为集合中所有元素最大和加一。这里，我们的第一维大小将等效地初始化为 items + 1；同样，第二维的大小将初始化为 capacity + 1：

```cpp
vector<vector<int>> DP(items + 1, vector<int>(capacity + 1, 0));
```

- 5. 我们需要从 1 开始遍历两个维度的长度。在外层循环的每次迭代开始时，我们将定义两个变量 currentWeight 和 currentValue，它们分别对应 weight[i-1] 和 values[i-1] 中的元素：

```cpp
for(int i = 1; i <= items; i++)
{
    int currentWeight = weight[i-1];
    int currentValue = values[i-1];

    for(int totalWeight = 1; totalWeight <= capacity; totalWeight++)
    {
        ....
    }
}
```

- 6. 现在，我们将实现我们的表格化方案：

```cpp
if(totalWeight < currentWeight)
{
    DP[i][totalWeight] = DP[i-1][totalWeight];
}
else
{
    DP[i][totalWeight] = max(DP[i-1][totalWeight], DP[i-1][totalWeight - currentWeight] + currentValue);
}
```

- 7. 在函数末尾，我们返回表的最后一个元素：

```cpp
return DP[items][capacity];
```

- 8. 现在，我们在 main() 中添加调用并打印输出：

```cpp
int result = Knapsack_01(items, capacity, values, weight);

cout << "The highest-valued subset of items that can fit in the knapsack is: " << result << endl;

return 0;
```

- 9. 让我们使用以下输入运行我们的程序：

```text
8 66
20 4 89 12 5 50 8 13
5 23 9 72 16 14 32 4
```

输出应如下所示：

```text
The highest-valued subset of items that can fit in the knapsack is: 180
```

正如我们所见，背包问题的相对高效的 DP 解决方案不过是对我们用于解决子集和问题时所用算法的轻微修改。

## 无界背包

我们关于背包问题的实现所探索的是最传统的版本，但正如我们在本章前面提到的，实际上存在许多种类的这个问题，可以应用于不同的场景。我们现在将考虑每种物品数量无限的情况。

让我们考虑一个通过暴力求解找到解的例子：

```text
Capacity = 25

Values -> { 5, 13, 4, 3, 8 }
Weight -> { 9, 12, 3, 7, 19 }

{ 0 } -> Weight = 9, Value = 5
{ 1 } -> Weight = 12, Value = 13
{ 2 } -> Weight = 3, Value = 4
{ 3 } -> Weight = 7, Value = 3
{ 4 } -> Weight = 19, Value = 8

{ 0, 0 } -> Weight = 18, Value = 10
{ 0, 1 } -> Weight = 21, Value = 18
{ 0, 2 } -> Weight = 12, Value = 9
{ 0, 3 } -> Weight = 16, Value = 8
{ 0, 4 } -> Weight = 28, Value = 13
{ 1, 1 } -> Weight = 24, Value = 26
{ 1, 2 } -> Weight = 15, Value = 17
{ 1, 3 } -> Weight = 19, Value = 16
{ 1, 4 } -> Weight = 31, Value = 21
{ 2, 2 } -> Weight = 6, Value = 8
{ 2, 3 } -> Weight = 10, Value = 7
{ 2, 4 } -> Weight = 22, Value = 12
{ 3, 3 } -> Weight = 14, Value = 6
{ 3, 4 } -> Weight = 26, Value = 11
{ 4, 4 } -> Weight = 38, Value = 16

{ 0, 0, 0 } -> Weight = 27, Value = 15
{ 0, 0, 1 } -> Weight = 30, Value = 26
{ 0, 0, 2 } -> Weight = 21, Value = 14
{ 0, 0, 3 } -> Weight = 25, Value = 13
{ 0, 0, 4 } -> Weight = 37, Value = 18
{ 0, 1, 1 } -> Weight = 33, Value = 31
.....
```

从暴力角度来看，这个问题似乎复杂得多。让我们重新陈述我们从 0-1 背包实现中得到的伪代码逻辑，以处理这个额外的限制。

```cpp
int current_weight = weight[i - 1];
int value = values[i - 1];

for(int total_weight = 0; total_weight <= capacity; total_weight++)
{
    if(total_weight < current_weight)
    {
        DP[i][total_weight] = DP[i - 1][total_weight];
    }
    else
    {
        DP[i][total_weight] = max
        (
            DP[i - 1][total_weight],
            DP[i - 1][total_weight - current_weight] + value,
            DP[i][total_weight - current_weight] + value
        );
    }
}
```

逻辑上，这种方法可行，但事实证明这并不是最有效的实现。让我们在下一节中了解其局限性以及如何克服它们。

## 状态空间缩减

使用 DP 有效地使用的一个相当棘手方面是状态空间缩减的概念，即重新制定一个可行的 DP 算法以使用表示状态所需的最小空间。这通常归结为利用问题固有的某些模式或对称性。

正如我们所见，这个算法在内存使用和冗余计算方面都相当浪费。最明显的问题是表的列数为 N + 1，尽管事实上只有一行包含那么多值。我们可以很容易地通过按需初始化每一行，按其所需元素大小调整，从而减少空间复杂度，将表的空间需求从 N² 减少到 N × (N + 1) / 2。让我们按如下方式修改我们的实现：

```cpp
vector<vector<int>> DP(N + 1);

DP[1] = { 0, 1 };

for(int row = 2; row <= N; row++)
{
    DP[row].resize(row + 1);
    for(int col = 1; col <= row; col++)
    {
        int a = DP[row-1][col-1];
        int b = DP[row-1][min(col, DP[row-1].size()-1)];
        DP[row][col] = a + b;
    }
}
```

我们可能进一步观察到每一行的第一半和第二半之间存在对称关系，这意味着我们实际上只需要计算前 (n/2) 列的值。因此，我们有以下内容：

```text
DP(7, 7) ≡ DP(7, 1)
DP(7, 6) ≡ DP(7, 2)
DP(7, 5) ≡ DP(7, 3)
```

我们可以以一般化的方式表述如下：

```text
DP(N, M) ≡ DP(N, N - M + 1)
```

考虑到这一点，我们可以按如下方式修改我们的实现：

```cpp
vector<vector<int>> DP(N + 1);

DP[0] = { 0, 1 };

for(int row = 1; row <= N; row++)
{
    int width = (row / 2) + (row % 2);

    DP[row].resize(width + 2);

    for(int col = 1; col <= width; col++)
    {
        DP[row][col] = DP[row-1][col-1] + DP[row-1][col];
    }

    if(row % 2 == 0)
    {
        DP[row][width+1] = DP[row][width];
    }
}
```

---

```cpp
for(int i = 0; i < queries; i++)
{
    int N, M;
    cin >> N >> M;

    if(M * 2 > N)
    {
        M = N - M + 1;
    }
    cout << DP[N][M] << endl;
}
```

最后，假设我们能够提前接收输入查询并预先计算结果，我们可以完全放弃存储完整的表，因为只有上一行才需要来为当前行生成结果。因此，我们可以进一步按如下方式修改我们的实现：

```cpp
map<pair<int, int>, int> results;
vector<pair<int, int>> queries;

int q;
cin >> q;

int maxRow = 0;

for(int i = 0; i < q; i++)
{
    int N, M;
    cin >> N >> M;
    queries.push_back({N, M});

    if(M * 2 > N) M = N - M + 1;

    results[{N, M}] = -1;
    maxRow = max(maxRow, N);
}

vector<int> prev = { 0, 1 };
```

