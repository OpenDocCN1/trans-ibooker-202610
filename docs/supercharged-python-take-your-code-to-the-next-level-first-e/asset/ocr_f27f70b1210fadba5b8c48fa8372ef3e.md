replaced by the `append` and `pop` methods of a Python list.

The key change that needs to be made—conceptually, at any rate—is to think of operating on the last element to be added to the end of the list, rather than to the literal top of a stack.

This end-of-the-list approach is functionally equivalent to a stack. Figure 3.7 illustrates 10 and 20 being pushed on, and then popped off, a list used as a stack. The result is that the items are popped off in reverse order.

![](bbox=[0.1631, 0.3297, 0.8135, 0.6625])

**Figure 3.7.** Stack operation with a Python list

One of the most useful demonstrations of a stack device is an interpreter for the Reverse Polish Notation (RPN) language. We develop a sophisticated language interpreter by the end of this book, but for now we start with a simple calculator.

The RPN language evaluates operators in a _postfix_ language, in which two expressions are followed by an operator. Most languages use an _infix_ notation. In postfix, the operands appear