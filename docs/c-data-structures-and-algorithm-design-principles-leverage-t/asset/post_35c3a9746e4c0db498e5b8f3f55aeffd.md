
![](img/b37c7e7c5fa2518dbf1afd10524f8d11_230_0.png)

**Figure 5.11:** Graph G representing the villages and distances between them

A simple greedy algorithm to construct the minimum spanning tree, T, could be as follows:

1. Add all the edges of G in a min-heap, H.
2. From H, pop an edge, e. Naturally, e has the minimum cost among all edges in H.
3. If both vertices of e are already in T, this means that adding e would create a cycle in T. Therefore, discard e and go to step 2. Otherwise, proceed to the next step.
4. Insert e in the minimum spanning tree, T.

Let's take a moment to think about why this strategy works. At each iteration of the loop in steps 2 and 3, we take the edge with the lowest cost and check whether it adds any vertex to our solution. This is stored in the minimum spanning tree, T. If it does, we add the edge to T; otherwise, we discard that edge and choose another edge with the minimum value. Our algorithm is greedy in the sense that at each iteration, it chooses the minimum edge weight to add to the solution. The preceding algorithm was invented in 1956 and is called Kruskal's minimum spanning tree algorithm. Applying this algorithm to the graph shown in figure 5.11 gives the following result:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_231_0.png)

The total weight of edges in the minimum spanning tree, T, is (2 × 1) + (3 × 2) + (2 × 3) = 14 units. Therefore, the answer to our problem is that at least 14 units of road would need to be built.

How do we know that our algorithm is indeed correct? We need to return to the definitions of optimal substructure and greedy choice and show that the MST problem exhibits these two properties. While a rigorous mathematical proof of the properties is beyond the ambit of this book, here are the intuitive ideas behind the proofs:

**Optimal substructure:** We will prove this by using contradiction. Let's assume that the MST problem does not exhibit an optimal substructure; that is, a minimum spanning tree was not composed of a set of smaller minimum spanning trees:

1. Let's say we are given a minimum spanning tree, T, over the vertices of graph G. Let's remove any edge, e, from T. Removing e decomposes T into smaller trees, T1 and T2.
2. Since we assumed that the MST problem does not exhibit optimal substructure, there must exist a spanning tree with a lesser total weight over the vertices of T1. Take this spanning tree and add the edge e and T2 to it. This new tree will be T′.
3. Now, since the total weight of T′ is less than that of T, this contradicts our original assumption that T is an MST. Therefore, the MST problem must exhibit the optimal substructure property.

**Greedy choice:** If the MST problem exhibits greedy choice property, then for a vertex, v, the minimum weight edge connecting v to the rest of the graph, G, should always be a part of the minimum spanning tree, T. We can prove this hypothesis by contradiction, as follows:

1. Say an edge (u, v) is the minimum weight edge connecting v to any other vertex in G. Assume that (u, v) is not a part of T.
2. If (u, v) is not a part of T, then T must consist of some other edge connecting v to the rest of G. Let this edge be (x, v). Since (u, v) is the minimum weight edge, by definition, the weight of (x, v) is greater than the weight of (u, v).
3. A tree with a lesser total weight than T can be obtained if (x, v) is replaced with (u, v) in T. This contradicts our assumption that T is the minimum spanning tree. Therefore, the MST problem must exhibit the greedy choice property.

> **Note**
>
> As we mentioned earlier, we can also take a rigorous mathematical approach to show that the MST problem exhibits the optimal substructure property and is suitable for the greedy choice property. You can find it here: https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-046j-design-and-analysis-of-algorithms-spring-2015/lecture-notes/MIT6_046JS15_lec12.pdf.

- **Find:** Starting from a given element, x, the find operation follows the parent pointers of elements until the root of the tree is reached. The parent of a root element is the root itself. In the example in the previous set, each element is the root of the tree, and hence this operation will return the lone element in the tree.
- **Union:** Given two elements, x and y, the union operation finds the roots of x and y. If the two roots are the same, this means that x and y belong to the same tree. Therefore, it does nothing. Otherwise, it sets the root with a higher rank as the parent of the root with a lower rank. The following figure shows the result of implementing the Union(1, 2) and Union(4, 5) operations on DS:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_234_0.png)

**Figure 5.14:** Merging 1, 2 and 4, 5

As subsequent union operations are applied, more trees merge into fewer (but larger) trees. The following figure shows the trees in DS after applying Union(2, 3):

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_234_1.png)

**Figure 5.15:** Merging 2, 3

The following diagram shows the trees in DS after applying **Union(2, 4)**:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_235_0.png)

**Figure 5.16:** Merging 2, 4

Now, let's understand how the disjoint-set data structure helps us implement Kruskal's algorithm. At the start of the algorithm, before step 1, we initialize a disjoint-set data structure with N equal to the number of vertices in our graph, G. Then, step 2 takes an edge from the min heap and step 3 checks whether the edge under consideration forms a cycle. Notice that this check for cycles can be implemented using the union operation on DS, which is applied to the two vertices of the edge. If the union operation succeeds in merging the two trees, then the edge is added to the MST; otherwise, the edge can safely be discarded as it would introduce a cycle in the MST. The following illustrated steps explain this logic:

1. First, we begin by initializing a disjoint-set data structure, DS, containing all of the given vertices in the graph:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_236_0.png)

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_236_1.png)

**Figure 5.17:** Step 1 of Kruskal's algorithm - initialization

2. Let's proceed to add the edge with the lowest weight to our MST. As you can see from the following figure, as we add edge (2, 4), we also apply Union(2, 4) to the elements in DS:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_237_0.png)

**Figure 5.18:** Adding edge (2, 4) to the MST after applying Union (2, 4) to the disjoint-set

3. As we proceed with adding edges as per the algorithm, we reach edge (1, 5). As you can see, in DS, the corresponding elements are in the same tree. Hence, we cannot add that edge. As you can see from the following graph, adding that would have created a cycle:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_238_0.png)

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_238_1.png)

**Figure 5.19:** Trying to add edge (1, 5) to MST fails because vertices 1 and 5 are in the same tree in DS

In the following exercise, we will implement Kruskal's minimum spanning tree algorithm using the disjoint-set data structure.

## Exercise 26: Kruskal's MST Algorithm

In this exercise, we will implement the disjoint-set data structure and Kruskal's algorithm to find an MST in the graph. Let's get started:

1. Begin by adding the following headers and declaring the Graph data structure:

```
#include<iostream>
#include<vector>
#include<algorithm>
#include<queue>
#include<map>
template <typename T> class Graph;
```

2. First, we will implement the disjoint set:

```
template<typename T>
class SimpleDisjointSet
{
private:
    struct Node
    {
        T data;

        Node(T _data) : data(_data)
        {}

        bool operator!=(const Node& n) const
        {
            return this->data != n.data;
        }
    };

    // Store the forest
    std::vector<Node> nodes;
    std::vector<size_t> parent;
    std::vector<size_t> rank;
```
