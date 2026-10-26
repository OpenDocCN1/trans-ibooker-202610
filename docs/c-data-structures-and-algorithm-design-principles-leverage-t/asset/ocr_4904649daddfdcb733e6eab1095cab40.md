A static array is aggregated, which means that it is allocated on the stack, and hence gets deallocated when the flow goes out of the function. On the other hand, a dynamic array is allocated on a heap and stays there until the memory is freed manually.

Since all the elements are present next to each other, when one of the elements is accessed, a few elements next to it are also brought into the cache. Hence, if you want to access those elements, it is a very fast operation as the data is already present in the cache. This property is also known as cache locality. Although it doesn’t affect the asymptotic time complexity of any operations, while traversing an array, it can give an impressive advantage for contiguous data in practice. Since traversing requires going through all the elements sequentially, after fetching the first element, the next few elements can be retrieved directly from the cache. Hence, the array is said to have good cache locality.

# Linked Data Structures

Linked data structures hold the data in multiple chunks of memory, also known as nodes, which may be placed at different places in the memory. The following diagram shows how data is stored in linked data structures:

![](bbox=[0.033, 0.475, 0.941, 0.517])

**Figure 1.2: Linked data structures**

In the basic structure of a linked list, each node contains the data to be stored in that node and a pointer to the next node. The last node contains a NULL pointer to indicate the end of the list. To reach any element, we must start from the beginning of the linked list, that is, the head, and then follow the next pointer until we reach the intended element. So, to reach the element present at index i, we need to traverse through the linked list and iterate i times. Hence, we can say that the complexity of accessing elements is O(n); that is, the time varies proportionally with the number of nodes.