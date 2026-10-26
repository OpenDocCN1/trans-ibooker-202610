7. Use the following functions to implement the test and driver code that tests our implementation of the bipartite checking algorithm:

```
template <typename T>
void test_bipartite()
{
    // Create an instance of and print the graph
    auto BG = create_bipartite_reference_graph<T>();
    std::cout << BG << std::endl;

    if (bipartite_check<T>(BG))
        std::cout << "The graph is bipartite" << std::endl;
    else
        std::cout << "The graph is not bipartite" << std::endl;
}

int main()
{
    using T = unsigned;
    test_bipartite<T>();

    return 0;
}
```

8. Run the program. You should see the following output:

![](bbox=[0.055, 0.598, 0.938, 0.95])

Figure 6.34: Output of Activity 13