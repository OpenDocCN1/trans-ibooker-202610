# Deleting an Element from a BST

Now, let's look at how deletion works. Consider the following BST:

![](bbox=[0.35, 0.18, 0.7, 0.45])

Figure 2.7: Binary search tree rooted at 12

We will delete the root node, 12, in the tree. Let's look at how we can delete any value. It's a bit trickier than insertion since we need to find the replacement of the deleted node so that the properties of the BST remain true.

The first step is to find the node to be deleted. After that, there are three possibilities:

+   - The node has no children: simply delete the node.
+   - The node has only one child: point the parent node's corresponding pointer to the only existing child.
+   - The node has two children: in this case, we replace the current node with its successor.

The successor is the next biggest number after the current node. Or, in other words, the successor is the smallest element among all the elements greater than the current one. Therefore, we'll first go to the right subtree, which contains all the elements greater than the current one, and find the smallest among them. Finding the smallest node means going to the left side of the subtree as much as we can because the left child node is always less than its parent. In the tree shown in figure 2.7, the right subtree of 12 starts at 18. So, we start looking from there, and then try to move down to the left child of 15. But 15 does not have a left child, and the other child, 16, is larger than 15. Hence, 15 should be the successor here.