

使用图 6.17 作为输入，你的最终输出应如下所示：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_289_0.png)

图 6.20：活动 13 的预期输出

## Note

本活动的解决方案可在第 524 页找到。

## Prim's MST Algorithm

MST 问题在第 5 章 贪心算法 中引入，定义如下：

> “给定一个图，G = < V, E >，其中 V 是顶点集合，E 是与边权重相关联的边集合，找到一棵树 T，使其跨越 V 中的所有顶点并且具有最小的总权重。”

在第 5 章 贪心算法 中，我们讨论了 MST 问题的实际应用以及 Kruskal 算法，该算法可在给定图中找到 MST。Kruskal 算法将图的全部边添加到最小堆中，并贪心地将最小成本边添加到 MST 中，在每次添加时检查树中是否形成环。

Prim 算法（也称为 Jarník 算法）的思想与 BFS 类似。该算法首先将起始顶点添加到 frontier 中，frontier 由之前访问过的顶点集合组成，然后迭代地探索当前 frontier 相邻的顶点。然而，在每次迭代中选择要访问的顶点时，会选择从 frontier 出发边成本最低的顶点。

在实现 Prim 算法时，我们为图的每个顶点附加一个标签，该标签存储其与起始顶点的距离。该算法的工作方式如下：

1. 首先，它初始化所有顶点的标签并将所有距离设置为无穷大。由于起始顶点到自身的距离为 0，因此它将起始顶点的标签设置为 0。然后，它将所有标签添加到最小堆 H 中。

在下图中，红色显示的数字代表从起始顶点（假设为顶点 1）估计的距离；黑色显示的数字代表边权重：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_290_0.png)

图 6.21：初始化 Prim 的 MST 算法

2. 接下来，它从 H 中弹出一个顶点 U。自然地，U 是从起始顶点距离最小的顶点。

3. 对于与 U 相邻的所有顶点 V，如果 V 的标签 > (U, V) 的边权重，则设置 V 的标签 = (U, V) 的边权重。此步骤称为 settling 或访问顶点 U：

图 6.22：访问顶点 1 后图的状况

4. 当图中仍有未访问的顶点时，返回步骤 2。下图显示了访问顶点 2 后图的状况，其中绿色显示的边是我们迄今为止 MST 中的唯一边：

图 6.23：访问顶点 2 后图的状况

5. 所有顶点都已 settle 后的最终 MST 显示如下：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_292_0.png)

图 6.24：我们图的 MST

### Exercise 30: Prim's Algorithm

在本练习中，我们将实现 Prim 算法以在图 6.22 所示的图中查找 MST。按照以下步骤完成本练习：

1. 添加所需的头文件，如下所示：

```cpp
#include <set>
#include <map>
#include <queue>
#include <limits>
#include <string>
#include <vector>
#include <iostream>
```

2. 使用以下结构体通过使用以下结构体实现图中的边：

```cpp
template<typename T> class Graph;

template<typename T>
struct Edge
{
    size_t src;
    size_t dest;
    T weight;

    // To compare edges, only compare their weights,
    // and not the source/destination vertices
    inline bool operator< (const Edge<T>& e) const
    {
        return this->weight < e.weight;
    }

    inline bool operator> (const Edge<T>& e) const
    {
        return this->weight > e.weight;
    }
};
```

3. 使用以下函数重载 Graph 类的 << 运算符，以便我们可以将图输出到 C++ 流中：

```cpp
template <typename T>
std::ostream& operator<<(std::ostream& os, const Graph<T>& G)
{
    for (auto i = 1; i <= G.vertices(); i++)
    {
        os << i << ":\t";

        auto edges = G.outgoing_edges(i);
        for (auto& e : edges)
            os << "{" << e.dest << ": " << e.weight << "}, ";

        os << std::endl;
    }

    return os;
}
```

4. 添加基于边列表的图实现，如下所示：

```cpp
template<typename T>
class Graph
{
public:
    // Initialize the graph with N vertices
    Graph(size_t N) : V(N)
    {}

    // Return number of vertices in the graph
    auto vertices() const
    {
        return V;
    }

    // Return all edges in the graph
    auto& edges() const
    {
        return edge_list;
    }

    void add_edge(Edge<T>&& e)
    {
        // Check if the source and destination vertices are within range
        if (e.src >= 1 && e.src <= V &&
            e.dest >= 1 && e.dest <= V)
            edge_list.emplace_back(e);
        else
            std::cerr << "Vertex out of bounds" << std::endl;
    }

    // Returns all outgoing edges from vertex v
    auto outgoing_edges(size_t v) const
    {
        std::vector<Edge<T>> edges_from_v;
        for (auto& e : edge_list)
        {
            if (e.src == v)
                edges_from_v.emplace_back(e);
        }
        return edges_from_v;
    }

private:
    size_t V;
    std::vector<Edge<T>> edge_list;
};
```

6. 接下来，我们将实现 **Label** 结构，其实例分配给图中的每个顶点以存储其与 frontier 的距离。使用以下代码执行此操作：

```cpp
template<typename T>
struct Label
{
    size_t vertex_ID;
    T distance_from_frontier;

    Label(size_t _id, T _distance) :
        vertex_ID(_id),
        distance_from_frontier(_distance)
    {}

    // To compare labels, only compare their distances from source
    inline bool operator< (const Label<T>& l) const
    {
        return this->distance_from_frontier < l.distance_from_frontier;
    }

    inline bool operator> (const Label<T>& l) const
    {
        return this->distance_from_frontier > l.distance_from_frontier;
    }

    inline bool operator() (const Label<T>& l) const
    {
        return *this > l;
    }
};
```

7. 编写一个函数来实现 Prim 的 MST 算法，如下所示：

```cpp
template <typename T>
auto prim_MST(const Graph<T>& G, size_t src)
{
    std::priority_queue<Label<T>, std::vector<Label<T>>,
                        std::greater<Label<T>>> heap;
    std::set<int> visited;

    std::vector<T> distance(G.vertices(), std::numeric_limits<T>::max());
    std::vector<size_t> MST;

    heap.emplace(src, 0);

    // Search for the destination vertex in the graph
    while (!heap.empty())
    {
        auto current_vertex = heap.top();
        heap.pop();

        // If the current vertex hasn't been visited in the past
        if (visited.find(current_vertex.vertex_ID) == visited.end())
        {
            std::cout << "Settling vertex ID " << current_vertex.vertex_ID << std::endl;
            MST.push_back(current_vertex.vertex_ID);

            // For each outgoing edge from the current vertex,
            // create a label for the destination vertex and add it to the heap
            for (auto e : G.outgoing_edges(current_vertex.vertex_ID))
            {
                auto neighbor_vertex_ID = e.dest;
                auto new_distance_to_frontier = e.weight;

                // Check if the new path to the vertex is shorter
                // than the previously known best path.
                // If yes, update the distance
                if (new_distance_to_frontier < distance[neighbor_vertex_ID])
                {
                    heap.emplace(neighbor_vertex_ID, new_distance_to_frontier);
                    distance[e.dest] = new_distance_to_frontier;
                }
            }

            visited.insert(current_vertex.vertex_ID);
        }
    }

    return MST;
}
```

8. 最后，添加以下代码，该代码运行我们对 Prim 算法的实现并输出结果：

```cpp
template<typename T>
void test_prim_MST()
{
    auto G = create_reference_graph<T>();
    std::cout << G << std::endl;

    auto MST = prim_MST<T>(G, 1);

    std::cout << "Minimum Spanning Tree:" << std::endl;
    for (auto v : MST)
        std::cout << v << std::endl;
    std::cout << std::endl;
}

int main()
{
    using T = unsigned;
    test_prim_MST<T>();

    return 0;
}
```

9. 运行程序。你的输出应如下所示：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_298_0.png)

图 6.25：练习 30 的输出

