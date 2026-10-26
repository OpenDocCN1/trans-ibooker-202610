# Working with the Stack Data Structure

Consider a scenario wherein a developer has been asked to develop his/her own stack structure that can take in any parameter. Here, the requirement is that the stack structure must follow the Last In First Out (LIFO) principle, where elements are placed on top of each other and when they're removed from the stack, the last element should be removed first. It should have the following functions:

+   - **push()** to place a new element on top of the stack
+   - **top()** to display the top element of the stack, if any
+   - **pop()** to remove the last inserted element from the stack
+   - **is_empty()** to check if the stack is empty
+   - **size()** to display the number of elements present in the stack
+   - **clean()** to empty the stack if it has any elements in it

The following lines of code show how to include the necessary libraries within the **Stack.h** header file:

```
#ifndef STACK_H__
#define STACK_H__
#include<iostream>
using namespace std;
```

As we already know, the stack consists of various operations. To define each of these functions, we'll write the following code:

```
template<typename T>
struct Node{
    T element;
    Node<T> *next;
};

template<typename T>
class Stack{
    Node<T> *head;
    int sz;

public :
```