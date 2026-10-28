Polling an element from a heap is also an O(log n) time operation. After we've polled the root element of the heap, we have to fix the heap so that it respects the heap property. This operation is known as heapify-down. The algorithm behind heapify-down has three steps:

+   1. Start from the root of the heap as the current node.
+   2. Determine the largest node between the children of the current node.
+   3. If the current node is less than its largest children, then swap these two nodes and repeat from step 2; otherwise, there is nothing else to do, so stop.

Finally, peeking is an O(1) operation that returns the root element of the heap.

In the code bundled with this book, you can find an application called MaxHeap that exposes the following set of methods: add (T element), peek (), and poll ().

> Important note
> 
> A special case of a tree is known as a Trie. Also known as a digital tree or a prefix tree, a Trie is an ordered tree structure used commonly for storing strings. Its name comes from the fact that Trie is a reT rieval data structure. Its performance is better than a binary tree. Trie is detailed in my book, Java Coding Problems (https://www.packtpub.com/programming/java-coding-problems), next to other data structures such as tuples, disjoint-set, binary indexed trees (Fenwick trees), and Bloom filters.

# Graphs in a nutshell

A graph is a data structure that's used to represent a collection of nodes that can be connected with edges. For example, a graph can be used to represent a network of members on a social media platform, so it is the great data structure for representing real-life connections. In other words, a tree (as detailed in the previous section) is a particular type of graph. In graph terms, a graph without cycles is called an acyclic graph.

+   • Vertex represents the information (for example, a member, a dog, or a value)
+   • Edge is the connection or the relationship between two vertices