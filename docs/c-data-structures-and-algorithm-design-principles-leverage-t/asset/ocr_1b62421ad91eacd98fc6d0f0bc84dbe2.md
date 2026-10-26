7. Finally, add the following test and driver code, which runs our DFS implementation and prints the output:

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

8. Compile and run the preceding code. Your output should look as follows:

![](bbox=[0.09, 0.68, 0.97, 0.95])

Figure 6.14: Expected output of Exercise 29