

{
    stringarray[head] = item; head++;
}
string dequeue()
{
    return stringarray[tail]; tail++;
}
}

With a queue, data is added and removed in a sequential fashion. This is very different from a list, where data may be added at any point in the list. With the queue, data is always added at the next spot in the sequence and processed similarly; however, two problems immediately present themselves with the queue. The first problem is how to handle the condition of reaching the end of the queue. The usual answer to this is to create what is known as a circular queue. When the head (or tail) reaches the end of the queue, it simply starts over at the beginning. Referring to Figure 4.2, this would mean that if the head or tail reaches the end, it is simply repositioned back to the beginning. In code, you would add to the previous code sample something similar to this pseudo-code:

```
if (head==20) head = 0;
```

and

```
if (tail == 20) tail = 0;
```

This still leaves us with the second problem: what happens if the head is adding new items faster than the tail is processing them? Without proper coding, the head will overtake the tail, and you will begin overwriting items in the queue that have not been processed; ergo, they are lost and will never be processed. This means you will be adding items to a space in the queue that already contains unprocessed data, and that unprocessed data will simply be lost. The answer to this problem is to stop allowing new items to be added whenever the head catches up to the tail. This should be communicated to the end user via a “queue is full” message.

Of course, there is another option, other than the circular queue, and that is the bounded queue. A bounded queue is simply one that can only contain a finite amount of data. When that limit is reached (i.e., when you reach the end of the queue), the queue is done. Clearly, this implementation of the queue is somewhat limited, and you will encounter it much less frequently than the unbounded queue (i.e., any queue that does not have an arbitrary, finite limit, such as the circular queue).

The queue is a very efficient data structure, and you will find it implemented in many diverse situations. The most common place to encounter a queue is with a printer. Printers usually have a queue in which print jobs are stored while awaiting processing. And, of course, they utilize circular queues. If you overload that queue, you will get a message telling you the print queue is full. If your programming problem requires orderly and sequential processing of data on a first-in, first-out basis, then the queue is an ideal data structure to use.

### 4.1.1.2 Stack

The stack is a data structure that is a special case of the list. With the stack, elements may be added to or removed from the top only. Adding an item to a stack is referred to as a push, and removing an item is referred to as a pop. In this scenario, the last item added must be the first one removed, which is also known as last in, first out (LIFO). A good analogy is to consider a stack of plates. Because the last item in is the first out, this data structure does not require a pointer. You can see the stack described in Figure 4.3.

![](img/49061be7fe11d9f7f3e56114ab15acdc_103_0.png)

FIGURE 4.3 The stack

The problem with this particular data structure is the LIFO processing. If you have accumulated a number of data items and must get to one of the first you put on the stack, you will first need to process all of the subsequent items. For this reason, a stack is primarily used when a few items need to be stored for a short period of time. You will find stacks in temporary memory storage. It is probably worthwhile to compare LIFO with FIFO (first in, first out). The FIFO data structure requires that the first item put in also be the first one out. This is similar to the stack, but the popping and pushing take place on different ends. This is shown in Figure 4.4.

![](img/49061be7fe11d9f7f3e56114ab15acdc_103_1.png)

FIGURE 4.4 FIFO

As was mentioned, certain computer memory structures and the CPU registers will often use stacks. However, their method of LIFO makes them poor choices for many standard data-processing situations. For example, if a network printer utilized a stack rather than a queue, it would always attempt to process the last job first. Because new jobs are continually being added, it would be entirely possible for the first job sent to the printer to wait hours, if not days, to be printed. Clearly, that would be unacceptable.

Here is code for the stack with an entire class for all the operations of the stack:

```cpp
class MyStack
{
void push(int data) {
if(!isFull()) {
top = top + 1;
stack[top] = data;
} else {
printf("Could not insert data, Stack is full.\n");
}
}
int pop(int data) {
if(!isempty()) {
data = stack[top];
top = top - 1;
return data;
} else {
printf("Could not retrieve data, Stack is empty.\n");
}
}
bool isfull() {
if(top == MAXSIZE)
return true;
else
return false;
}
int peek() {
return stack[top];
}
}
```

### 4.1.1.3 Linked List

A linked list is a data structure wherein each item has a pointer (or link) to the next item in the list or the item preceding it, but not both (a double-linked list, which will be discussed later in this chapter, does both). This can be quite useful because if an item is displaced for any reason, it is aware of the next item in the list. Each item knows what comes after it, or what comes before it, but not both.

There is a problem with the linked list. If you do not implement it in such a way that data is processed in an orderly, sequential fashion (such as FIFO), then items can be removed or added anywhere (as with a standard list). This means that when an item is inserted or deleted, you must adjust the pointer of the item it was inserted next to. For example, consider the diagram shown in Figure 4.5.

![](img/49061be7fe11d9f7f3e56114ab15acdc_105_0.png)

FIGURE 4.5 The linked list

Each of the items (B*, C*, etc. ) in the boxes is a pointer to the next item in the linked list. The * notation is the same the C and C++ languages use to denote pointers. Note that when item X is inserted between item A and item B, item A's pointer must also be changed so that it now points to X rather than to B. The same situation occurs if an item is deleted. This problem is minimized, however, if items are always added or removed in a sequential fashion, such as FIFO.

### 4.1.1.4 Double-Linked List

A double-linked list is a data structure wherein each item has a pointer (or link) to the item in front of it and the item behind it. This data structure is the logical next step after the linked list. It is highly efficient in that any data point has links to the preceding and succeeding data points. The source code is virtually identical to that of a linked list. It simply has two pointers: one for the preceding item and one for the succeeding item. This sort of data structure is highly efficient.

Note there is also a special form of the double-linked list called a circularly linked list, where the first and last nodes are linked together. This can be done for both single- and double-linked lists. To traverse a circularly linked list, you begin at any node and follow the list in either direction until you return to the original node. Viewed another way, circularly linked lists can be seen as having no beginning or end. This type of list is very useful for managing buffers.

The circularly linked version of the double-linked list makes it ideal for the printer buffer. It allows the printer buffer to have every document aware of the documents on either side. This is because it has a double-linked list (each item has a pointer to the previous item and the next item) and its final item is linked back to the first item. This type of structure provides an unbroken chain of items.

Whatever method you use to implement the double-linked list (circular, etc.), it has the same complication as the linked list. When you insert or delete an item, you must update any other items that have pointers to the item you deleted or the space you are inserting the item into. The only difference between a double-linked list and a single-linked list is that when you insert or delete an item in a double-linked list, you must update the pointers on either side of that item, rather than only one pointer (either preceding or next). A double-linked list is shown in Figure 4.6.

![](img/49061be7fe11d9f7f3e56114ab15acdc_105_1.png)

FIGURE 4.6 The double-linked list

