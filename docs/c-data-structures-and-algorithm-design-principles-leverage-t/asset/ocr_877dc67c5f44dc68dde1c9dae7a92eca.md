```
}

// Overloads the << operator so a graph be written directly to a stream
// Can be used as std::cout << obj << std::endl;
template <typename T>
friend std::ostream& operator<< >(std::ostream& os, const Graph<T>& G);
```

```
private:
    size_t V;      // Stores number of vertices in graph
    std::vector<Edge<T>> edge_list;
};
```

5. The following hash map stores the list of colors that will be used by our coloring algorithm:

```
// Initialize the colors that will be used to color the vertices
std::unordered_map<size_t, std::string> color_map = {
    {1, "Red"},
    {2, "Blue"},
    {3, "Green"},
    {4, "Yellow"},
    {5, "Black"},
    {6, "White"}
};
```

6. Next, let's implement a helper function that prints the colors that have been assigned to each vertex:

```
void print_colors(std::vector<size_t>& colors)
{
    for (auto i=1; i<colors.size(); i++)
    {
        std::cout << i << ": " << color_map[colors[i]] << std::endl;
    }
}
```