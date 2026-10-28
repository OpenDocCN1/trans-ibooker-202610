Computer scientist J. W. J. Williams originally invented heaps as a component of a new sorting algorithm, heapsort, which we’ll discuss later in the chapter. However, he recognized that heaps are useful data structures for other tasks. The max heap’s simple structure allows it to efficiently support the operations required for priority queues: (1) allowing a user to efficiently look up the largest element, (2) removing the largest element, and (3) adding an arbitrary element.

Heaps are often visualized as trees but are often implemented with arrays for efficiency. In this chapter, we present these two representations in parallel to allow the reader to make mental connections between them. However, it is not required that we use arrays for implementation.

In the array-based implementation, each element in the array corresponds to a node in the tree with the root node at index 1 (we skip index 0 to stay consistent with common convention for heaps). Child node indexes are defined relative to the indexes of their parents, so a node at index i has children at indexes 2i and 2i + 1. For example, the node at index 2 has a child at index 2 × 2 = 4 and index 2 × 2 + 1 = 5, as shown in Figure 7-4.

![](bbox=[0.22, 0.42, 0.38, 0.55])

Figure 7-4: The heap’s position within the binary tree corresponds to index locations in an array.

Similarly, we compute the index of a node’s parent as Floor(i/2). The indexing scheme allows the algorithm to trivially compute the index of a child based on that of the parent, and the index of a parent based on its child.

The root node always corresponds to the maximum value in a max heap. Since we store the root node in a fixed spot in the array (index = 1), we can always find this maximum value in constant time. It’s just an array lookup. The layout of the data itself thus addresses one of the necessary priority queue operations.

Since we’ll be adding and removing arbitrary elements in our priority queue, in order to avoid constantly resizing the array, we want to preallocate an array that is large enough to accommodate the number of items we expect to add. Remember from Chapter 3 that dynamically resizing an array can be expensive, forcing us to create a new array and copy over the values, and in this case wasting the precious efficiency of the heap. Instead, we can initially allocate a large array, track the index of the last filled element of the array, and call that index the virtual end of the array. This allows us to append new elements by simply updating the index for the last element.