The working of the algorithm is illustrated in the following steps:

+   1. First, it initializes the labels on all the vertices and sets all the distances to infinity. Since the distance from the starting vertex to itself is 0, it sets the label of the starting vertex to 0. Then, it adds all the labels to a min-heap, H.

In the following diagram, the numbers shown in red represent the current best-known distances from the source (vertex 2) and the destination (vertex 6):

![](bbox=[0.07, 0.32, 0.92, 0.78])

Figure 6.27: Initializing Dijkstra's algorithm

+   2. Then, it pops a vertex, U, from H. Naturally, U is the vertex with the minimum distance from the starting vertex. If U is the required destination, we have found our shortest path and the algorithm terminates.