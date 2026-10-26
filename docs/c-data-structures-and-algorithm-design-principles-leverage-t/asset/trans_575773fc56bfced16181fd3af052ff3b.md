
使用二叉最小堆和邻接表存储最小生成树时，Prim算法的时间复杂度为 O(E log V)，而当使用一种称为“斐波那契最小堆”的堆时，可改进至 O(E + V log V)。

虽然Prim算法和Kruskal算法都是贪心算法的例子，但它们在重要方面存在差异，下面列出其中一些：

| Kruskal算法 | Prim算法 |
|---|---|
| 通过将图中代价最小的边加入MST来开始构建MST | 从图中的任意起始顶点开始构建MST |
| 目前最佳时间复杂度为 O(E log V) | 目前最佳时间复杂度为 O(E + V log V) |
| 通常适用于边数较少的稀疏图 | 通常适用于边数较多的稠密图 |

图6.26：Kruskal算法与Prim算法的比较表

## Dijkstra最短路径算法

每当用户在路线规划应用（如Google Maps）或汽车内置导航软件中请求路线时，图上的单源最短路径问题就被求解一次。该问题定义如下：

> “给定一个有向图 G = < V, E >，其中 V 是顶点集合，E 是边集合，每条边关联一个边权重、一个源顶点和一个目标顶点，找出从源到目标的一条最小代价路径。”

Dijkstra算法适用于具有非负边权的图，它只是对Prim最小生成树算法稍作修改，主要有两处变化：

- 与在每个顶点上设置等于边界最小距离的标签不同，Dijkstra算法在每个顶点上设置的距离等于该顶点到源点的总距离。
- 如果目标顶点从堆中弹出，Dijkstra算法即终止；而Prim算法只有在堆上不再有需要处理的顶点时才终止。

该算法的运行过程通过以下步骤说明：

- 1. 首先，它初始化所有顶点上的标签，并将所有距离设为无穷大。由于起始顶点到自身的距离为0，它将起始顶点的标签设为0。然后，将所有标签加入一个最小堆 H。

在下图中，红色数字表示从源点（顶点2）到目标点（顶点6）的当前已知最短距离：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_300_0.png)

图6.27：初始化Dijkstra算法

- 2. 然后，从 H 中弹出一个顶点 U。自然地，U 是距离起始顶点最近的顶点。如果 U 就是所需的目标顶点，则我们已找到最短路径，算法终止。

- 2. 编写以下 struct 来实现图实现中的一条边：

```cpp
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

- 3. 重载 Graph 类的 << 运算符，使其可以通过流输出，如下所示：

```cpp
template <typename T>
std::ostream& operator<<(std::ostream& os, const Graph<T>& G)
{
    for (auto i = 1; i < G.vertices(); i++)
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

- 4. 实现图，如下所示：

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

    // Overloads the << operator so a graph can be written directly to a
    // stream
    // Can be used as std::cout << obj << std::endl;
    template <typename T>
    friend std::ostream& operator<< <>(std::ostream& os, const Graph<T>& G);

private:
    size_t V;              // Stores number of vertices in graph
    std::vector<Edge<T>> edge_list;
};
```

- 5. 编写一个函数，使用**Graph**类创建*图6.28*所示的参考图，如下所示：

```cpp
template <typename T>
auto create_reference_graph()
{
    Graph<T> G(9);

    std::map<unsigned, std::vector<std::pair<size_t, T>>> edges;
    edges[1] = { {2, 2}, {5, 3} };
    edges[2] = { {1, 2}, {5, 5}, {4, 1} };
    edges[3] = { {4, 2}, {7, 3} };
    edges[4] = { {2, 1}, {3, 2}, {5, 2}, {6, 4}, {8, 5} };
    edges[5] = { {1, 3}, {2, 5}, {4, 2}, {8, 3} };
    edges[6] = { {4, 4}, {7, 4}, {8, 1} };
    edges[7] = { {3, 3}, {6, 4} };
    edges[8] = { {4, 5}, {5, 3}, {6, 1} };

    for (auto& i : edges)
        for (auto& j : i.second)
            G.add_edge(Edge<T>{ i.first, j.first, j.second });

    return G;
}
```

- 6. 实现Dijkstra算法，如下所示：

```cpp
template <typename T>
auto dijkstra_shortest_path(const Graph<T>& G, size_t src, size_t dest)
{
    std::priority_queue<Label<T>, std::vector<Label<T>>,
    std::greater<Label<T>>> heap;
    std::set<int> visited;
    std::vector<size_t> parent(G.vertices());
    std::vector<T> distance(G.vertices(), std::numeric_limits<T>::max());
    std::vector<size_t> shortest_path;

    heap.emplace(src, 0);
    parent[src] = src;

    // Search for the destination vertex in the graph
    while (!heap.empty()) {
        auto current_vertex = heap.top();
        heap.pop();

        // If the search has reached the destination vertex
        if (current_vertex.vertex_ID == dest) {
            std::cout << "Destination " <<
            current_vertex.vertex_ID << " reached." << std::endl;
            break;
        }
        if (visited.find(current_vertex.vertex_ID) == visited.end()) {
            std::cout << "Settling vertex " <<
            current_vertex.vertex_ID << std::endl;
            // For each outgoing edge from the current vertex,
            // create a label for the destination vertex and add it to the heap
            for (auto e : G.outgoing_edges(current_vertex.vertex_ID)) {
                auto neighbor_vertex_ID = e.dest;
                auto new_distance_to_dest = current_vertex.distance_from_source
                + e.weight;

                // Check if the new path to the destination vertex
                // has a lower cost than any previous paths found to it, if yes, then
                // this path should be preferred
                if (new_distance_to_dest < distance[neighbor_vertex_ID]) {
                    heap.emplace(neighbor_vertex_ID, new_distance_to_dest);

                    parent[e.dest] = current_vertex.vertex_ID;
                    distance[e.dest] = new_distance_to_dest;
                }
            }
            visited.insert(current_vertex.vertex_ID);
        }
    }
    // Construct the path from source to the destination by backtracking
    // using the parent indexes
    auto current_vertex = dest;
    while (current_vertex != src) {
        shortest_path.push_back(current_vertex);
        current_vertex = parent[current_vertex];
    }
    shortest_path.push_back(src);
    std::reverse(shortest_path.begin(), shortest_path.end());
    return shortest_path;
}
```

我们的实现分两个阶段工作——它从源点开始搜索目标顶点，并使用回溯阶段，通过从目标点沿父指针回到源点来找到最短路径。

- 7. 最后，添加以下代码，通过查找图中顶点1和6之间的最短路径来测试我们Dijkstra算法的实现：

```cpp
template<typename T>
void test_dijkstra()
{

auto G = create_reference_graph<T>();
std::cout << "Reference graph:" << std::endl;
std::cout << G << std::endl;

auto shortest_path = dijkstra_shortest_path<T>(G, 1, 6);

std::cout << "The shortest path between 1 and 6 is:" << std::endl;
for (auto v : shortest_path)
    std::cout << v << " ";
```
