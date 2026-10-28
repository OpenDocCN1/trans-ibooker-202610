**Solution:** Commonly, social networks are designed using graphs. The result is a huge graph such as those in the next figure (this figure was gathered via Google Image via the *social network graph* keywords):

![](bbox=[0.12, 0.195, 0.86, 0.365])

Figure 19.5 – Social network graph

So, finding a path between two people means finding a path in such a graph. In this case, the problem reduces to how to efficiently find a path between two nodes in such a huge graph.

We can start with one person and traverse the graph to find the other person. Traversing a graph can be done using **BFS (Breadth-first Search)** or **DFS (Depth-first Search)**. For more details about these algorithms, check out *Chapter 13, Trees and Graphs.*

DFS will be very inefficient! Two persons might be only one degree of separation apart, but DFS may traverse millions of nodes (persons) before finding this relatively immediate connection.

Hence, the winner is BFS. More precisely, we can employ bidirectional BFS. Like two trains that come from opposite directions and intersect at some moment in time, we use one BFS that starts from person A (the source) and one BFS that starts from person B (the destination). When the searches collide, we have found a path between A and B.

Why not unidirectional BFS? Because going from A to B will traverse p+p*p people. Mainly, unidirectional BFS will traverse A’s p friends, and then each of their p friends. This means that for a path of length q, the unidirectional BFS will perform in O(p^q) runtime. On the other hand, the bidirectional BFS traverses 2p nodes: each of A’s p friends and each of B’s p friends. This means that for a path of length q, the bidirectional BFS performs in O(p^{q/2}+p^{q/2}) = O(p^{q/2}). Obviously, O(p^{q/2}) is better than O(p^q).

Let’s consider a path such as Ana -> Bob -> Carla -> Dan -> Elvira, where each person has 100 friends. A unidirectional BFS will traverse 100 million (100^4) nodes. A bidirectional BFS will traverse only 20,000 nodes (2 x 100^2).