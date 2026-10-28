```
default:
return false;
}
}
return stackBraces.empty();
}
```

The complete application is called StackBraces. Challenge yourself by implementing a similar problem but for multiple types of parentheses (for example, allow () {} [] in the same given string).

# Coding challenge 3 – Stack of plates

Amazon, Google, Adobe, Microsoft, Flipkart

Problem: Consider a stack of n plates. If the number of plates is bigger than n, then we need to arrange them in a new stack of n plates. So, each time the current stack exceeds the n capacity, a new stack of that capacity is created. Write a snippet of code that shapes these stacks so that they act as a single stack. In other words, the push () and pop () methods will work like there is a single stack. Additionally, write a popAt (int stackIndex) method that pops a value from the stack, as indicated via stackIndex.

Solution: We know how to deal with a single stack, but how do we link multiple stacks together? Well, since we have to link, how about a linked list? If the linked list contains a stack in each node, then the next pointer of a node will point to the next stack. The following diagram visualizes this solution:

![](bbox=[0.15, 0.9, 0.85, 0.97])

Figure 12.1 – Linked list of stacks