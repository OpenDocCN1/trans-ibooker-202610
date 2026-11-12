# ### 4.1.1.2 Stack

The stack is a data structure that is a special case of the list. With the stack, elements may be added to or removed from the top only. Adding an item to a stack is referred to as a push, and removing an item is referred to as a pop. In this scenario, the last item added must be the first one removed, which is also known as last in, first out (LIFO). A good analogy is to consider a stack of plates. Because the last item in is the first out, this data structure does not require a pointer. You can see the stack described in Figure 4.3.

![](bbox=[0.113, 0.275, 0.36, 0.385])

FIGURE 4.3 The stack

The problem with this particular data structure is the LIFO processing. If you have accumulated a number of data items and must get to one of the first you put on the stack, you will first need to process all of the subsequent items. For this reason, a stack is primarily used when a few items need to be stored for a short period of time. You will find stacks in temporary memory storage. It is probably worthwhile to compare LIFO with FIFO (first in, first out). The FIFO data structure requires that the first item put in also be the first one out. This is similar to the stack, but the popping and pushing take place on different ends. This is shown in Figure 4.4.

![](bbox=[0.113, 0.595, 0.36, 0.705])

FIGURE 4.4 FIFO

As was mentioned, certain computer memory structures and the CPU registers will often use stacks. However, their method of LIFO makes them poor choices for many standard data-processing situations. For example, if a network printer utilized a stack rather than a queue, it would always attempt to process the last job first. Because new jobs are continually being added, it would be entirely possible for the first job sent to the printer to wait hours, if not days, to be printed. Clearly, that would be unacceptable.