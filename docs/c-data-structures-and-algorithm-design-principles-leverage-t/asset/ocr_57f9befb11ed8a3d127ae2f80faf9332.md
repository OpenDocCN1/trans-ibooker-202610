As we can see from the first initialization, providing the size is not mandatory. If we don't specify the size explicitly, and if we don't infer it by specifying its elements, the vector is initialized with the capacity of elements depending on the compiler implementation. The term "size" refers to the number of elements actually present in the vector, which may differ from its capacity. So, for the first initialization, the size will be zero, but the capacity could be some small number or zero.

We can insert elements inside the vector using the push_back or insert functions. push_back will insert elements at the end. insert takes the iterator as the first parameter for the position, and it can be used to insert the element in any location. push_back is a very frequently used function for vectors because of its performance. The pseudocode of the algorithm for push_back would be as follows:

```
push_back(val):
    if size < capacity
        // If vector has enough space to accommodate this element
        - Set element after the current last element = val
        - Increment size
        - return;

    if vector is already full
        - Allocate memory of size 2*size
        - Copy/Move elements to newly allocated memory
        - Make original data point to new memory
        - Insert the element at the end
```

The actual implementation might differ a bit, but the logic remains the same. As we can see, if there's enough space, it only takes O(1) time to insert something at the back. However, if there's not enough space, it will have to copy/move all the elements, which will take O(n) time. Most of the implementations double the size of the vector every time we run out of capacity. Hence, the O(n) time operation is done after n elements. So, on average, it just takes one extra step, making its average time complexity closer to O(1). This, in practice, provides pretty good performance, and, hence, it is a highly used container.

For the insert function, you don't have any option other than to shift the elements that come after the given iterator to the right. The insert function does that for us. It also takes care of reallocation whenever it is required. Due to the need to shift the elements, it takes O(n) time. The following examples demonstrate how to implement vector insertion functions.