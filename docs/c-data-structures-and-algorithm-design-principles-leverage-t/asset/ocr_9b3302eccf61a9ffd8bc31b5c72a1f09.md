Thus, we start by comparing the root node with 7. If it is greater than 7, we move to the left subtree, since all the elements there are smaller than the parent node, and vice versa. We compare each child node until we stumble upon 7, or a node less than 7 with no right node. In this case, coming to node 4 leads to our target, 7.

As we can see, we're not traversing the whole tree. Instead, we are reducing our scope by half every time the current node is not the desired one, which we do by choosing either the left or the right side. This works similar to a binary search for linear structures, which we will learn about in Chapter 4, *Divide and Conquer*.

# Inserting a New Element into a BST

Now, let's look at how insertion works. The steps are shown in the following figure:

![](bbox=[0.299, 0.331, 0.683, 0.785])

As you can see, first, we have to find the parent node where we want to insert the new value. Thus, we have to take a similar approach to the one we took for searching; that is, by going in the direction based on comparing each node with our new element, starting with the root node. At the last step, 18 is greater than 17, but 17 doesn't have a right child. Therefore, we insert 18 in that position.