The following section provides a summary of the comparison of contiguous and linked data structures.

# Comparison

The following table briefly summarizes the important differences between linked and contiguous data structures in general:

| Contiguous Data Structures | Linked Data Structures |
|---|---|
| All the data is stored next to one another in memory. | Data is stored in nodes, which may be scattered across the memory. |
| Accessing random element is immediate due to contiguous data. | Accessing random elements is linear and, hence, slower. |
| Since data is stored contiguously, traversal is faster due to cache locality. | Traversal is a bit slower as there is no cache locality. |
| No memory overhead on top of the memory required for storing the elements themselves. | Extra memory is required to store pointers in each node. |

Figure 1.4: Table comparing contiguous and linked data structures

The following table contains a summary of the performance of arrays and linked lists regarding various parameters:

| Parameter | Array | Linked List |
|---|---:|---:|
| Random access | O(1) | O(n) |
| Insertion at end | O(1) | O(1) |
| Insertion in the middle | O(n) | O(1) |
| Cache locality | Yes | No |

Figure 1.5: Table showing time complexities of some operations for arrays and linked lists

For any application, we can choose either data structure or a combination of both, based on the requirements and the frequencies of the different operations.

Arrays and linked lists are very common and are extensively used in any application to store data. Hence, the implementation of these data structures must be as bug-free and as efficient as possible. To avoid reinventing the code, C++ provides various structures, such as std::array, std::vector, and std::list. We will see some of them in more detail in upcoming sections.