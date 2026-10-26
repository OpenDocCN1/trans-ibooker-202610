
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

