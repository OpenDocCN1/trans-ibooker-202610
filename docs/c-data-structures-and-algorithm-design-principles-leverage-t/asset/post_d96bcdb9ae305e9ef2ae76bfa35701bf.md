
## Chapter 5: Greedy Algorithms

### Activity 11: The Interval Scheduling Problem

In this activity, we will find the optimal scheduling of tasks to maximize the number of tasks that can be completed. Follow these steps to complete the activity:

- 1. Add the required header files and define the Task struct as follows:

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

- 2. The following function can be used to generate a list of N tasks with random data:

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

- 3. Implement the scheduling algorithm as follows:

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

- 4. The following utility functions are used to print the list of tasks, test our implementation, and include the driver code for the program:

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

### Activity 12: The Welsh-Powell Algorithm

We will implement the Welsh-Powell algorithm on the graph in this activity. A reference implementation is given here:

- 1. Add the required header files and declare the graph that will be implemented later:

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

- 2. Implement the struct, representing edges like so:

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

- 7. The following function outputs the vector of colors:

```cpp
void print_colors(std::vector<size_t>& colors)
{
    for (auto i = 1; i < colors.size(); i++)
    {
        std::cout << i << ": " << color_map[colors[i]] << std::endl;
    }
}
```

- 8. Finally, the following driver code creates the required graph, runs the vertex coloring algorithm, and outputs the results:

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

## Chapter 6: Graph Algorithms I

### Activity 13: Finding out Whether a Graph is Bipartite Using DFS

In this activity, we will check whether a graph is bipartite using depth-first search traversal. Follow these steps to complete the activity:

- 1. Add the required header files and declare the graph to be used:

```cpp
#include <string>
#include <vector>
#include <iostream>
#include <set>
#include <map>
#include <stack>

template<typename T> class Graph;
```

- 2. Write the following struct to define an edge in our graph:

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

- 3. Use the following function to overload the << operator for the graph so that it can be written to standard output:

```cpp
template <typename T>
std::ostream& operator<<(std::ostream& os, const Graph<T>& G)
{
    for (auto i = 1; i < G.vertices(); i++)
    {
        os << i << "\t";

        auto edges = G.outgoing_edges(i);
        for (auto& e : edges)
            os << "{" << e.dest << ": " << e.weight << "}, ";

        os << std::endl;
    }

    return os;
}
```

- 4. Implement the edge list graph as follows:

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
};
```
