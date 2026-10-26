9. Our program should output the following:

```
DISTANCE FROM VERTEX 0:
    0: 0
    1: 3
    2: 6
    3: 13
    4: 8
```

As we can see, Bellman-Ford avoids the trap that would lead Dijkstra's algorithm to evaluate the shortest paths incorrectly. However, there is still another significant problem to contend with, which we will discuss in the next section.

# The Bellman-Ford Algorithm (Part II) – Negative Weight Cycles

Consider the graph shown in the following figure:

![](bbox=[0.217, 0.405, 0.767, 0.689])

Figure 7.2: Graph with a negative weight cycle

The edges highlighted in red indicate a negative weight cycle or a cycle in the graph where the combined edge weights produce a negative sum. In such a situation, this cycle would be considered repeatedly, and the final results would be skewed.

For the sake of comparison, consider a graph with only positive edge weights. A cycle in such a graph would never be considered in the solution because the shortest distance to the first node in the cycle would have been found already. To demonstrate this, imagine that the edge weight between nodes B and D in the preceding figure is positive. Starting from node A, the first iteration through the edges would determine that the shortest distance to node B is equal to 3. After two more iterations, we would also know the shortest distance from A to C (A → B → D → C), which is equal to 14 (3 + 8 + 3).