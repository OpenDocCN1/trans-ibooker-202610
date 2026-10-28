```
```text
while (size() > 1) {
    int l1 = poll();
    int l2 = poll();

    totalLength += (l1 + l2);

    add(l1 + l2);
}

return totalLength;
}
```
```

The complete application is called *HeapConnectRopes*.

# Advanced topics

Right from the start, you should know that the following topics are rarely encountered in technical interviews. First, let me enumerate these topics as a non-exhaustive list:

+   - AVL trees (a brief description and an implementation are available in the code bundled with this book)
+   - Red-Black trees (a brief description and an implementation are available in the code bundled with this book)
+   - Dijkstra's algorithm
+   - Rabin-Karp substring search
+   - The Bellman-Ford algorithm
+   - The Floyd-Warshall algorithm
+   - Interval trees
+   - Minimum spanning trees
+   - B-trees
+   - Bipartite graph
+   - Graph coloring
+   - P, NP, and NP-complete