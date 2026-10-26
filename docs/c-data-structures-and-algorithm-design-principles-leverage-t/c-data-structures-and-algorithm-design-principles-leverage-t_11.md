## 第5章：贪心算法

### 活动11：区间调度问题

在本活动中，我们将找到任务的最优调度方案，以最大化能够完成的任务数量。按照以下步骤完成本活动：

- 1. 添加所需的头文件，并如下定义 `Task` 结构体：

```cpp
#include <list>
#include <algorithm>
#include <iostream>
#include <random>

// Every task is represented as a pair <start_time, end_time>
struct Task
{
    unsigned ID;
    unsigned start_time;
    unsigned end_time;
};
```

- 2. 以下函数可用于生成包含 N 个随机数据任务的任务列表：

```cpp
auto initialize_tasks(size_t num_tasks)
{
    std::random_device rd;
    std::mt19937 rand(rd());
    std::uniform_int_distribution<std::mt19937::result_type>
    uniform_dist(1, num_tasks);

    // Create and initialize a set of tasks
    std::list<Task> tasks;

    for (unsigned i = 1; i <= num_tasks; i++)
    {
        auto start_time = uniform_dist(rand);
        auto duration = uniform_dist(rand);

        tasks.push_back({i, start_time, start_time + duration });
    }

    return tasks;
}
```

- 3. 实现调度算法，如下所示：

```cpp
auto schedule(std::list<Task> tasks)
{
    // Sort the list of tasks by their end times
    tasks.sort([](const auto& lhs, const auto& rhs)
        { return lhs.end_time < rhs.end_time; });

    // Remove the tasks that interfere with one another
    for (auto curr_task = tasks.begin(); curr_task != tasks.end(); curr_task++)
    {
        // Point to the next task
        auto next_task = std::next(curr_task, 1);

        // While subsequent tasks interfere with the current task in iter
        while (next_task != tasks.end() &&
            next_task->start_time < curr_task->end_time)
        {
            next_task = tasks.erase(next_task);
        }
    }

    return tasks;
}
```

- 4. 以下工具函数用于打印任务列表、测试我们的实现，并包含程序的驱动代码：

```cpp
void print(std::list<Task>& tasks)
{
    std::cout << "Task ID \t Starting Time \t End time" << std::endl;

    for (auto t : tasks)
        std::cout << t.ID << "\t\t" << t.start_time << "\t\t" << t.end_time << std::endl;
}
```

```cpp
void test_interval_scheduling(unsigned num_tasks)
{
    auto tasks = initialize_tasks(num_tasks);

    std::cout << "Original list of tasks: " << std::endl;
    print(tasks);

    std::cout << "Scheduled tasks: " << std::endl;
    auto scheduled_tasks = schedule(tasks);
    print(scheduled_tasks);
}

int main()
{
    test_interval_scheduling(20);
    return 0;
}
```

### 活动12：Welsh-Powell 算法

在本活动中，我们将在图上实现 Welsh-Powell 算法。这里给出一个参考实现：

- 1. 添加所需的头文件，并声明稍后将实现的图：

```cpp
#include <unordered_map>
#include <set>
#include <map>
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>

template <typename T> class Graph;
```

- 2. 实现表示边的结构体，如下所示：

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
};
```

```cpp
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

// Overloads the << operator so a graph can be written directly to a stream
// Can be used as std::cout << obj << std::endl;
template <typename T>
friend std::ostream& operator<<(std::ostream& os, const Graph<T>& G);

private:
    size_t V;        // Stores number of vertices in graph
    std::vector<Edge<T>> edge_list;
};
```

```cpp
for (auto const i : degrees)
{
    if (assigned_colors[i.first] != 0)
        continue;

    auto outgoing_edges = G.outgoing_edges(i.first);
    std::set<size_t> neighbour_colors;

    // We assume that the graph is bidirectional
    for (auto e : outgoing_edges)
    {
        auto dest_color = assigned_colors[e.dest];
        neighbour_colors.insert(dest_color);
    }

    if (neighbour_colors.find(color_to_be_assigned) == neighbour_colors.end())
        assigned_colors[i.first] = color_to_be_assigned;
}

color_to_be_assigned++;

// If there are no uncolored vertices left, exit
if (std::find(assigned_colors.begin() + 1, assigned_colors.end(),
              0) == assigned_colors.end())
    break;
}

return assigned_colors;
}
```

- 7. 以下函数输出颜色向量：

```cpp
void print_colors(std::vector<size_t>& colors)
{
    for (auto i = 1; i < colors.size(); i++)
    {
        std::cout << i << ": " << color_map[colors[i]] << std::endl;
    }
}
```

- 8. 最后，以下驱动代码创建所需的图，运行顶点着色算法，并输出结果：

```cpp
int main()
{
    using T = unsigned;

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
    std::cout << "Original Graph" << std::endl;
    std::cout << G;

    auto colors = welsh_powell_coloring<T>(G);
    std::cout << "Vertex Colors: " << std::endl;
    print_colors(colors);
    return 0;
}
```
