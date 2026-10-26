

5.  现在，我们需要一个函数来对图执行深度优先搜索。按如下方式实现：

    ```
    template <typename T>
    auto depth_first_search(const Graph<T>& G, size_t dest)
    {
        std::stack<size_t> stack;
        std::vector<size_t> visit_order;
        std::set<size_t> visited;
        stack.push(1); // Assume that DFS always starts from vertex ID 1

        while (!stack.empty())
        {
            auto current_vertex = stack.top();
            stack.pop();

            // If the current vertex hasn't been visited in the past
            if (visited.find(current_vertex) == visited.end())
            {
                visited.insert(current_vertex);
                visit_order.push_back(current_vertex);

                for (auto e : G.outgoing_edges(current_vertex))
                {
                    // If the vertex hasn't been visited, insert it in the stack.
                    if (visited.find(e.dest) == visited.end())
                    {
                        stack.push(e.dest);
                    }
                }
            }
        }

        return visit_order;
    }
    ```

6.  我们将在下图所示的图上测试DFS的实现：

    ![](img/b37c7e7c5fa2518dbf1afd10524f8d11_282_0.png)

    图6.13：用于在练习29中实现DFS遍历的图

    使用以下函数来创建并返回该图：

    ```
    template <typename T>
    auto create_reference_graph()
    {
        Graph<T> G(9);

        std::map<unsigned, std::vector<std::pair<size_t, T>>> edges;
        edges[1] = { {2, 0}, {5, 0} };
        edges[2] = { {1, 0}, {5, 0}, {4, 0} };
        edges[3] = { {4, 0}, {7, 0} };
        edges[4] = { {2, 0}, {3, 0}, {5, 0}, {6, 0}, {8, 0} };
        edges[5] = { {1, 0}, {2, 0}, {4, 0}, {8, 0} };
        edges[6] = { {4, 0}, {7, 0}, {8, 0} };
        edges[7] = { {3, 0}, {6, 0} };
        edges[8] = { {4, 0}, {5, 0}, {6, 0} };

        for (auto& i : edges)
            for (auto& j : i.second)
                G.add_edge(Edge<T>{ i.first, j.first, j.second });

        return G;
    }
    ```

    注意这里对边权重使用了空值，因为DFS不需要边权重。一个更简单的图实现完全可以省略边权重，这不会影响我们的DFS算法的行为。

7.  最后，添加以下测试和驱动程序代码，它们将运行我们的DFS实现并打印输出：

    ```
    template <typename T>
    void test_DFS()
    {
        // Create an instance of and print the graph
        auto G = create_reference_graph<unsigned>();
        std::cout << G << std::endl;

        // Run DFS starting from vertex ID 1 and print the order
        // in which vertices are visited.
        std::cout << "DFS Order of vertices: " << std::endl;
        auto dfs_visit_order = depth_first_search(G, 1);
        for (auto v : dfs_visit_order)
            std::cout << v << std::endl;
    }

    int main()
    {
        using T = unsigned;
        test_DFS<T>();

        return 0;
    }
    ```

8.  编译并运行上述代码。你的输出应如下所示：

    ![](img/b37c7e7c5fa2518dbf1afd10524f8d11_283_0.png)

    图6.14：练习29的预期输出

下图展示了我们的DFS实现访问各顶点的顺序：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_284_0.png)

图6.15：顶点访问顺序及DFS的方向

BFS和DFS的时间复杂度均为O(V + E)。然而，这两种算法之间有几个重要的区别。以下列表总结了它们之间的区别，并提出了一些应优先选用其中一种算法的情况：

- BFS更适合查找离源顶点较近的顶点，而DFS通常更适合查找离源较远的顶点。

- 在BFS中，一旦访问了某个顶点，从源顶点到该顶点所找到的路径保证是最短路径，而DFS则没有这样的保证。这就是为什么所有单源和多源最短路径算法都使用某种BFS变体的原因。这将在本章接下来的章节中探讨。

- 由于BFS会访问当前边界上的所有相邻顶点，因此BFS生成的搜索树又矮又宽，需要相对更多的内存；而DFS生成的搜索树又长又窄，需要相对更少的内存。

## 活动13：使用DFS判断图是否为二分图

二分图是这样一种图：其顶点可以被划分为两个集合，使得图中的任意一条边都必须连接一个集合中的顶点与另一个集合中的顶点。

二分图可以用于建模多种不同的实际用例。例如，如果我们有一份学生名单和一份课程名单，学生与课程之间的关系可以被建模为二分图：如果某名学生注册了某门课程，就在该学生与该课程之间建立一条边。正如你所想，学生到学生或课程到课程的边是没有意义的，因此这类边在二分图中是不允许存在的。下图展示了这样一个模型：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_285_0.png)

图6.16：表示学生注册不同课程的二分图示例

一旦准备好了如上图所示的模型，就可以用它来创建课程时间表，使得同一名学生注册的任何两门课程不会重叠。例如，如果Jolene注册了数学和计算机科学，那么这两门课程就不应该被安排在同一时间，以避免冲突。在时间表中最小化此类冲突可以通过求解图中的最大流问题来实现。已知的最大流问题标准算法有Ford-Fulkerson算法、Dinic算法和推送-重标记算法等。然而，这些算法通常较为复杂，因此超出了本书的范围。

使用二分图建模实体间关系的另一个用例，是大型视频流媒体平台（如Netflix和YouTube）维护的观众与电影列表之间的关系。

二分图的一个有趣性质是：某些操作（如寻找最大匹配和顶点覆盖）在一般图中是NP完全的，但可以在多项式时间内于二分图中求解。因此，判断给定图是否为二分图是很有用的。在本活动中，你需要实现一个C++程序，用于检查给定图G是否为二分图。

二分图检查算法使用略微修改过的DFS版本，其工作方式如下：

1.  假设DFS从顶点1开始。将顶点ID 1添加到栈中。
2.  如果栈上还有未访问的顶点，则从栈中弹出一个顶点并将其设为当前顶点。
3.  如果分配给父顶点的颜色是蓝色，则将当前顶点分配为红色；否则，将当前顶点分配为蓝色。
4.  将当前顶点的所有未访问相邻顶点添加到栈中，并将当前顶点标记为已访问。
5.  重复步骤2、3和4，直到所有顶点都被分配了颜色。如果算法终止时所有顶点都已着色，则给定图是二分图。
6.  如果在执行步骤2时，搜索遇到一个已被访问且已分配颜色的顶点，而该颜色与步骤3中本应分配的颜色（搜索树中其父顶点所分配颜色的相反色）不同，则算法立即终止，并判定给定图不是二分图。

下图说明了上述算法的工作过程：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_287_0.png)

图6.17：初始化

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_287_1.png)

图6.18：由于顶点1被分配为蓝色，我们将顶点2涂为红色

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_288_0.png)

集合A　　　　　　　　　　　　　　集合B

图6.19：由于顶点2被涂为红色，我们将顶点8涂为蓝色。

从前面的这组图可以观察到，算法在图中曲折前进，为每个访问到的顶点交替分配颜色。如果所有顶点都能以这种方式着色，则该图是二分图。如果DFS到达两个已经以该方式着色的顶点，则可以安全地判定该图不是二分图。

