Implementing a graph based on the adjacency list can be done as follows (here, we are using a Map to implement the adjacency list):

```
public class Graph<T> {

    // the adjacency list is represented as a map
    private final Map<T, List<T>> adjacencyList;

    public Graph() {
        this.adjacencyList = new HashMap<>();
    }

    // operations
}
```

Next, let's briefly cover the traversal of a graph.

# Graph traversal

The two most common ways to traverse a graph are via Depth-first Search (DFS) and Breadth-first Search (BFS). Let's have a rundown of each. BFS is mainly used for graphs.

In the case of graphs, we must consider that a graph may have cycles. A plain BFS implementation (as you saw in the case of binary trees) doesn't take cycles into account, so we risk an infinite loop while traversing the BFS queue. Eliminating this risk can be done via an additional collection that holds the visited nodes. The steps for this algorithm are as follows:

+   1. Mark the start node (current node) as visited (add it to the collection of visited nodes) and add it to the BFS queue.
+   2. Pop the current node from the queue.
+   3. Visit the current node.
+   4. Get the adjacent nodes of the current node.