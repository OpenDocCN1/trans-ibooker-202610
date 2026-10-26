3. As we proceed with adding edges as per the algorithm, we reach edge (1,5). As you can see, in DS, the corresponding elements are in the same tree. Hence, we cannot add that edge. As you can see from the following graph, adding that would have created a cycle:

![](bbox=[0.15, 0.2, 0.85, 0.6])

![](bbox=[0.22, 0.6, 0.85, 0.82])

Figure 5.19: Trying to add edge (1, 5) to MST fails because vertices 1 and 5 are in the same tree in DS

In the following exercise, we will implement Kruskal's minimum spanning tree algorithm using the disjoint-set data structure.