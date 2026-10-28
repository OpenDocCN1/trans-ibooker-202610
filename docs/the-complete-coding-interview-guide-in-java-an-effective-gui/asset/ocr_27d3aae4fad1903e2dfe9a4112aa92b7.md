# Balanced and unbalanced binary trees

When a binary tree guarantees O(log n) times for insert and find operations, we can say that we have a balanced binary tree, but one that's not necessarily as balanced as it could be. When the difference between the heights of the left sub-tree and the right sub-tree for any node in the tree is no more than 1, then the tree is height-balanced. In the following diagram, the left-hand side tree is an unbalanced binary tree, the middle side tree is a balanced binary tree but not height-balanced, and the right-hand side tree is a height-balanced tree:

![](bbox=[0.05, 0.38, 0.95, 0.58])

Figure 13.5 – Unbalanced binary tree versus balanced binary tree versus height-balanced binary tree

There are two types of balanced trees: Red-Black trees and AVL trees.

# Red-Black tree

A Red-Black tree is a self-balancing BST where each node is under the incident of the following rules:

+   - Every node is either red or black
+   - The root node is always black
+   - Every leaf (NULL) is black
+   - Both children of a red node are black
+   - Every path from a node to a NULL node has the same number of black nodes

The following diagram represents a Red-Black tree: