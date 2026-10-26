
5.  Now, we need a function to perform DFS for our graph. Implement it as follows:

    ```
    template <typename T>
    auto depth_first_search(const Graph<T>& G, size_t dest)
    {
        std::stack<size_t> stack;
        std::vector<size_t> visit_order;
        std::set<size_t> visited;
        stack.push(1); // Assume that DFS always starts from vertex ID 1

        while (!stack.empty())
        {
            auto current_vertex = stack.top();
            stack.pop();

            // If the current vertex hasn't been visited in the past
            if (visited.find(current_vertex) == visited.end())
            {
                visited.insert(current_vertex);
                visit_order.push_back(current_vertex);

                for (auto e : G.outgoing_edges(current_vertex))
                {
                    // If the vertex hasn't been visited, insert it in the stack.
                    if (visited.find(e.dest) == visited.end())
                    {
                        stack.push(e.dest);
                    }
                }
            }
        }

        return visit_order;
    }
    ```

6.  We shall test our implementation of the DFS on the graph shown here:

    ![](img/b37c7e7c5fa2518dbf1afd10524f8d11_282_0.png)

    Figure 6.13: Graph for implementing DFS traversal in Exercise 29

    Use the following function to create and return the graph:

    ```
    template <typename T>
    auto create_reference_graph()
    {
        Graph<T> G(9);

        std::map<unsigned, std::vector<std::pair<size_t, T>>> edges;
        edges[1] = { {2, 0}, {5, 0} };
        edges[2] = { {1, 0}, {5, 0}, {4, 0} };
        edges[3] = { {4, 0}, {7, 0} };
        edges[4] = { {2, 0}, {3, 0}, {5, 0}, {6, 0}, {8, 0} };
        edges[5] = { {1, 0}, {2, 0}, {4, 0}, {8, 0} };
        edges[6] = { {4, 0}, {7, 0}, {8, 0} };
        edges[7] = { {3, 0}, {6, 0} };
        edges[8] = { {4, 0}, {5, 0}, {6, 0} };

        for (auto& i : edges)
            for (auto& j : i.second)
                G.add_edge(Edge<T>{ i.first, j.first, j.second });

        return G;
    }
    ```

    Note the use of null values for edge weights since DFS does not require edge weights. A simpler implementation of the graph could have omitted the edge weights entirely without affecting the behavior of our DFS algorithm.

7.  Finally, add the following test and driver code, which runs our DFS implementation and prints the output:

    ```
    template <typename T>
    void test_DFS()
    {
        // Create an instance of and print the graph
        auto G = create_reference_graph<unsigned>();
        std::cout << G << std::endl;

        // Run DFS starting from vertex ID 1 and print the order
        // in which vertices are visited.
        std::cout << "DFS Order of vertices: " << std::endl;
        auto dfs_visit_order = depth_first_search(G, 1);
        for (auto v : dfs_visit_order)
            std::cout << v << std::endl;
    }

    int main()
    {
        using T = unsigned;
        test_DFS<T>();

        return 0;
    }
    ```

8.  Compile and run the preceding code. Your output should look as follows:

    ![](img/b37c7e7c5fa2518dbf1afd10524f8d11_283_0.png)

    Figure 6.14: Expected output of Exercise 29

The following figure shows the order in which the vertices were visited by our DFS implementation:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_284_0.png)

Figure 6.15: The order of vertices visited and the direction of DFS

The time complexity of both BFS and DFS is O(V + E). However, there are several important differences between the two algorithms. The following list summarizes the differences between the two and points out some cases where one should be preferred over the other:

- BFS is more suited to finding vertices that are closer to the source vertex, whereas DFS is often more suited to finding vertices that are farther away from the source.

- Once a vertex is visited in BFS, the path that's found from the source to the vertex is guaranteed to be the shortest path, while no such guarantees exist for DFS. This is the reason why all single-source and multiple-source shortest path algorithms use some variant of BFS. This shall be explored in the upcoming sections of this chapter.

- As BFS visits all the vertices adjacent to the current frontier, the search trees that are created by BFS are short and wide, and require comparatively more memory, whereas the search trees that are created by DFS are long and narrow, and require comparatively less memory.

## Activity 13: Finding out Whether a Graph is Bipartite Using DFS

A bipartite graph is one where the vertices can be divided into two sets so that any edges in the graph must connect a vertex from one set to a vertex from the other set.

Bipartite graphs can be used to model several different practical use cases. For instance, if we are given a list of students and a list of classes, the relationship between students and classes can be modeled as a bipartite graph containing an edge between a student and a class if the student is enrolled in that class. As you would imagine, edges leading from one student to another, or from one subject to another, would not make sense. Therefore, such edges are not allowed in a bipartite graph. The following figure illustrates such a model:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_285_0.png)

Figure 6.16: A sample bipartite graph representing student enrollment in different classes

Once a model such as the one shown here has been prepared, it can be used to create a schedule of classes so that no two classes that have been enrolled by the same student overlap. For example, if Jolene is enrolled in Math and Computer Science, these two classes should not be scheduled at the same time to avoid a conflict. Minimizing such conflicts in timetables can be achieved through solving a maximum flow problem in graphs. Several standard algorithms are known for the maximum flow problem: Ford-Fulkerson's, Dinic's, and the push-relabel algorithms are some examples. However, such algorithms are often complex and, therefore, beyond the scope of this book.

Another use case of modeling relationships between entities using a bipartite graph is between the viewers and the list of movies maintained by large video streaming platforms such as Netflix and YouTube.

An interesting property of bipartite graphs is that some operations such as finding a maximum matching and vertex cover, which are NP-complete for general graphs, can be solved in polynomial time for bipartite graphs. Therefore, it is useful to determine whether a given graph is bipartite or not. In this activity, you are required to implement a C++ program that checks whether a given graph, G, is bipartite.

The bipartite checking algorithm uses a slightly modified version of DFS and works as follows:

1.  Assume that the DFS starts with vertex 1. Add the vertex ID, 1, to the stack.
2.  If unvisited vertices remain on the stack, pop a vertex from the stack and set it as the current vertex.
3.  If the color that was assigned to the parent vertex was blue, assign the current vertex red; otherwise, assign the current vertex blue.
4.  Add all the unvisited adjacent vertices of the current vertex to the stack and mark the current vertex as visited.
5.  Repeat steps 2, 3, and 4 until all the vertices have been assigned a color. If all the vertices are colored when the algorithm terminates, the given graph is bipartite.
6.  If, while running step 2, the search encounters a vertex that has already been visited and assigned a color that is different from the color that it would have been assigned in step 3 (the inverse of the color assigned to its parent vertex in the search tree), the algorithm terminates immediately and the given graph is not bipartite.

The following figures illustrate the working of the preceding algorithm:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_287_0.png)

Figure 6.17: Initialization

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_287_1.png)

Figure 6.18: Since vertex 1 was assigned blue, we color vertex 2 red

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_288_0.png)

Set A　　　　　　　　　　　　　　Set B

Figure 6.19: Since vertex 2 was colored red, we color vertex 8 blue.

As can be observed from the preceding set of figures, the algorithm zigzags through the graph, assigning alternate colors to each vertex that's visited. If all the vertices can be colored this way, the graph is bipartite. If DFS reaches two vertices that have already been colored this way, the graph can be safely declared to be not bipartite.
