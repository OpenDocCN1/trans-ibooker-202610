

## 6 图算法 I

### 学习目标

学完本章后，你将能够：

- 描述图在解决各种现实问题中的用途
- 选择并实现合适的遍历方法，以在图中查找元素
- 使用 Prim 算法解决最小生成树（MST）问题
- 判断在解决 MST 问题时应当使用 Prim 算法还是 Kruskal 算法
- 使用 Dijkstra 算法找到图中两个顶点/节点之间的最短路径

在本章中，我们将学习用于解决可表示为图的问题的基础且最常用的算法；这些算法将在下一章进一步讨论。

### 引言

在前两章中，我们讨论了两种算法设计范式：分治（divide and conquer）和贪心（greedy approach），并由此得到了排序、搜索、寻找图的最小权重生成树等广泛使用且重要的计算问题的著名解决方案。在本章中，我们将讨论一些专门适用于图数据结构的算法。

图被定义为一组顶点和连接成对顶点的边。数学上常写作 G = < V, E >，其中 V 表示顶点集合，E 表示构成图的边集合。从一个节点指向另一个节点的边称为有向边，没有方向的边称为无向边。边也可以带权重或不带权重，正如我们在第 2 章《树、堆和图》中看到的。

**注意**

在讨论图时，“节点”和“顶点”这两个术语可以互换使用。在本章中，我们将统一使用“顶点”。

图是最通用的数据结构之一，以至于树和链表等其他链式数据结构都只是图的特例。图的实用性在于，它们是对象（表示为节点）之间关系（表示为边）的一般表示。图中同一对节点之间可以有多条边，甚至一条边上可以有多个权重，节点也可以有从自身到自身的边（也称为自边）。下图展示了这些特征在一个图中的存在方式。图的变体，称为“超图”（hypergraphs），允许连接多个节点的边；另一类变体称为“混合图”（mixed graphs），允许在同一图中同时存在有向边和无向边：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_264_0.png)

**图 6.1：** 一个具有多个边权重、自边（也称为环）以及有向和无向边的图

由于图具有高度的通用性，它们在多个领域都有应用。理论计算机科学家使用图来建模有限状态机和自动机；人工智能和机器学习专家使用图从不同类型网络的结构随时间变化中提取信息；交通工程师使用图来研究道路网络中的交通流。

在本章中，我们将只研究使用加权有向图的算法，必要时使用正边权。我们首先研究**图遍历问题**，并介绍它的两种解决方案：**广度优先搜索（BFS）**和**深度优先搜索（DFS）**。接下来，我们将回到上一章介绍的最小生成树问题，并给出另一种解决方案——Prim 算法。最后，我们将介绍单源最短路径问题，它为 Google Maps 和 OSRM 路线规划器等导航应用提供了支撑。

让我们从图遍历的基本问题开始。

### 图遍历问题

假设你最近搬到了一个新社区的一间公寓。当你认识新邻居、结交新朋友时，人们经常推荐附近可以去吃饭的餐馆。你想去所有被推荐的餐馆，于是你拿出一张社区地图，在上面标出所有餐馆和你的家，而地图上已经标好了所有的道路。如果我们将每家餐馆和你的家表示为顶点，将连接餐馆的道路表示为图中的边，那么从给定顶点出发访问图中所有顶点的问题就称为图遍历问题。

在下图中，蓝色的数字假定为顶点 ID。顶点 1 是家，餐馆标记为 R1 到 R7。所有边都没有箭头，因为边被假定为双向的，也就是说，你可以沿任一方向在路上行驶：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_265_0.png)

图 6.2：将社区地图表示为图

用数学记号表示，给定一个图 G = < V, E >，图遍历问题就是要从给定顶点 s 出发访问所有 v ∈ V。图遍历问题也被称为图搜索问题，因为它可以用来“查找”图中的顶点。不同的图遍历算法会给出访问图中顶点的不同顺序。

2. 下图显示了在访问 R1 和 R2 之后的 BFS 状态，这两个顶点谁先被访问都可以。与源顶点距离相同的顶点的访问顺序无关紧要；但是，距离源顶点较近的顶点总是先被访问：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_267_0.png)

图 6.4：访问 R1 和 R2 顶点后的 BFS 前沿

3. 下图显示了访问 R3、R5 和 R6 之后的 BFS 状态。这基本上是在整张图被遍历之前的倒数第二个阶段：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_267_1.png)

图 6.5：访问 R3、R5 和 R6 顶点后的 BFS 前沿

BFS 的一个有用特性是，对于每一个被访问的顶点，它的所有子顶点都会在任何孙顶点之前被访问。然而，在实现 BFS 时，前沿通常不会显式地维护在单独的数据结构中。相反，我们使用一个顶点 ID 队列来确保离源顶点较近的顶点总是在较远的顶点之前被访问。在下面的练习中，我们将用 C++ 实现 BFS。

### 练习 28：实现 BFS

在本练习中，我们将使用图的边列表表示来实现广度优先搜索算法。为此，请执行以下步骤：

1. 添加所需的头文件并声明图，如下所示：

```
#include <string>
#include <vector>
#include <iostream>
#include <set>
#include <map>
#include <queue>

template<typename T> class Graph;
```

2. 编写以下结构体，它表示我们的图中的一条边：

```
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

由于我们的边定义使用了模板，因此可以很容易地将边的权重设置为所需的任何数据类型。

3. 接下来，为 **Graph** 数据类型重载 `<<` 运算符，以显示图的内容：

```
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

4. 编写一个类来定义我们的图数据结构，如下所示：

```
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
