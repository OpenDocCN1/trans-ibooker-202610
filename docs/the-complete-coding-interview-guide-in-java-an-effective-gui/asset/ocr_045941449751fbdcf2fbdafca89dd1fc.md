In order to save space, a bit-matrix can be used as well.

In the case of weighted graphs, the adjacency matrix can store the weight of the edge, while 0 can be used to indicate the absence of the edge.

Implementing a graph based on the adjacency matrix can be done as follows (all we need is the list of vertices since the edges are passed to each method that has to traverse the graph as the adjacency matrix):

```
public class Graph<T> {
	// the vertices list
	private final List<T> elements;
	public Graph() {
		this.elements = new ArrayList<>();
	}
	// operations
}
```

Another approach we can use to represent a graph in a computer is the adjacency list.

# Adjacency list

An adjacency list is an array of lists whose size is equal to the number of vertices in the graph. Every vertex is stored in this array and it stores a list of adjacent vertices. In other words, the list at index i of the array contains the adjacent vertices of the vertex stored in the array at index i. The following diagram shows an example of an adjacency list for an undirected graph:

![](bbox=[0.05, 0.58, 0.95, 0.98])