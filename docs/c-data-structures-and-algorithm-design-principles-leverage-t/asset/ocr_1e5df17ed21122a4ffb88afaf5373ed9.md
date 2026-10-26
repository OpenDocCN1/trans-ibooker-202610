```
G.add_edge(Edge<T>{ i.first, j.first, j.second });

std::cout << "Original Graph" << std::endl;
std::cout << G;

auto MST = minimum_spanning_tree(G);
std::cout << std::endl << "Minimum Spanning Tree" << std::endl;
std::cout << MST;

return 0;
}
```

10. Finally, run the program! Your output should look as follows:

![](bbox=[0.054, 0.361, 0.932, 0.711])

Verify that the output of our algorithm is indeed the MST that was shown in *figure 5.12*.

The complexity of Kruskal's algorithm without using the disjoint set is O(E log E), where E is the number of edges in the graph. With the disjoint set, however, the total complexity comes down to O(Eα(V)), where α(v) is the inverse of the Ackermann function. Since the inverse Ackermann function grows much slower than the logarithm function, the difference in the performance of the two implementations is small for graphs with a few vertices but can be notably large for larger graph instances.

Figure 5.20: Getting an MST from a given graph