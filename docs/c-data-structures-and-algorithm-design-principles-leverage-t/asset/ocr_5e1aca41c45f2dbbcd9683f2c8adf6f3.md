# ## Binary Search Tree

A **binary search tree (BST)** is a popular version of the binary tree. BST is nothing but a binary tree with the following properties:

+   - Value of the parent node ≥ value of the left child
+   - Value of the parent node ≤ value of the right child

In short, left child ≤ parent ≤ right child.

This leads us to an interesting feature. At any point in time, we can always say that all the elements that are less than or equal to the parent node will be on the left side, while those greater than or equal to the parent node will be on the right side. So, the problem of searching an element keeps on reducing by half, in terms of search space, at each step.

If the BST is constructed in a way that all the elements except those at the last level have both children, the height of the tree will be log n, where n is the number of elements. Due to this, the searching and insertion will have a time complexity of O(log n). This type of binary tree is also known as a **complete binary tree**.

# ### Searching in a BST

Let's look at how we can search, insert, and delete elements in a binary search tree. Consider a BST with unique positive integers, as shown in the following figure:

![](bbox=[0.345, 0.581, 0.702, 0.768])

*Figure 2.5: Searching for an element in a binary search tree*

Let's say that we have to search for 7. As we can see from the steps represented by arrows in the preceding figure, we choose the side after comparing the value with the current node's data. As we've already mentioned, all the nodes on the left will always be less than the current node, and all the nodes on the right will always be greater than the current node.