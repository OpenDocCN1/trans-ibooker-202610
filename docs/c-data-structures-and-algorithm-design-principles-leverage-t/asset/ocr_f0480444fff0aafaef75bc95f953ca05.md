# Exercise 5: Building a Basic Custom Container

In this exercise, we're going to implement an `std::forward_list` equivalent container with some improvements. We'll start with a basic implementation called `singly_ll`, and gradually keep on improving:

+   1. Let's add the required headers and then start with the basic implementation of `singly_ll` with a single node:

```
```cpp
#include <iostream>
#include <algorithm>

struct singly_ll_node
{
    int data;
    singly_ll_node* next;
};
```
```

+   2. Now, we'll implement the actual `singly_ll` class, which wraps the node around for better interfacing:

```
```cpp
class singly_ll
{
public:
    using node = singly_ll_node;
    using node_ptr = node*;

private:
    node_ptr head;
```
```

+   3. Now, let's add `push_front` and `pop_front`, just like in `forward_list`:

```
```cpp
public:
void push_front(int val)
{
    auto new_node = new node{val, NULL};
    if(head != NULL)
        new_node->next = head;
    head = new_node;
}

void pop_front()
{
    auto first = head;
    if(head)
```
```