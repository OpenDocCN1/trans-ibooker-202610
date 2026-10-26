# 第7章：图算法II

# 活动15：贪心机器人

我们可以使用*练习33：实现贝尔曼-福特算法（第二部分）*中的精确算法来解决此活动。这里的潜在陷阱与正确解释所需任务以及在您实际要解决的上下文环境中表示图有关。让我们开始吧：

+   1. 第一步与练习相同。我们将包含相同的头文件，并定义一个**Edge**结构体和一个**UNKNOWN**常量：

```cpp
#include <iostream>
#include <vector>
#include <climits>

using namespace std;

struct Edge
{
        int start;
        int end;
        int weight;

        Edge(int s, int e, int w) : start(s), end(e), weight(w) {}
};

const int UNKNOWN = INT_MAX;
vector<Edge*> edges;
```

+   2. 在`main()`中，我们将声明一个整数**N**，它决定网格的高度/宽度。然后，我们将在`for`循环中从0迭代到N * N - 1，并读取输入中给出的邻接数据：

```cpp
int main()
{
        int N;
        cin >> N;

        for(int i = 0; i < N * N - 1; i++)
        {
                string directions;
                int power;
```

```cpp
}

if(distance[edge->start] + edge->weight < distance[edge->end])
{
    distance[edge->end] = distance[edge->start] + edge->weight;
}
}
}
}
.....
}
```

7. 按照标准实现，我们还将执行负环检查，以处理与机器人贪婪能量消耗相关的条件。如果发现负环，我们将返回UNKNOWN：

```cpp
// Check for negative cycles
for(auto edge : edges)
{
    if(distance[edge->start] == UNKNOWN)
    {
        continue;
    }
    if(distance[edge->start] + edge->weight < distance[edge->end])
    {
        return UNKNOWN;
    }
}
return distance[N];
```

8. 现在，我们可以在main()中调用BellmanFord()并相应地处理输出：

```cpp
int result = BellmanFord(N * N, edges);

(result == UNKNOWN) ? cout << "ABORT TRAVERSAL" << endl
                    : cout << -1 * result << endl;

return 0;
```

```cpp
weight[u][v] = w;
maxWeight = max(maxWeight, w);

used[u][u] = used[v][v] = used[u][v] = used[v][u] = true;
e--;
}
for(int i = 0; i < V; i++)
{
    // Set V to -1 to indicate the graph is invalid
    if(!used[i][i])
    {
        V = -1;
        break;
    }
}
}
```

4. 让我们还要定义一个名为**RESULT**的枚举，其中包含我们需要考虑的每种图类型对应的值：

```cpp
enum RESULT
{
    VALID,
    INVALID,
    INTERESTING
};
```

5. 在main()中，我们将接收输入，并声明每种图类型的计数器。然后，我们将循环遍历给定的迭代次数，创建一个新图，并调用一个**TestGraph()**函数，该函数将**Graph**对象作为输入并返回**RESULT**。根据返回的值，我们将相应地递增每个计数器：

```cpp
int main()
{
    unsigned int seed;
    int iterations, V, E;

    cin >> seed;
    cin >> iterations;
    cin >> V >> E;

    int invalid = 0;
    int valid = 0;
```




```
```cpp
int interesting = 0;

srand(seed);

while(iterations--)
{
    Graph (G, E);

    switch(TestGraph(G))
    {
        case INVALID: invalid++; break;
        case VALID: valid++; break;
        case INTERESTING:
        {
            valid++;
            interesting++;
            break;
        }
    }
}

return 0;
}
```
```

6. **TestGraph()** 将首先检查每个图的 V 值是否等于 -1，如果是则返回 INVALID。否则，它将执行 Johnson 算法来检索最短距离。第一步将使用 Bellman-Ford 算法检索重新加权数组：

```
```cpp
RESULT TestGraph(Graph G)
{
    if(G.V == -1)
    {
        return INVALID;
    }

    vector<int> distance = BellmanFord(G);

    ......
}
```
```

```
continue;
}

if(distance[edge.u] + edge.w < distance[edge.v])
{
    return {};
}
}
return distance;
}
```

8. 与练习中一样，我们将检查 BellmanFord() 返回的 vector 是否为空。如果为空，则返回 VALID（图有效但无趣）。否则，我们将继续执行 Johnson 算法的其余部分，对边重新加权，并对每个顶点调用 Dijkstra 算法：

```
RESULT TestGraph(Graph G)
{
    if(G.V == -1)
    {
        return INVALID;
    }

    vector<int> distance = BellmanFord(G);

    if(distance.empty())
    {
        return VALID;
    }

    for(auto edge : G.edges)
    {
        G.weight[edge.u][edge.v] += (distance[edge.u] - distance[edge.v]);
    }

    double result = 0;

    for(int i = 0; i < G.V; i++)
    {
```

```
vector<int> shortest = Dijkstra(i, G);

    }
}
```

9. 对于这个解决方案，让我们使用一种更高效的 Dijkstra 算法形式，它使用最小优先队列来确定遍历顺序。为此，添加到队列中的每个值必须包含两个值：节点的索引及其距离值。我们将使用 std::pair<int, int> 来实现，此处已将其重定义为 State。当向队列推入元素时，第一个值必须对应距离，因为优先队列的内部排序逻辑会首先考虑该值。所有这些都可以由 std::priority_queue 处理，但我们需要提供三个模板参数，分别对应数据类型、容器和比较谓词：

```
vector<int> Dijkstra(int source, Graph G)
{
    typedef pair<int, int> State;

    priority_queue<State, vector<State>, greater<State>> Q;
    vector<bool> visited(G.V, false);
    vector<int> distance(G.V, UNKNOWN);

    Q.push({0, source});
    distance[source] = 0;

    while(!Q.empty())
    {
        State top = Q.top();
        Q.pop();

        int node = top.second;
        int dist = top.first;

        visited[node] = true;

        for(auto next : G.adj[node])
        {
            if(visited[next])
            {
```

```
continue;
}

if(dist != UNKNOWN && distance[next] > dist + G.weight[node][next])
{
    distance[next] = distance[node] + G.weight[node][next];
    Q.push({distance[next], next});
}
}
}
return distance;
}
```

10. 现在，我们将在 TestGraph() 中计算每组路径的平均值。为此，我们遍历 Dijkstra() 返回的数组，并累加满足以下条件的距离之和：索引不等于起始节点的索引，并且对应值不等于 UNKNOWN。每当找到一个有效距离时，计数器也会递增，这样我们就可以通过将总和除以计数来得到最终平均值。然后，将这些平均值中的每一个加到总结果中，再除以图中的顶点总数。请记住，我们必须再次重新加权距离以获得正确的值：

```
double result = 0;

for(int i = 0; i < G.V; i++)
{
    vector<int> shortest = Dijkstra(i, G);

    double average = 0;
    int count = 0;

    for(int j = 0; j < G.V; j++)
    {
        if(i == j || shortest[j] == UNKNOWN)
        {
```

## 活动 17：迷宫传送游戏

整个活动与我们本章讨论的算法的标准实现相当接近，但有一些细微的修改。

问题描述中使用的术语，即*迷宫*、*房间*、*传送器*和*点数*，自然也可以很容易地称为*图*、*顶点*、*边*和*边权重*。玩家能够无限降低分数的情况可以重新定义为*负权重环*。按照以下步骤完成此活动：

+   1. 让我们首先包含必要的头文件，并为活动设置变量和输入：

```
```cpp
#include <iostream>
#include <vector>
#include <stack>
#include <climits>

struct Edge
{
    int start;
    int end;
    int weight;

    Edge(int s, int e, int w) : start(s), end(e), weight(w) {}
}

const int UNKNOWN = INT_MAX;
vector<Edge*> edges; // Collection of edge pointers
```
```

+   2. 我们将以与原始 Bellman-Ford 实现相同的形式接收输入，但同时也会为我们的图构建邻接表（此处表示为整数向量的向量 `adj`）：

```
```cpp
int main()
{
    int V, E;
    cin >> V >> E;

    vector<Edge*> edges;
    vector<vector<int>> adj(V + 1);

    for(int i = 0; i < E; i++)
    {
        int u, v, w;
```

```
```cpp
cin >> u >> v >> w;

edges.push_back(new Edge(u, v, w));
adj[u].push_back(v);
}

vector<int> results;
```
```

3. 问题的第一部分可以使用与练习 32，*实现 Bellman-Ford 算法（第一部分）* 中所述完全相同的方式，使用 Bellman-Ford 求解。但是，我们不会打印距离数组中的所有值，而是将其返回类型设置为 `int`，并添加几行额外代码，使其仅返回从源顶点开始的最短距离（如果检测到负环，则返回 `UNKNOWN`）：

```
```cpp
int BellmanFord(int V, int start, vector<Edge*> edges)
{
    // Standard Bellman-Ford implementation

    vector<int> distance(V, UNKNOWN);

    distance[start] = 0;

    for(int i = 0; i < V - 1; i++)
    {
        for(auto edge : edges)
        {
            if(distance[edge->start] == UNKNOWN)
            {
                continue;
            }
            if(distance[edge->start] + edge->weight < distance[edge->end])
            {
                distance[edge->end] = distance[edge->start] + edge->weight;
            }
        }
    }

    // Return UNKNOWN if a negative cycle is found

    if(HasNegativeCycle(distance, edges))
    {
```

7. 现在，我们将开始 while 循环，在栈上每次执行 DFS 遍历后递增 componentIndex：

```
while(!stack.empty())
{
    int node = stack.top();
    stack.pop();

    if(!visited[node])
    {
        vector<int> component;

        GetComponent(node, transpose, visited, component);

        components.push_back(component);
        componentIndex++;
    }
}
```

8. 现在，我们可以编写 GetComponent() 中的逻辑来处理这种情况。我们首先将每个节点在 inComponent 中的索引值设置为 componentIndex。然后，在遍历每个节点的邻居时，我们会加入另一个条件，用于处理节点已被访问过的情况：

```
component.push_back(node);
visited[node] = true;

inComponent[node] = componentIndex;

for(auto next : adj[node])
{
    if(!visited[next])
    {
        GetComponent(next, visited, adj, component);
    }
    else if(inComponent[node] != inComponent[next])
    {
        isStuck[inComponent[next]] = false;
    }
}
```

处理此问题中的输入和输出可以非常简单地实现：

```
```cpp
int main()
{
    int n;
    cin >> n;

    vector<int> distance(n);

    for(int i = 0; i < n; i++)
    {
        cin >> distance[i];
    }

    LL result = TravelItinerary(n, distance);

    cout << result << endl;

    return 0;
}
```
```

+   4. 我们现在将定义一个名为 **TravelItinerary()** 的函数，它接受 n 和 distance 作为参数，并返回一个长整数：

```
```cpp
LL TravelItinerary(int n, vector<int> distance)
{
    ...
}
```
```

+   5. 现在，我们必须将之前介绍的递归算法转换为自底向上的方法。在伪代码中，这可能如下所示：

```
```text
DP -> Array of size N + 1

DP[0] = 1 (There is one way to reach the starting location)

for i = 0 to N-1:

    for j = 1 to distance[i]:

        DP[i + j] += DP[i]

return DP[N]
```
```





3. 我们为此函数的代码也将与 `LCS_BruteForce()` 非常相似，只是我们将通过递归遍历两个字符串的前缀（从完整字符串开始）来反转逻辑，并在每一步将结果存储在我们的 **memo** 表中：

```
// Base case - LCS is always zero for empty strings
if(i == 0 || j == 0)
{
    return 0;
}

// Have we found a result for the prefixes of the two strings?
if(memo[i - 1][j - 1] != UNKNOWN)
{
    // If so, return it
    return memo[i - 1][j - 1];
}

// Are the last characters of A's prefix and B's prefix equal?
if(A[i-1] == B[j-1])
{
    // LCS for this state is equal to 1 plus the LCS of the prefixes of A
    // and B, both reduced by one character
    memo[i-1][j-1] = 1 + LCS_Memoization(A, B, i-1, j-1, memo);

    // Return the cached result
    return memo[i-1][j-1];
}

// If the last characters are not equal, LCS for this state is equal to
// the maximum LCS of A's prefix reduced by one character and B's prefix, and
// B's prefix reduced by one character and A's prefix

memo[i-1][j-1] = max(LCS_Memoization(A, B, i-1, j, memo),
                     LCS_Memoization(A, B, i, j-1, memo));

return memo[i-1][j-1];
```

4. 现在，让我们重新定义我们的 **main()** 函数以执行这两种方法并显示每种方法所花费的时间：

```
int main()
{
    string A, B;
    cin >> A >> B;

    int tests = 2;

    clock_t timer = clock();

    for(int i = 0; i < tests; i++)
    {
        int LCS;

        switch(i)
        {
            case 0:
            {
                LCS = LCS_BruteForce(A, B, 0, 0, {});

                #if DEBUG
                    PrintSubsequences(A, B);
                #endif
                break;
            }
            case 1:
            {
                vector<vector<int>> memo(A.size(), vector<int>(B.size(), UNKNOWN));
                LCS = LCS_Memoization(A, B, A.size(), B.size(), memo);
                break;
            }
        }
        cout << "Length of the longest common subsequence of " << A << " and " << B << " is: " << LCS << ends;
    }
}
```

```
GetTime(timer, types[i]);

    cout << endl;
}
    return 0;
}
```

5. 现在，让我们尝试对两个新字符串 ABCABDBEFBA 和 ABCBEFBEAB 使用我们的两种算法。你的程序输出应类似于以下内容：

```
SIZE = 3
    ABC_______ ABC_______
SIZE = 4
    ABC_B_____ ABCB_____
    ABC_B_____ ABC___B__
    ABC_B_____ ABC_____B
    ABC___B___ ABC_____B
    ABC____E__ ABC___E__
    ABC_____B_ ABC___B__
    ABC_____B_ ABC_____B
    ABC______A ABC_____A_
SIZE = 5
    ABCAB_____ ABC____AB
    ABC_B_B___ ABCB_____B
    ABC_B__E__ ABCB____E_
    ABC_B___B_ ABCB__B__
    ABC_B___B_ ABCB_____B
    ABC_B____A ABCB____A_
    ABC_B_B___ ABC___B__B
    ABC_B__E__ ABC___BE__
    ABC_B___B_ ABC___B__B
    ABC_B____A ABC__B_A_
    ABC___BE__ ABC___BE__
    ABC____E_B ABC____E_B
    ABC____E_A ABC____EA_
    ABC____FB_ ABC__FB__
    ABC_____BA ABC___B_A_
SIZE = 6
    ABC_B_BE___ ABCB__BE__
```

```
ABC_B__E_B_ ABC B__E_B
ABC_B__E_A ABC B__EA_
ABC_B___FB_ ABC B_FB___
ABC_B___BA ABC B__B_A_
ABC_B__E_B_ ABC___BE_B
ABC_B__E_A ABC___BEA_
ABC___BE_B_ ABC___BE_B
ABC___BE_A ABC___BEA_
ABC____EFB_ ABC_EFB___
ABC____FBA ABC__FB_A_
SIZE = 7
    ABC_B_BE_B_ ABC B__BE_B
    ABC_B_BE__A ABC B__BEA_
    ABC_B__EFB_ ABC BEFB___
    ABC_B___FBA ABC B_FB_A_
    ABC____EFBA ABC_EFB_A_
SIZE = 8
    ABC_B__EFBA ABC BEFB_A_
```

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING BRUTE FORCE: 0.00242 SECONDS

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING MEMOIZATION: 0.00003 SECONDS

6. 当然，暴力方法所花费的时间会受到打印子序列的额外步骤的影响。通过在将 DEBUG 常量设置为 0 后再次运行我们的代码，输出现在如下：

```
Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING BRUTE FORCE: 0.00055 SECONDS

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING MEMOIZATION: 0.00002 SECONDS
```

7. 当我们完成时，最长公共子序列的长度将包含在 DP[A.size()][B.size()] 中——即当 A 和 B 的前缀都等于整个字符串时 LCS 的值。因此，我们完整的 DP 逻辑写成如下：

```
string LCS_Tabulation(string A, string B)
{
    vector<vector<int>> DP(A.size() + 1, vector<int>(B.size() + 1));

    for(int i = 0; i <= A.size(); i++)
    {
        for(int j = 0; j <= B.size(); j++)
        {
            if(i == 0 || j == 0)
            {
                DP[i][j] = 0;
            }
            else if(A[i-1] == B[j-1])
            {
                DP[i][j] = DP[i-1][j-1] + 1;
            }
            else
            {
                DP[i][j] = max(DP[i-1][j], DP[i][j-1]);
            }
        }
    }

    int length = DP[A.size()][B.size()];
    ....
}
```

到目前为止，我们已经讨论了几种寻找最长公共子序列长度的方法，但如果我们还想输出其实际字符怎么办？当然，我们的暴力解决方案会做到这一点，但效率非常低；然而，使用前一个 DP 表中包含的结果，我们可以使用回溯轻松地重建 LCS。让我们突出显示我们在表中需要遵循的路径来实现这一点：

在 C++ 中，这可以编码如下：

```
string ReconstructLCS(vector<vector<int>> &DP, string &A, string &B, int i, int j)
{
    if(i == 0 || j == 0)
    {
        return "";
    }

    if(A[i-1] == B[j-1])
    {
        return ReconstructLCS(DP, A, B, i-1, j-1) + A[i-1];
    }
    else if(DP[i-1][j] > DP[i][j-1])
    {
        return ReconstructLCS(DP, A, B, i-1, j);
    }
    else
    {
        return ReconstructLCS(DP, A, B, i, j-1);
    }
}
```

9. 现在，我们可以在 LCS_Tabulation() 的最后一行返回 ReconstructLCS() 的结果：

```
string LCS_Tabulation(string A, string B)
{
    ....

    string lcs = ReconstructLCS(DP, A, B, A.size(), B.size());

    return lcs;
}
```

10. 我们 main() 中的代码现在应修改为适应 LCS_Tabulation() 的添加：

```
int main()
{
    string A, B;
    cin >> A >> B;
```

```
int tests = 3;

clock_t timer = clock();

for(int i = 0; i < tests; i++)
{
    int LCS;

    switch(i)
    {
        ....

        case 2:
        {
            string lcs = LCS_Tabulation(A, B);

            LCS = lcs.size();

            cout << "The longest common subsequence of " << A << " and " << B << " is: " << lcs << endl;

            break;
        }
    }
    cout << "Length of the longest common subsequence of " << A << " and " << B << " is: " << LCS << endl;

    GetTime(timer, types[i]);
}
return 0;
}
```

11. 使用字符串 ABCABDBEFBA 和 ABCBEFBEAB，你的程序输出应类似于此：

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING BRUTE FORCE: 0.00060 SECONDS

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING MEMOIZATION: 0.00005 SECONDS

The longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is: ABCBEFBA
Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING TABULATION: 0.00009 SECONDS

## Note

实际花费的时间值会根据你的系统而有所不同。请注意这些值之间的差异。

现在，我们已经查看了另一个详细的示例，说明如何使用不同技术将相同的逻辑应用于相同的问题，以及这对算法执行时间的影响。

## Activity 21: Melodic Permutations

我们首先要问自己的问题是：这个问题中一个单一的状态构成什么？

Base case --> Empty set:

- 1. 考虑旋律中的每个音符。
- 2. 对于之前遇到过的每个音符子集，要么追加当前音符，要么什么都不做。
- 3. 如果子集与目标匹配，则将其添加到解决方案中。

鉴于我们的选项是追加音符到之前子集或保持原样，我们可以重述逻辑如下：

对于旋律中的给定音符，包含该音符的大小为 | n | 的子集的计数等于不包含该音符的大小为 | n - 1 | 的所有子集的总计数。

因此，每个状态可以用两个维度表示：

- **Dimension 1**: 已考虑的旋律长度。
- **Dimension 2**: 通过取之前找到的子集并要么将其追加到旋律中索引 [length - 1] 处的音符，要么什么都不做而形成的 resulting 子集。





在伪代码中，逻辑可以表示如下：

```text
for i = 1 to length of melody (inclusive):

    for each subset previously found:
        DP(i, subset) = DP(i, subset) + DP(i - 1, subset)
        DP(i, subset U melody[i - 1]) = DP(i, subset U melody[i - 1]) + DP(i - 1, subset)
```

因此，现在首要的问题是，我们如何表示这些状态？

记住，对于一个包含 n 个元素的集合，总共有 2ⁿ 个子集组成它——例如，一个包含 4 个元素的集合可以被划分为总共 2⁴（或 16）个子集：

S = { A, B, C, D }

- { } -> { _ _ _ _ }
- { A } -> { # _ _ _ }
- { B } -> { _ # _ _ }
- { C } -> { _ _ # _ }
- { D } -> { _ _ _ # }
- { A, B } -> { # # _ _ }
- { A, C } -> { # _ # _ }
- { A, D } -> { # _ _ # }
- { B, C } -> { _ # # _ }
- { B, D } -> { _ # _ # }
- { C, D } -> { _ _ # # }
- { A, B, C } -> { # # # _ }
- { A, B, D } -> { # # _ # }
- { A, C, D } -> { # _ # # }
- { B, C, D } -> { _ # # # }
- { A, B, C, D } -> { # # # # }

如果我们从 0 到 (2⁴ − 1) 包含在内以二进制迭代，我们得到以下数字：

| Decimal | Binary | Subset |
|---:|:---:|:---:|
| 0 | 0000 | `{ _ _ _ _ }` |
| 1 | 0001 | `{ # _ _ _ }` |
| 2 | 0010 | `{ _ # _ _ }` |
| 3 | 0011 | `{ # # _ _ }` |
| 4 | 0100 | `{ _ _ # _ }` |
| 5 | 0101 | `{ # _ # _ }` |
| 6 | 0110 | `{ _ # # _ }` |
| 7 | 0111 | `{ # # # _ }` |
| 8 | 1000 | `{ _ _ _ # }` |
| 9 | 1001 | `{ # _ _ # }` |
| 10 | 1010 | `{ _ # _ # }` |
| 11 | 1011 | `{ # # _ # }` |
| 12 | 1100 | `{ _ _ # # }` |
| 13 | 1101 | `{ # _ # # }` |
| 14 | 1110 | `{ _ # # # }` |
| 15 | 1111 | `{ # # # # }` |

正如我们所见，从 0 到 2ⁿ − 1 的每个二进制数的位数恰好对应于 n 个元素的一个可能子集的索引。由于音阶中有 12 个音符，这意味着音符的可能子集总数为 2¹²（或 4,096）。通过将音阶中的每个音符映射为 2 的幂，我们可以使用按位算术来表示每个状态中遇到的子集。

以下是解决此活动的步骤：

1. 进入代码时，我们应该首先包含以下头文件：

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <map>

using namespace std;
```

2. 让我们开始在我们的 main() 函数中处理输入：

```cpp
int main()
{
    int melodyLength;
    int setLength;

    cin >> melodyLength;

    vector<string> melody(melodyLength);

    for(int i = 0; i < melodyLength; i++)
    {
        cin >> melody[i];
    }

    cin >> setLength;

    vector<string> set(setLength);

    for(int i = 0; i < setLength; i++)
    {
        cin >> set[i];
    }
    ....
}
```

3. 现在，让我们编写一个名为 ConvertNotes() 的函数，该函数接收音符字符串的向量作为输入并返回其对应整数值的向量。音阶中总共 12 个音符中的每一个都需要映射到特定的位（从 A 开始），同音等价的音符分配相同的值。我们将使用 std::map 来处理转换：

```cpp
vector<int> ConvertNotes(vector<string> notes)
{
    map<string, int> M =
    {
        { "A",  0 },
        { "A#", 1 },
        { "Bb", 1 },
        { "B",  2 },
        { "Cb", 2 },
```

5. 我们的第一步是定义我们的目标子集。我们将使用按位或运算符来做到这一点：

```cpp
unsigned int target = 0;

for(auto note : set)
{
    target |= note;
}
```

6. 例如，如果我们的目标集合是 { C, F#, A }，映射将如下所示：

```text
C = 3
F# = 9
A = 0

converted = { 2³, 2⁹, 2⁰ } = { 8, 512, 1 }

target = (8 | 512 | 1) = 521

0000001000
+ 0000000001
+ 1000000000
= 1000001001
```

7. 我们现在将定义一个二维 DP 表，第一维初始化为 melodyLength + 1，第二维初始化为最大子集值加一（即 `111111111111` = 2¹² - 1，因此第二维将包含 2¹²，或 4,096 个元素）：

```cpp
vector<vector<int>> DP(melody.size() + 1, vector<int>(4096, 0));
```

8. 我们的 DP 公式可以定义如下：

Base case：

```text
DP(0, 0) -> 1
```

Recurrence：

```text
DP(i, subset) -> DP(i, subset) + DP(i - 1, subset)
DP(i, subset U note[i - 1]) -> DP(i, subset U note[i - 1]) + DP(i - 1, subset)
```

这里，i 从 1 到旋律的长度。我们可以将前述逻辑用 C++ 编写如下：

```cpp
// Base case -> empty set
DP[0][0] = 1;

for(int i = 1; i <= melody.size(); i++)
{
    for(unsigned int subset = 0; subset < 4096; subset++)
    {
        // Keep results for previous values of i
        DP[i][subset] += DP[i-1][subset];

        // Add results for union of subset with melody[i-1]
        DP[i][subset | melody[i-1]] += DP[i-1][subset];
    }
}

// Solution
return DP[melody.size()][target];
```

9. 现在，我们可以通过调用 CountMelodicPermutations 并输出结果来完成我们的 main() 函数：

```cpp
int count = CountMelodicPermutations(ConvertNotes(melody), ConvertNotes(set));

cout << count << endl;
```

```cpp
products.push_back(Product(quantity, cost, value));
}
...

return 0;
}
```

4. 与任何 DP 算法一样，我们现在必须定义状态和基本情况。我们知道形成最终结果的项目的子集必须符合以下标准：

- 该子集中所有产品的成本之和不得超过预算。
- 该子集中所有产品的数量之和不得超过容量。
- 该子集中所有产品的价值之和必须最大化。

鉴于这些标准，我们可以看到每个状态可以由以下参数定义：

- 当前正在考虑的项目
- 先前购买的单位数
- 已购买项目的总成本
- 以零售价出售产品后获得的总利润

我们还可以得出结论，搜索将在以下情况下终止：

- 所有项目都已考虑
- 总成本超过预算
- 单位总数超过容量

与传统的 0-1 背包问题一样，我们将从 0 到 N-1 线性考虑每个项目。对于索引为 i 的每个项目，我们的状态可以在两种方式之一转换：要么包含当前项目，要么将其保留。用伪代码编写递归逻辑可能如下所示：

```text
F(i, count, cost, total):

I      -> 当前项目的索引
Cost   -> 花费的总金额
count  -> 购买的单位数
total  -> 所选项目的总利润值
```

Base cases：

```text
if i = N: return total
if cost > budget: return 0
if count > capacity: return 0
```

Recurrence：

F(i, count, cost, total) = maximum of:

```text
F(i + 1, count + quantity[i], cost + price[i], total + value[i]) - 包含该项目
```

AND

```text
F(i + 1, count, cost, total) - 保持原样
```

如前面的代码所示，递推关系是根据 i、count、cost 和 total 的值定义的。将此逻辑从自顶向下转换为自底向上可以这样做：

Base case：

```text
DP(0, 0, 0) = 0 [尚未选择任何内容]

For i = 1 to N:
    Product -> quantity, price, value

For cost = 0 to budget:
    For count = 0 to capacity:
```

现在，让我们看一下实现：

1. 包含所需的头文件并定义一些全局常量和变量，我们将在本问题中稍后需要。我们将出于方便起见在全局声明我们的大部分数据，但值得重申的是，在完整规模应用程序的上下文中，这通常被认为是不良做法：

```cpp
#include <iostream>
#include <vector>
using namespace std;

const int UNKNOWN = 1e9;
const char EMPTY_SPACE = '.';
const string roads = "-/\\";

struct Point
{
    int x;
    int y;

    Point(){}
    Point(int x, int y) : x(x), y(y) {}
};

int N;
int H, W;

vector<string> grid;
vector<vector<int>> terrain;
vector<vector<int>> cost;
vector<Point> houses;
```

## Step 1: Handling the Input

2. 由于此问题需要相当多的输入，让我们将其全部包含在其自己的函数 `Input()` 中，该函数将返回 void：

```cpp
void Input()
{
    cin >> H >> W;
    cin >> N;

    grid.resize(H);
    houses.resize(N);
```





```
terrain.resize(H, vector<int>(W, UNKNOWN));    cost.resize(H,
vector<int>(W, UNKNOWN));

// Map of property
for(auto & row : grid) cin >> row;

// Terrain ruggedness
for(int i = 0; i < H; i++)
{
    for(int j = 0; j < W; j++)
    {
        cin >> terrain[i][j];
    }
}

// House coordinates
for(int i = 0; i < N; i++)
{
    cin >> houses[i].x >> houses[i].y;

    // Set house labels in grid
    grid[houses[i].y][houses[i].x] = char(i + 'A');
}
}
```

## 第 2 步：构建图

问题描述说明如下：

- 当且仅当两座房屋之间存在直接的水平、垂直或对角线路径时，才可以在它们之间修建道路。
- 道路不得穿越水体、山脉、森林等。
- 两座房屋之间修建道路的成本等于它们之间路径上的崎岖度值之和。

要测试第一个条件，我们只需比较两点的坐标，并判断以下三个条件中是否有任何一个成立：

- `A.x = B.x`（它们之间有一条水平线）
- `A.y = B.y`（它们之间有一条垂直线）
- `| A.x - B.x | = | A.y - B.y |`（它们之间有一条对角线）

现在，让我们回到代码中。

- 3. 为此，让我们编写一个函数 `DirectLine()`，它接受两个点 `a` 和 `b` 作为参数，并返回一个布尔值：

```
bool DirectLine(Point a, Point b)
{
    return a.x == b.x || a.y == b.y || abs(a.x - b.x) == abs(a.y - b.y);
}
```

- 4. 为了处理第二种和第三种情况，我们可以简单地在网格中从点 `a` 到点 `b` 执行线性遍历。当我们依次考虑网格中的每个点时，我们可以累加 `terrain` 矩阵中包含的值之和。在此过程中，我们可以同时检查 `grid[a.y][a.x]` 中的字符，一旦遇到不等于 `EMPTY_SPACE`（即 `'.'`）的字符就终止。如果在遍历结束时点 `a` 等于点 `b`，我们将获得的累加值存储到 `cost` 矩阵中；否则，我们已经确定 `a` 和 `b` 之间不存在邻接关系，此时返回 `UNKNOWN`。我们可以使用 `GetCost()` 函数完成此操作，它接受两个整数 `start` 和 `end` 作为参数，分别表示 `a` 和 `b` 的索引，并返回一个整数：

```
int GetCost(int start, int end)
{
    Point a = houses[start];
    Point b = houses[end];

    // The values by which the coordinates change on each iteration
    int x_dir = 0;
    int y_dir = 0;

    if(a.x != b.x)
    {
        x_dir = (a.x < b.x) ? 1 : -1;
    }
    if(a.y != b.y)
    {
        y_dir = (a.y < b.y) ? 1 : -1;
    }
    int cost = 0;
```

```
```cpp
do
{
    a.x += x_dir;
    a.y += y_dir;

    cost += terrain[a.y][a.x];
}
while(grid[a.y][a.x] == ',');

return (a != b) ? UNKNOWN : res;
}
```
```

5. 最后一行要求我们在 **Point** 结构体中定义 **operator !=**：

```
```cpp
struct Point
{
    ......

    bool operator !=(const Point &other) const { return x != other.x || y
!= other.y; }
}
```
```

6. 现在，让我们创建如下的 **GetAdjacencies()** 函数：

```
```cpp
void GetAdjacencies()
{
    for(int i = 0; i < N; i++)
    {
        for(int j = 0; j < N; j++)
        {
            if(DirectLine(houses[i], houses[j])
            {
                cost[i][j] = cost[j][i] = GetCost(i, j);
            }
        }
    }
}
```
```

```
return (y_dir == 1) ? 3 : 2;
}

void DrawPath(int start, int end)
{
    .....

    int direction = GetDirection(x_dir, y_dir);

    char mark = roads[direction];

    .....
}
```

12. 现在我们可以从 `a` 到 `b` 执行线性遍历，如果网格中的某个单元格值为 **EMPTY_SPACE**，则将其设置为 **mark**。否则，我们必须检查该单元格中的字符是否是方向不同的道路字符，如果是，则将其设置为 `'+'`：

```
do
{
    a.x += x_dir;
    a.y += y_dir;

    if(grid[a.y][a.x] == EMPTY_SPACE)
    {
        grid[a.y][a.x] = mark;
    }
    else if(!isalpha(grid[a.y][a.x]))
    {
        // If two roads of differing orientations intersect, replace
        // symbol with '+'

        grid[a.y][a.x] = (mark != grid[a.y][a.x]) ? '+' : mark;
    }
}
while(a != b);
```

13. 剩下要做的就是在 `main()` 中调用我们的函数并打印输出：

```
```cpp
int main()
{
        Input();
        BuildGraph();
        GetShortestPaths();

        for(auto it : grid)
        {
                cout << it << endl;
        }
        return 0;
}
```
```

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_616_0.png)
