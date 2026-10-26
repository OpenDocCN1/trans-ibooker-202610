6. We shall test our implementation of the DFS on the graph shown here:

![](bbox=[0.22, 0.15, 0.78, 0.52])

Figure 6.13: Graph for implementing DFS traversal in Exercise 29

Use the following function to create and return the graph:

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

Note the use of null values for edge weights since DFS does not require edge weights. A simpler implementation of the graph could have omitted the edge weights entirely without affecting the behavior of our DFS algorithm.