```
```cpp
return edges_from_v;
}

// Overloads the << operator so a graph be written directly to a
// stream
// Can be used as std::cout << obj << std::endl;
template <typename T>
friend std::ostream& operator<< <>(std::ostream& os, const Graph<T>&
G);

private:
    size_t V;              // Stores number of vertices in graph
    std::vector<Edge<T>> edge_list;
};
```
```

+   5. Write a function to create the reference graph shown in *figure 6.28* using the **Graph** class, as shown here:

```
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
```