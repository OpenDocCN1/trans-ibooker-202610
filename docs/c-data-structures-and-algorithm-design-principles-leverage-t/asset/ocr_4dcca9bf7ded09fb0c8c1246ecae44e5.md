# Chapter 1: Lists, Stacks, and Queues

# Activity 1: Implementing a Song Playlist

In this activity, we will implement a tweaked version of a doubly linked list which can be used to store a song playlist and supports the necessary functions. Follow these steps to complete the activity:

+   1. Let's first include the header and write the node structure with the required data members:

```
#include <iostream>

template <typename T>
struct cir_list_node
{
    T* data;
    cir_list_node *next, *prev;

    ~cir_list_node()
    {
        delete data;
    }
};

template <typename T>
struct cir_list
{
    public:
        using node = cir_list_node<T>;
        using node_ptr = node*;
    private:
        node_ptr head;
        size_t n;
```

+   2. Now, let's write a basic constructor and size function:

```
public:
cir_list(): n(0)
{
    head = new node{NULL, NULL, NULL};  // Dummy node - having NULL data
    head->next = head;
```