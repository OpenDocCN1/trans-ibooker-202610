# Bidirectional Iterators

In the Iterators section, we saw the difference between the flexibility of array-based random access iterators and forward_list-based forward iterators. The flexibility of std::list::iterator lies between both of them. It is more flexible compared to forward iterators, since it can allow us to traverse backward. Hence, std::list also supports functions for reverse traversal by exposing reverse iterators where the operations are inverted. Having said that, it is not as flexible as random access iterators. Although we can advance in either direction by any number of moves, since these moves have to be done by traversing the elements one by one instead of jumping directly to the desired element, the time complexity is still linear, and not a constant, as in the case of random access iterators. Since these iterators can move in either direction, they are known as bidirectional iterators.

# Iterator Invalidation for Different Containers

So far, we've seen that iterators provide us with a uniform way of accessing, traversing, inserting, and deleting elements from any container. But there are some cases when iterators become invalid after modifying the container, because the iterators are implemented based on pointers, which are bound to memory addresses. So, if the memory address of any node or element changes because of modification in the container, it invalidates the iterator, and using it regardless can lead to undefined behavior.

For example, a very basic example would be vector::push_back, which simply adds a new element at the end. However, as we saw earlier, in some cases, it also requires the movement of all the elements to a new buffer. Hence, all iterators, pointers, and even the references to any of the existing elements will be invalidated. Similarly, if the vector::insert function leads to reallocation, all the elements will need to be moved. Hence, all the iterators, pointers, and references are invalidated. If not, the function will invalidate all the iterators pointing to the element that is on the right side of the insertion position, since these elements will be shifted during the process.

Unlike vectors, linked list-based iterators are safer for insertion and deletion operations because the elements will not be shifted or moved. Hence, none of the insertion functions for std::list or forward_list affect the validity of the iterators. An exception is that deletion-related operations invalidate iterators of the elements that are deleted, which is obvious and reasonable. It doesn't affect the validity of the iterators of the rest of the elements. The following example shows iterator invalidation for different iterators:

```
std::vector<int> vec = {1, 2, 3, 4, 5};
```

```
auto it4 = vec.begin() + 4;
```